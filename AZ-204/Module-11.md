# Work with Azure Cosmos DB

Objectives:

- Identify classes and methods used to create resources
- Create resources by using the Azure Cosmos DB .NET v3 SDK
- Write stored procedures, triggers, and user-defined functions by using JavaScript
- Implement change feed notifications

## Exploring the Azure Cosmos DB .NET SDK v3

This page covers the key aspects of the Azure Cosmos DB .NET SDK v3, which is used to interact with the Azure Cosmos DB service.

### About the .NET SDK v3

- The .NET SDK v3 is represented by the `Microsoft.Azure.Cosmos` NuGet package.
- It provides a modern, asynchronous, and object-oriented API for working with Azure Cosmos DB.
- The SDK supports multiple API models, including SQL (Core), MongoDB, Cassandra, and more.

### Key Concepts

- **CosmosClient**: The main entry point for interacting with Azure Cosmos DB. Maintains efficient connection management.
- **Database**: A logical container for organizing and managing data.
- **Container**: The fundamental unit of scalability, can be a collection, graph, or table.
- **Item**: The content stored within a container, such as documents, edges/vertices, or rows.

### Common Operations

The .NET SDK v3 provides methods for performing CRUD (create, read, update, delete) operations on Cosmos DB resources:

1. **Database Operations**:
   - Create a database: `client.CreateDatabaseIfNotExistsAsync()`
   - Read a database: `database.ReadAsync()`
   - Delete a database: `database.DeleteAsync()`

2. **Container Operations**:
   - Create a container: `database.CreateContainerIfNotExistsAsync()`
     - Requires a container ID and partition key path
   - Get a container by ID: `database.GetContainer(containerID)`
   - Delete a container: `database.GetContainer(containerID).DeleteContainerAsync()`

3. **Item Operations**:
   - Create an item: `container.CreateItemAsync()`
     - Requires a JSON serializable object with an "id" property
     - Must include the partition key value
   - Read an item: `container.ReadItemAsync<T>(id, new PartitionKey(partitionKeyValue))`
     - Requires the item's unique ID and partition key value
   - Query items: `container.GetItemQueryIterator<T>()`
     - Allows querying items using SQL-like syntax
     - Can specify partition key value to scope the query

