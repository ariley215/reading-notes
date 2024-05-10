# Discover Azure Message Queues

Objectives:

- Choose the appropriate queue mechanism for your solution.
- Explain how the messaging entities that form the core capabilities of Service Bus operate.
- Send and receive message from a Service Bus queue by using .NET.
- Identify the key components of Azure Queue Storage
- Create queues and manage messages in Azure Queue Storage by using .NET.

## Azure Queue Mechanisms

Azure supports two primary types of queue mechanisms, each serving distinct purposes:

### 1. **Service Bus Queues**

- **Overview:** Part of Azure's broader messaging infrastructure, which includes capabilities for queuing, publish/subscribe, and advanced integration patterns.
- **Use Case:** Ideal for integrating applications or components across different communication protocols, data contracts, trust domains, or network environments.
  - They offer robust messaging features to facilitate complex communications.

### 2. **Storage Queues**

- **Overview:** Integrated within the Azure Storage infrastructure, designed to store a large number of messages.
- **Details:**
  - **Message Size:** Each message can be up to 64 KB in size.
  - **Capacity:** A queue can contain millions of messages, subject to the total capacity limit of the storage account.
- **Use Case:** Typically used to create a backlog of work to be processed asynchronously, accessible globally via authenticated HTTP or HTTPS calls.

These queues provide scalable solutions for different operational needs.
 
*Service Bus queues* supporting more complex messaging patterns and *Storage queues* offering massive storage capabilities.

## Choosing a Message Queue Solution in Azure

When selecting a queuing solution in Azure, it's crucial to understand the different features and capabilities of Service Bus queues and Storage queues. Here's a guide to help decide which one, or a combination of both, might fit your solution's needs.

### Considerations for Using Service Bus Queues

Service Bus queues are recommended under the following circumstances:

- **Non-Polling Message Reception:** Ideal if your solution requires receiving messages without constant polling. Service Bus supports long-polling receive operations via TCP-based protocols.
- **Order Guarantee:** Use Service Bus queues when your application needs guaranteed first-in-first-out (FIFO) message delivery.
- **Duplicate Detection:** Automatically detects duplicate messages, which is crucial for applications where message uniqueness is a requirement.
- **Parallel Processing with Sessions:** Supports processing messages in parallel long-running streams, using the session ID property to associate messages with streams. This is beneficial for applications that process messages in isolated sessions.
- **Transactional Support:** Provides transactional behavior and atomicity in operations, critical when sending or receiving multiple messages as a single atomic operation.
- **Large Message Support:** Suitable for handling messages that are larger than 64 KB but do not exceed 256 KB.

### Considerations for Using Storage Queues

Storage queues are more appropriate in scenarios such as:

- **High Volume Storage:** If your application needs to store more than 80 gigabytes of messages in a queue, Storage queues are more suitable due to their high capacity.
- **Progress Tracking:** Allows for tracking the progress of message processing, which is useful if a processing worker fails. This enables another worker to pick up where the last one left off.
- **Transaction Logging:** Provides server-side logs of all transactions executed against your queues, which is valuable for auditing and troubleshooting purposes.

## Explore Azure Service Bus

Azure Service Bus is a fully managed enterprise integration message broker designed to decouple applications and services by transferring data using messages. This capability enhances the scalability and reliability of applications by ensuring that components do not need to be online simultaneously.

### Common Messaging Scenarios

- **Messaging:** Transfers business data such as sales orders, purchase orders, journals, or inventory movements.
- **Decoupling Applications:** Improves system reliability and scalability, facilitating independent operation of client and service.
- **Topics and Subscriptions:** Supports one-to-many relationships between publishers and subscribers.
- **Message Sessions:** Facilitates workflows that require message ordering or message deferral.

### Service Bus Tiers

Azure Service Bus is available in two tiers, each designed to cater to different use cases:

