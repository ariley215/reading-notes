# Develop for Azure Cache for Redis

- Explain the key scenarios Azure Cache for Redis covers and its service tiers.
- Identify the key parameters for creating an Azure Cache for Redis instance and interact with the cache.
- Connect an app to Azure Cache for Redis by using .NET Core.

## Explore Azure Cache for Redis

Azure Cache for Redis provides an in-memory data store based on the Redis software. 

Redis improves the performance and scalability of applications that heavily use backend data stores.

- It processes large volumes of application requests by keeping frequently accessed data in server memory
    - Enabling quick read and write operations.

Redis offers a critical low-latency and high-throughput data storage solution for modern applications.

### Key Scenarios

Azure Cache for Redis supports common application architecture patterns to improve performance:

- **Data Cache:**
  - Databases are often too large to load directly into a cache.
  - Uses the cache-aside pattern to load data into the cache as needed.
  - Updates the cache when the system changes the data, distributing updates to other clients.

- **Content Cache:**
  - Many web pages are generated from templates with static content (e.g., headers, footers, banners).
  - Static items are cached for quick access, reducing the need to fetch from backend datastores.

- **Session Store:**
  - Commonly used with shopping carts and user history data in web applications.
  - Uses cookies as keys to query data in a database.
  - In-memory cache provides faster data retrieval than a full relational database.

- **Job and Message Queuing:**
  - Tasks are added to a queue for operations that take time to execute.
  - Queued tasks are processed in sequence by another server, known as task queuing.

- **Distributed Transactions:**
  - Executes a series of commands against a backend data store as a single atomic operation.
  - Ensures all commands succeed or all are rolled back to the initial state.

### Service Tiers

Azure Cache for Redis is available in the following tiers:

- **Basic:**
  - An OSS Redis cache running on a single VM.
  - No service-level agreement (SLA).
  - Ideal for development/test and noncritical workloads.

- **Standard:**
  - An OSS Redis cache running on two VMs in a replicated configuration.

- **Premium:**
  - High-performance OSS Redis caches with higher throughput and lower latency.
  - Offers better availability and more features.
  - Deployed on more powerful VMs compared to Basic or Standard caches.

- **Enterprise:**
  - High-performance caches powered by Redis Labs' Redis Enterprise software.
  - Supports Redis modules including RediSearch, RedisBloom, and RedisTimeSeries.
  - Offers higher availability than the Premium tier.

- **Enterprise Flash:**
  - Cost-effective large caches powered by Redis Labs' Redis Enterprise software.
  - Extends Redis data storage to nonvolatile memory, reducing overall per-GB memory cost.