The SDK also provides advanced features like change feed processing, spatial queries, and much more. Refer to the [Azure Cosmos DB .NET SDK v3 GitHub repository](https://github.com/Azure/azure-cosmos-dotnet-v3) and samples for more details.

Code Requirements:

- All examples use the asynchronous versions of the SDK methods to leverage the benefits of async/await.
- The `CosmosClient` instance is created once and reused throughout the application for efficient connection management.
- Appropriate error handling and retry logic are implemented to ensure resilient and reliable interactions with Azure Cosmos DB.
- The SDK supports multiple API models, so the examples use generic terms like "container" and "item" to be applicable across different API types.

Here's an updated study guide page focused on creating stored procedures in Azure Cosmos DB, following the criteria for a good study guide:

## Creating Stored Procedures in Azure Cosmos DB

Azure Cosmos DB allows you to write and execute server-side logic using stored procedures, triggers, and user-defined functions (UDFs). Stored procedures, in particular, provide a way to perform complex operations on data stored within your Cosmos DB containers.

### Key Concepts

- **Stored Procedures**: Server-side JavaScript functions that can create, read, update, and delete items within a Cosmos DB container.
- **Transactional Execution**: Stored procedures are executed in a transactional manner, with the ability to either fully commit or fully rollback changes.
- **Continuation-based Model**: Stored procedures can implement a continuation-based model to batch or resume execution if needed, due to the bounded execution time.

### Writing Stored Procedures

1. **Registration**: Stored procedures are registered at the container level and can operate on any document or attachment in that container.
2. **Context Object**: The `context` object provides access to all operations that can be performed in Cosmos DB, as well as the request and response objects.
3. **Example**: Here's a simple "Hello World" stored procedure:

   ```javascript
   var helloWorldStoredProc = {
       id: "helloWorld",
       serverScript: function () {
           var context = getContext();
           var response = context.getResponse();
           response.setBody("Hello, World");
       }
   }
   ```

### Creating Items using a Stored Procedure

1. **Asynchronous Operation**: When creating an item using a stored procedure, the operation is asynchronous and depends on JavaScript callback functions.
2. **Callback Function**: The callback function has two parameters: one for the error object (in case of failure) and another for the created object.
3. **Example**: This stored procedure creates a new document and returns the ID of the newly created item:

   ```javascript
   var createDocumentStoredProc = {
       id: "createMyDocument",
       body: function createMyDocument(documentToCreate) {
           var context = getContext();
           var collection = context.getCollection();
           var accepted = collection.createDocument(collection.getSelfLink(),
                 documentToCreate,
                 function (err, documentCreated) {
                     if (err) throw new Error('Error' + err.message);
                     context.getResponse().setBody(documentCreated.id)
                 });
           if (!accepted) return;
       }
   }
   ```

### Passing Arrays as Input Parameters

- Input parameters to stored procedures are always sent as strings, even if you pass an array.
- To work around this, you can define a function within your stored procedure to parse the string as an array:

  ```javascript
  function sample(arr) {
      if (typeof arr === "string") arr = JSON.parse(arr);
      arr.forEach(function(a) {
          // do something here
          console.log(a);
      });
  }
  ```

### Bounded Execution and Transactions

- All Cosmos DB operations, including stored procedures, have a limited amount of time to execute.
- Stored procedures can implement a continuation-based model to batch or resume execution if needed.
- Stored procedures also support transactions, allowing you to implement complex, multi-item operations that either fully commit or fully rollback.

For more information on working with stored procedures, triggers, and UDFs in Azure Cosmos DB, refer to the [official documentation](https://docs.microsoft.com/en-us/azure/cosmos-db/how-to-write-stored-procedures-triggers-udfs).

## Creating Triggers and User-Defined Functions in Azure Cosmos DB

Azure Cosmos DB supports two types of triggers - pre-triggers and post-triggers - as well as user-defined functions (UDFs) that can be used to extend the functionality of your Cosmos DB applications.

### Triggers

Triggers in Cosmos DB are server-side scripts that can be executed before (pre-trigger) or after (post-trigger) specific database operations, such as create, update, or delete.

#### Pre-Triggers

Pre-triggers are executed before a database item is modified. They can be used to validate the properties of an item before it is created or updated. For example, the following pre-trigger adds a timestamp property to a new item if it doesn't already exist:

```javascript
function validateToDoItemTimestamp() {
    var context = getContext();
    var request = context.getRequest();

    // item to be created in the current operation
    var itemToCreate = request.getBody();

    // validate properties
    if (!("timestamp" in itemToCreate)) {
        var ts = new Date();
        itemToCreate["timestamp"] = ts.getTime();
    }

    // update the item that will be created
    request.setBody(itemToCreate);
}
```

#### Post-Triggers

Post-triggers are executed after a database item has been modified. They can be used to perform additional actions, such as updating metadata. The following post-trigger updates a metadata document with information about a newly created item:

```javascript
function updateMetadata() {
    var context = getContext();
    var container = context.getCollection();
    var response = context.getResponse();

    // item that was created
    var createdItem = response.getBody();

    // query for metadata document
    var filterQuery = 'SELECT * FROM root r WHERE r.id = "_metadata"';
    var accept = container.queryDocuments(container.getSelfLink(), filterQuery,
        updateMetadataCallback);
    if(!accept) throw "Unable to update metadata, abort";

    function updateMetadataCallback(err, items, responseOptions) {
        if(err) throw new Error("Error" + err.message);
        if(items.length != 1) throw 'Unable to find metadata document';

        var metadataItem = items[0];

        // update metadata
        metadataItem.createdItems += 1;
        metadataItem.createdNames += " " + createdItem.id;
        var accept = container.replaceDocument(metadataItem._self,
            metadataItem, function(err, itemReplaced) {
                if(err) throw "Unable to update metadata, abort";
            });
        if(!accept) throw "Unable to update metadata, abort";
        return;
    }
}
```

It's important to note that triggers are not automatically executed - they must be explicitly specified for each database operation where you want them to run.

### User-Defined Functions (UDFs)

User-defined functions in Cosmos DB are JavaScript functions that can be used to extend the functionality of your queries. They can be registered at the database or container level and then used within queries.

For example, the following UDF calculates the income tax for various income brackets:

```javascript
function tax(income) {
    if(income == undefined)
        throw 'no input';

    if (income < 1000)
        return income * 0.1;
    else if (income < 10000)
        return income * 0.2;
    else
        return income * 0.4;
}
```

This UDF could then be used in a query like this:

```sql
SELECT c.name, udf.tax(c.income) AS tax
FROM c
WHERE c.country = 'USA'
```

For more information on working with triggers and UDFs in Azure Cosmos DB, refer to the [official documentation](https://docs.microsoft.com/en-us/azure/cosmos-db/how-to-write-stored-procedures-triggers-udfs).

## Exploring the Change Feed in Azure Cosmos DB

Azure Cosmos DB's Change Feed provides a persistent, ordered record of changes made to the data in a Cosmos DB container. This change history can power real-time data processing and event-driven architectures.

### Understanding the Change Feed

The Change Feed tracks all insert and update operations. It does not currently log delete operations, so you can use a "soft delete" marker as a workaround.

### Reading the Change Feed

You can access the Change Feed using a "push" or "pull" model:

1. **Push Model**:
   - Azure Functions Trigger: Functions are triggered directly by the Change Feed, scaling and processing changes automatically.
   - Change Feed Processor Library: The Cosmos DB SDKs provide a library that simplifies reading and processing the Change Feed.

2. **Pull Model**:
   - Manual reading: You can query the Change Feed directly, giving more control but requiring you to manage checkpointing and scaling.

The push model, using Azure Functions or the Change Feed Processor, is generally recommended as it handles complexities like scaling and state management.

### Implementing the Change Feed Processor

When using the Change Feed Processor, you'll need:

1. **Monitored Container**: The Cosmos DB container to monitor for changes.
2. **Lease Container**: A separate container to store the processing state.
3. **Compute Instance**: The application hosting the Change Feed Processor.
4. **Delegate**: The code to process each batch of changes.

Here's a basic C# example:

```csharp
var changeFeedProcessor = cosmosClient.GetContainer(databaseName, sourceContainerName)
    .GetChangeFeedProcessorBuilder<ToDoItem>(
        processorName: "changeFeedSample",
        onChangesDelegate: HandleChangesAsync)
    .WithInstanceName("consoleHost")
    .WithLeaseContainer(leaseContainer)
    .Build();

await changeFeedProcessor.StartAsync();
```

The `HandleChangesAsync` delegate defines the logic to process each batch of changes.

For more information, see the [official documentation](https://docs.microsoft.com/en-us/azure/cosmos-db/change-feed).