| Feature             | Premium                                   | Standard                              |
|---------------------|-------------------------------------------|---------------------------------------|
| Throughput          | High throughput                           | Variable throughput                   |
| Performance         | Predictable performance                   | Variable latency                      |
| Pricing             | Fixed pricing                             | Pay as you go variable pricing        |
| Scalability         | Ability to scale workload up and down     | N/A                                   |
| Message Size        | Up to 100 MB                              | Up to 256 KB                          |

The premium tier is recommended for mission-critical applications due to its high performance and scalability.

### Advanced Features

Azure Service Bus includes several advanced features to solve complex messaging problems:

| Feature             | Description |
|---------------------|-------------|
| Message Sessions    | Ensures first-in, first-out (FIFO) delivery with ordered handling of messages. |
| Autoforwarding      | Chains a queue or subscription to another in the same namespace. |
| Dead-letter Queue   | Stores undeliverable messages for later inspection and processing. |
| Scheduled Delivery  | Allows messages to be delivered at a specified future time. |
| Message Deferral    | Defers the retrieval of a message to a later time. |
| Batching            | Delays sending a message to batch multiple messages together. |
| Transactions        | Supports atomic grouped operations within a single execution scope. |
| Filtering and Actions | Allows subscribers to define rules on which messages to receive. |
| Autodelete on Idle  | Automatically deletes a queue or topic after a specified idle period. |
| Duplicate Detection | Identifies and removes duplicate messages in the queue. |
| Security Protocols  | Includes support for SAS, RBAC, and Managed Identities. |
| Geo-disaster Recovery | Enables continuity of operations across different regions during outages. |
| Security            | Supports AMQP 1.0 and HTTP/REST protocols for secure messaging. |

### Compliance with Standards and Protocols

Service Bus is compliant with the Advanced Messaging Queueing Protocol (AMQP) 1.0, which is beneficial for interoperability with both Azure-based and on-premises messaging systems. 

The platform is also fully compliant with Java/Jakarta EE Java Message Service (JMS) 2.0 API, enhancing its utility in enterprise applications.

### Client Libraries

Azure SDK provides fully supported client libraries for interacting with Service Bus across multiple programming languages:

- **Azure Service Bus for .NET**
- **Azure Service Bus libraries for Java**
- **Azure Service Bus provider for Java JMS 2.0**
- **Azure Service Bus Modules for JavaScript and TypeScript**
- **Azure Service Bus libraries for Python**

These libraries facilitate the development of applications that integrate seamlessly with Azure Service Bus, leveraging its full range of messaging capabilities.

## Discover Service Bus Queues, Topics, and Subscriptions

Azure Service Bus is a robust messaging system that supports a variety of patterns and approaches to ensure flexible message handling in distributed applications. 

This section covers the primary entities: queues, topics, subscriptions, and rules/actions.

### Queues

Queues in Azure Service Bus support First In, First Out (FIFO) message delivery and are used primarily to decouple message producers from consumers. Here are the key features and functionalities of Service Bus queues:

- **Durability:** Messages are stored durably, allowing asynchronous processing by producers and consumers.
- **Load-Leveling:** Queues help in managing varying loads over time by allowing producers to send messages at a different rate than consumers retrieve them.
- **Loose Coupling:** Producers and consumers operate without awareness of each other, simplifying system upgrades and maintenance.
- **Receive Modes:** Service Bus supports two modes:
  - **Receive and Delete:** Messages are immediately marked as consumed upon retrieval.
  - **Peek Lock:** Messages are locked during processing and only marked as consumed after the consumer acknowledges completion. If processing is not confirmed, the message becomes available again.

### Topics and Subscriptions

Unlike queues, topics and subscriptions support a publish-subscribe pattern, allowing messages to be broadcast to multiple recipients:

- **One-to-Many Communication:** Publishers send messages to a topic, and each subscription to that topic can receive a copy of the message.
- **Flexibility:** Subscriptions can filter messages so that only relevant messages are received, based on predefined rules and actions.

### Rules and Actions

To further refine message processing, subscriptions can use rules and actions to filter and modify incoming messages:

- **Filtering:** Subscriptions can define SQL filter expressions that operate on message properties, allowing only messages that match certain criteria to be passed to the consumer.
- **Actions:** Modifications to message properties can be defined, allowing for dynamic changes to messages as they are processed by subscriptions.

### Creation and Management

Queues, topics, and subscriptions can be created and managed using the Azure portal, PowerShell, CLI, or Resource Manager templates. 

Messages can be sent and received using clients written in popular programming languages such as C#, Java, Python, and JavaScript.

### Practical Usage

- **Scenarios for Queues:** Best for simple point-to-point communication where each message is processed by a single consumer.
- **Scenarios for Topics and Subscriptions:** Ideal for scenarios where messages need to be consumed by multiple applications or when complex filtering and routing of messages are required.

## Explore Service Bus Message Payloads and Serialization

Azure Service Bus supports robust messaging capabilities, handling not only the transportation of messages but also providing advanced features for message routing, serialization, and properties management.

### Message Structure

Service Bus messages consist of:

- **Payload:** A binary section that carries the data of the message. Service Bus does not manipulate this data, ensuring the integrity of the message contents.
- **Metadata:** Comprises two types of properties:
  - **Broker Properties:** System-defined properties that control message-level functionality or map to standardized metadata items.
  - **User Properties:** Key-value pairs defined and set by the application to include application-specific information.

### Message Routing and Correlation

Service Bus utilizes several broker properties to facilitate sophisticated routing and correlation scenarios:

- **Simple Request/Reply:** Involves a single publisher and consumer. The `ReplyTo` property of the outbound message specifies where replies should be sent, and the `CorrelationId` links replies to the original message.
- **Multicast Request/Reply:** Extends the simple request/reply pattern to multiple subscribers, each potentially responding to messages sent to a topic.
- **Multiplexing:** Uses the `SessionId` to route streams of related messages through a single queue or subscription, allowing related messages to be processed together.
- **Multiplexed Request/Reply:** Allows multiple publishers to share a reply queue. The `ReplyToSessionId` directs replies to the specific session waiting for responses.

### Routing Techniques

- **Autoforward Chaining and Topic Subscription Rules:** Utilized within a Service Bus namespace to automate the forwarding of messages based on rules.
- **Cross-Namespace Routing:** Can be achieved using Azure LogicApps for complex workflows that span multiple Service Bus namespaces.

### Payload Serialization

- **Binary Format:** The payload is always treated as an opaque binary block to maintain the data’s integrity and confidentiality.
- **ContentType Property:** Describes the payload's format using MIME content-type descriptions, enhancing interoperability and understanding of the payload structure.
- **Serialization Protocols:**
  - **.NET Framework:** Supports creating `BrokeredMessage` instances with arbitrary .NET objects, utilizing binary serialization.
  - **AMQP Protocol:** Encodes objects into an AMQP-specific format, which can be decoded using the `GetBody<T>()` method by specifying the expected type.

### Best Practices for Serialization

While Service Bus provides mechanisms for automatic serialization, it is recommended that applications:

- **Control Serialization:** Explicitly manage the serialization and deserialization processes to ensure compatibility and optimize performance across different platforms and services.
- **Manage AMQP Encodings:** Handle AMQP encoded messages carefully, as they are designed specifically for AMQP clients and might be challenging for HTTP clients to decode.

## Explore Azure Queue Storage

Azure Queue Storage allows for storing a vast number of messages, which can be accessed globally through authenticated HTTP or HTTPS calls. 

It is particularly useful for managing large backlogs of work that need to be processed asynchronously.

### Key Components

The Azure Queue Storage service is composed of the following elements:

- **Storage Account:** The gateway for all access to Azure Storage, including queues. Each storage account can contain multiple queues.
- **Queue:** A specific queue within the storage account that holds messages. Each queue is identified by a unique name that must be in all lowercase.
- **Message:** Individual messages within the queue that can be up to 64 KB in size. Messages can be in any format and have a configurable time-to-live.