For a detailed comparison of each tier, refer to the [Azure Cache for Redis Pricing](https://azure.microsoft.com/en-us/pricing/details/cache/).

## Configure Azure Cache for Redis

You can create a Redis cache using the Azure portal, the Azure CLI, or Azure PowerShell. Here are the steps and considerations for configuring your Redis cache instance properly.

### Create and Configure an Azure Cache for Redis Instance

#### Name

- **Requirement:** The Redis cache needs a globally unique name.
- **Format:**
  - Must be between 1 and 63 characters.
  - Can include numbers, letters, and the '-' character.
  - Cannot start or end with the '-' character.
  - Consecutive '-' characters are not valid.

#### Location

- **Best Practice:** Place your cache instance and application in the same region.
- **Cross-Region Consideration:** Connecting to a cache in a different region can increase latency and reduce reliability.
- **External Connection:** Select a location close to where the application consuming the data is running if connecting outside of Azure.

#### Cache Type

- **Determination:** The tier determines the size, performance, and features available.
- **Recommendation:** Use Standard tier or higher for production systems.
  - **Basic Tier:** Single node system, no data replication, no SLA.

#### Clustering Support

- **Availability:** Supported in Premium, Enterprise, and Enterprise Flash tiers.
- **Implementation:** Specify the number of shards (up to 10) to split the dataset among multiple nodes.
- **Cost:** Calculated as the cost of the original node multiplied by the number of shards.

### Accessing the Redis Instance

- **Command-Line Tool:** Redis has a command-line tool for interacting with an Azure Cache for Redis as a client.
  - **Windows:** Download the Redis command-line tools for Windows.
  - **Other Platforms:** Download from [Redis.io](https://redis.io/download).

- **Common Commands:**

  | Command              | Description                                                       |
  |----------------------|-------------------------------------------------------------------|
  | `ping`               | Ping the server. Returns "PONG".                                  |
  | `set [key] [value]`  | Sets a key/value in the cache. Returns "OK" on success.           |
  | `get [key]`          | Gets a value from the cache.                                      |
  | `exists [key]`       | Returns '1' if the key exists in the cache, '0' if it doesn't.    |
  | `type [key]`         | Returns the type associated with the value for the given key.     |
  | `incr [key]`         | Increment the given value associated with the key by '1'.         |
  | `incrby [key] [amount]` | Increment the given value associated with the key by the specified amount. |
  | `del [key]`          | Deletes the value associated with the key.                        |
  | `flushdb`            | Delete all keys and values in the database.                       |

  Example:

  ```plaintext
  > set somekey somevalue
  OK
  > get somekey
  "somevalue"
  > exists somekey
  (string) 1
  > del somekey
  (string) 1
  > exists somekey
  (string) 0
  ```

### Adding an Expiration Time to Values

- **Purpose:** Expire values when they're stale by applying a time to live (TTL) to a key.
- **Behavior:** When the TTL elapses, the key is automatically deleted.
- **Precision:**
  - Expirations can be set using seconds or milliseconds precision.
  - The expire time resolution is always 1 millisecond.
- **Persistence:** Information about expires is replicated and persisted on disk.

Example:

```plaintext
> set counter 100
OK
> expire counter 5
(integer) 1
> get counter
100
... wait ...
> get counter
(nil)
```

### Accessing a Redis Cache from a Client

- **Requirements:** Host name, port, and an access key.
- **Retrieval:** Retrieve this information in the Azure portal through the Settings > Access Keys page.
- **Host Name:** The public Internet address of your cache (e.g., sportsresults.redis.cache.windows.net).
- **Access Key:**
  - Acts as a password for your cache.
  - Two keys (primary and secondary) are provided.
  - Periodically regenerate the keys for security.
- **Warning:** Your access keys should be treated as confidential information. Anyone with an access key can perform any operation on your cache.

## Interact with Azure Cache for Redis by Using .NET

A client application typically uses a client library to form requests and execute commands on a Redis cache. You can get a list of client libraries directly from the Redis clients page.

### Executing Commands on the Redis Cache

A popular high-performance Redis client for the .NET language is StackExchange.Redis. The package is available through NuGet and can be added to your .NET code using the command line or IDE. Following are examples of how to use the client.

### Connecting to Your Redis Cache with StackExchange.Redis

Use the host address, port number, and an access key to connect to a Redis server. Azure also offers a connection string that bundles this data into a single string:

```plaintext
[cache-name].redis.cache.windows.net:6380,password=[password-here],ssl=True,abortConnect=False
```

Parameters:

- `ssl`: Ensures communication is encrypted.
- `abortConnection`: Allows a connection to be created even if the server is unavailable.

#### Creating a Connection

The main connection object in StackExchange.Redis is the `ConnectionMultiplexer` class. It abstracts the process of connecting to a Redis server and is optimized to manage connections efficiently.

Create a `ConnectionMultiplexer` instance using the `ConnectionMultiplexer.Connect` or `ConnectionMultiplexer.ConnectAsync` method:

```csharp
using StackExchange.Redis;
...
var connectionString = "[cache-name].redis.cache.windows.net:6380,password=[password-here],ssl=True,abortConnect=False";
var redisConnection = ConnectionMultiplexer.Connect(connectionString);
```

#### Accessing a Redis Database

The Redis database is represented by the `IDatabase` type. Retrieve one using the `GetDatabase()` method:

```csharp
IDatabase db = redisConnection.GetDatabase();
```

Tip: The object returned from `GetDatabase` is lightweight and does not need to be stored. Only the `ConnectionMultiplexer` needs to be kept alive.

#### Storing and Retrieving Values

Store a key/value in the cache:

```csharp
bool wasSet = db.StringSet("favorite:flavor", "i-love-rocky-road");
```

Retrieve the value with the `StringGet` method:

```csharp
string value = db.StringGet("favorite:flavor");
Console.WriteLine(value); // displays: "i-love-rocky-road"
```

#### Working with Binary Values

Redis keys and values are binary safe. Use the same methods to store binary data:

```csharp
byte[] key = ...;
byte[] value = ...;

db.StringSet(key, value);

byte[] retrievedValue = db.StringGet(key);
```

#### Other Common Operations

The `IDatabase` interface includes methods to work with hashes, lists, sets, and ordered sets. Here are some common methods:

| Method | Description |
| --- | --- |
| `CreateBatch` | Creates a group of operations sent to the server as a single unit. |
| `CreateTransaction` | Creates a group of operations processed on the server as a single unit. |
| `KeyDelete` | Deletes the key/value. |
| `KeyExists` | Returns whether the given key exists in the cache. |
| `KeyExpire` | Sets a time-to-live (TTL) expiration on a key. |
| `KeyRename` | Renames a key. |
| `KeyTimeToLive` | Returns the TTL for a key. |
| `KeyType` | Returns the type of the value stored at the key (e.g., string, list, set, zset). |

#### Executing Other Commands

Use the `Execute` and `ExecuteAsync` methods to pass textual commands to the Redis server:

```csharp
var result = db.Execute("ping");
Console.WriteLine(result.ToString()); // displays: "PONG"

var clientList = await db.ExecuteAsync("client", "list");
Console.WriteLine($"Type = {clientList.Type}\r\nResult = {clientList}");
```

#### Storing Complex Values

Cache object graphs by serializing them to a textual format like JSON. Use the Newtonsoft.Json library:

```csharp
public class GameStat
{
    // class definition
}

var stat = new GameStat("Soccer", new DateTime(2019, 7, 16), "Local Game", 
                new[] { "Team 1", "Team 2" },
                new[] { ("Team 1", 2), ("Team 2", 1) });

string serializedValue = Newtonsoft.Json.JsonConvert.SerializeObject(stat);
bool added = db.StringSet("event:1950-world-cup", serializedValue);

var result = db.StringGet("event:2019-local-game");
var deserializedStat = Newtonsoft.Json.JsonConvert.DeserializeObject<GameStat>(result.ToString());
Console.WriteLine(deserializedStat.Sport); // displays "Soccer"
```

### Cleaning Up the Connection

Dispose of the `ConnectionMultiplexer` to close all connections and shut down communication to the server:

```csharp
redisConnection.Dispose();
redisConnection = null;
```