![Queue service components](https://learn.microsoft.com/en-us/training/wwl-azure/discover-azure-message-queue/media/queue-storage-service-components.png)

### URL Format and Access

Queues are addressable using a specific URL format, which allows for straightforward access and integration with other services:

```
https://<storage account>.queue.core.windows.net/<queue>
```

For example, a queue named 'images-to-download' in a storage account named 'myaccount' would be accessed via:

```
https://myaccount.queue.core.windows.net/images-to-download
```

### Message Properties

- **Size Limit:** Each message can be up to 64 KB.
- **Time-to-Live:** Starting with version 2017-07-29, the maximum time-to-live for a message can be any positive number or -1, indicating that the message does not expire. If not specified, the default is seven days.

### Use Cases

- **Asynchronous Task Processing:** Queue Storage is commonly used to decouple application components. Messages representing tasks to be processed are enqueued and can be processed independently by different components.
- **Load Leveling:** By using queues, applications can manage varying loads efficiently. This is achieved by storing excessive demands in the queue, allowing application components to process messages at a steady rate.

## Manage Azure Queue Storage Using .NET

This section explores how to work with Azure Queue Storage using .NET, focusing on queue management and message handling. The .NET Azure SDK provides intuitive client libraries that facilitate these operations.

### Necessary NuGet Packages

To work with Azure Queue Storage in a .NET project, include the following NuGet packages:

- **Azure.Core:** Provides shared primitives, abstractions, and helpers for the Azure SDK client libraries.
- **Azure.Storage.Queues:** Enables interaction with Azure Queue Storage.
- **System.Configuration.ConfigurationManager:** Allows access to configuration files.

### Creating a Queue Service Client

Begin by creating an instance of the `QueueClient` class. This client will be used to interact with the queue:

```csharp
// Instantiate a QueueClient
QueueClient queueClient = new QueueClient(connectionString, queueName);
```

### Creating a Queue

Ensure the queue exists before attempting to interact with it:

```csharp
// Get the connection string from app settings
string connectionString = ConfigurationManager.AppSettings["StorageConnectionString"];
QueueClient queueClient = new QueueClient(connectionString, queueName);

// Create the queue if it doesn't exist
queueClient.CreateIfNotExists();
```

### Inserting a Message into a Queue

Insert messages into the queue using the `SendMessage` method:

```csharp
if (queueClient.Exists())
{
    // Send a message to the queue
    queueClient.SendMessage(message);
}
```

### Peeking at Messages

Peek at messages without removing them from the queue:

```csharp
if (queueClient.Exists())
{
    // Peek at the next message
    PeekedMessage[] peekedMessage = queueClient.PeekMessages();
}
```

### Changing the Contents of a Queued Message

Modify the contents of a message in the queue:

```csharp
if (queueClient.Exists())
{
    QueueMessage[] message = queueClient.ReceiveMessages();

    // Update the message contents
    queueClient.UpdateMessage(message[0].MessageId, 
                              message[0].PopReceipt, 
                              "Updated contents",
                              TimeSpan.FromSeconds(60.0));  // Extend visibility timeout
}
```

### Dequeuing Messages

Process and remove a message from the queue:

```csharp
if (queueClient.Exists())
{
    QueueMessage[] retrievedMessage = queueClient.ReceiveMessages();
    Console.WriteLine($"Dequeued message: '{retrievedMessage[0].Body}'");

    // Delete the message after processing
    queueClient.DeleteMessage(retrievedMessage[0].MessageId, retrievedMessage[0].PopReceipt);
}
```

### Getting the Queue Length

Determine the number of messages in the queue:

```csharp
if (queueClient.Exists())
{
    QueueProperties properties = queueClient.GetProperties();
    int cachedMessagesCount = properties.ApproximateMessagesCount;

    Console.WriteLine($"Number of messages in queue: {cachedMessagesCount}");
}
```

### Deleting a Queue

Remove a queue and all its messages:

```csharp
if (queueClient.Exists())
{
    queueClient.Delete();
}
```
