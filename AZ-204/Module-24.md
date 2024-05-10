# Explore Azure Event Hubs

Objectives:

- Describe the benefits of using Event Hubs and how it captures streaming data.
- Explain how to process events.
- Perform common operations with the Event Hubs client library.

### **Discovering Azure Event Hubs**

**Overview of Azure Event Hubs:**
Azure Event Hubs serves as the "front door" for an event pipeline, often referred to as an event ingestor in solution architectures. It acts as a component or service that sits between event publishers and consumers, decoupling the production of an event stream from the consumption of those events. Event Hubs provides a unified streaming platform with a time retention buffer, effectively decoupling event producers from consumers.

**Key Features of Azure Event Hubs:**

| Feature                         | Description |
|---------------------------------|-------------|
| Fully managed PaaS              | Event Hubs is a fully managed Platform-as-a-Service (PaaS), minimizing configuration or management overhead, allowing you to focus on business solutions. It supports Apache Kafka ecosystems, offering a PaaS Kafka experience without the need to manage Kafka clusters. |
| Real-time and batch processing  | Utilizes a partitioned consumer model, enabling concurrent processing by multiple applications and allowing control over the processing speed. |
| Capture event data              | Captures data in near-real-time in Azure Blob storage or Azure Data Lake Storage for long-term retention or micro-batch processing. |
| Scalable                        | Provides scaling options like Auto-inflate, which automatically scales throughput units based on usage needs. |
| Rich ecosystem                  | Supports Apache Kafka (1.0 and later) clients and applications, allowing them to communicate with Event Hubs without the need for Kafka cluster management. |

**Key Components of Azure Event Hubs:**

- **Event Hubs Client:** The primary interface for developers interacting with the Event Hubs client library, tailored for specific uses like publishing or consuming events.
- **Event Hubs Producer:** Acts as a source of telemetry data, diagnostics, usage logs, or other data, coming from devices, applications, games, business solutions, or websites.
- **Event Hubs Consumer:** Reads data from Event Hubs for processing, which may include aggregation, computation, filtering, distribution, or storage.
- **Partition:** An ordered sequence of events held in an Event Hub, crucial for data organization and parallel processing by consumers.
- **Consumer Group:** Represents a view of the entire Event Hub, enabling multiple consumers to have a separate view of the event stream and read it independently at their own pace.
- **Event Receivers:** Any entity that reads event data from an Event Hub
  - connecting via the AMQP 1.0 session or Kafka protocol for Kafka consumers.
- **Throughput Units:** Pre-purchased units of capacity that control the throughput capacity of Event Hubs
  - essential for managing data flow and processing speed.

**Usage Recommendations:**

- It's advisable to have one active consumer per partition and consumer group pairing to avoid duplicate events.
- Consider throughput units carefully to match the scale of data flow and processing requirements of your applications.

The following figure shows the Event Hubs stream processing architecture:

![event hubs stream process](https://learn.microsoft.com/en-us/training/wwl-azure/azure-event-hubs/media/event-hubs-stream-processing.png)

### **Exploring Azure Event Hubs Capture**

**Key Concepts:**

1. **Overview of Event Hubs Capture:**
   - Azure Event Hubs Capture is designed as a seamless integration feature that automatically captures streaming data into Azure Blob Storage or Azure Data Lake Storage, based on predefined size or time intervals.
   - This feature is highly scalable, aligning with Event Hubs' throughput units in the standard tier or processing units in the premium tier.

2. **Configuration and Flexibility:**
   - Event Hubs Capture allows you to specify the exact storage account and container or Azure Data Lake Store account where data should be captured. This flexibility supports various architectural needs and scenarios.
   - You can set up the capture feature quickly with minimal configuration, and there are no administrative costs associated with running it.

3. **Functionality and Use Cases:**
   - The captured data is written in Apache Avro format, which is compact, fast, and widely used in analytics ecosystems, making it ideal for real-time and batch processing.
   - Event Hubs Capture supports real-time analytics and batch processing on the same stream of data, facilitating complex analytics and big data scenarios.

4. **Scaling and Throughput:**
   - Event Hubs Capture scales automatically with your Event Hubs throughput units. It's capable of handling massive streams of data by scaling up as necessary based on your configuration and usage patterns.
   - Throughput units define the rate at which data can be ingested, and you can adjust them to meet your specific needs.

**Using Azure Event Hubs Capture:**

- Setting up Event Hubs Capture involves specifying the target Azure Blob Storage or Azure Data Lake Storage account, and defining the size or time interval for capturing data.
- The system automatically manages the capture process, writing data to storage without manual intervention, and scales with the configured Event Hubs throughput.
- The integration of Event Hubs Capture with Azure Blob Storage and Azure Data Lake enhances Azure's big data and real-time analytics capabilities, making it a powerful tool for developers and businesses focusing on data-driven decision-making.

![Event Hubs Data Capture Process](https://path_to_image_in_docs_or_online)

### **Details on Event Hubs Capture Configuration**

**Capture Configuration Steps:**

1. **Choose Storage:** Select either Azure Blob Storage or Azure Data Lake Storage for data capture.
2. **Set Intervals:** Define the time or size intervals at which data should be automatically captured and stored.
3. **Specify Format:** Data is captured in Apache Avro format, which integrates seamlessly with various analytics tools.

**Capture Windowing:**

- Event Hubs Capture uses a "first wins policy" for its capture window, where the first threshold (time or size) reached triggers data capture.
- Each partition captures data independently, ensuring efficient data flow and storage.

**Scaling Considerations:**

- Event Hubs automatically adjusts the number of throughput units based on data flow, ensuring optimal performance and cost-efficiency.
- Monitoring tools are available to track throughput, data ingress, and egress, helping fine-tune the scaling settings.

# Scaling Your Processing Application with Azure Event Hubs

## Key Concepts

- **Event Processor Versions**:
  - Initially, `EventProcessorHost` facilitated load balancing and event checkpointing in older versions.
  - From version 5.0 onwards, `EventProcessorClient` for .NET and Java, and `EventHubConsumerClient` for Python and JavaScript, have taken over these responsibilities.
- **Partitioned Consumers**:
  - Unlike the competing consumers pattern, the partitioned consumer pattern excels by eliminating bottlenecks and enhancing parallel processing.
  - This setup significantly increases scalability by reducing contention.

## Practical Example

Consider a home security company monitoring 100,000 homes, receiving data every minute from multiple sensors per home:

1. Sensors send data to an event hub partitioned into 16 segments.
2. Consumers read these events, aggregate them, and store the results in blob storage, updating a user-friendly webpage in near real-time.

## Design Considerations

- **Scalability**: Implement multiple consumer instances, with each managing a subset of the Event Hubs partitions.
- **Load Balancing**: Dynamically adjust the number of consumers to manage workload efficiently, especially when new sensor types are introduced.
- **Fault Tolerance**: Design consumers to seamlessly take over the partitions of a failed instance and resume from the last known good state.

## Environment Requirements

- **Infrastructure**: Robust cloud infrastructure with auto-scaling capabilities to manage varying loads and ensure availability.
- **Networking**: High bandwidth and low latency network connections to handle high volumes of incoming data efficiently.
- **Computing**: Adequate computing resources to process large volumes of data swiftly, including multi-core processors and high RAM configurations.
- **Storage**: Efficient and scalable storage solutions to handle large data accumulations and rapid access needs.

## Using Event Hubs SDKs

- **Functionality**: The SDKs provide built-in support for event processing, including load balancing and state management among consumer instances.
- **Implementation**: In production environments, it's recommended to use the event processor client, which supports cooperative processing within a consumer group.

## Advanced Features

- **Partition Ownership**: Each event processor instance manages one or more partitions, with ownership distributed among active instances.
- **Message Processing**: Define functions for processing events and handling errors. Implement retry mechanisms while being cautious of poisoned messages.
- **Checkpointing**: Mark the position of the last successfully processed event to ensure resiliency and continuity in event processing.

## Best Practices

- Minimize processing time within consumer functions to maintain efficiency.
- Consider using separate consumer groups for different processing tasks to optimize performance and manageability.

## AZ-204: Develop Event-Based Solutions

### Controlling Access to Events in Azure Event Hubs

Ensuring secure and controlled access to event data is critical in Azure Event Hubs. Azure offers robust mechanisms for authentication and authorization, primarily using Microsoft Entra ID, shared access signatures (SAS), and role-based access control (RBAC).

#### Authentication and Authorization Methods

1. **Microsoft Entra ID and OAuth:**
   - **Azure Built-in Roles:**
     - **Azure Event Hubs Data Owner:** Grants complete access to Event Hubs resources.
     - **Azure Event Hubs Data Sender:** Provides send-only access.
     - **Azure Event Hubs Data Receiver:** Allows receive-only access.

   These roles enable fine-grained control over who can send and receive data, enhancing security and operational efficiency.

2. **Managed Identities:**
   - Assigning specific Azure roles to a managed identity allows that identity to send or receive data from Event Hubs without managing credentials in code, thus maintaining a secure environment.

3. **Microsoft Identity Platform:**
   - Utilizing Microsoft Entra ID, applications can request OAuth 2.0 access tokens from the Microsoft identity platform. This process removes the need to store sensitive credentials in the application code and uses tokens to authorize requests to Event Hubs.

#### Shared Access Signatures (SAS)

- **Event Hubs Publishers:**
  - Each publisher in an Event Hub acts as a virtual endpoint that can only send messages. Fine-grained access control is achieved by assigning each client a unique token, allowing it to send messages to its designated publisher.

- **Event Hubs Consumers:**
  - For backend applications consuming data, SAS tokens ensure that only clients with appropriate rights (manage or listen) can access Event Hubs data. This is controlled at the namespace or event hub instance/topic level, affecting all consumer groups within the entity.

#### Practical Application and Best Practices

- **Secure Token Handling:** Ensure that tokens are securely managed and not exposed to clients, to prevent unauthorized access.
- **Role Assignment:** Carefully assign roles to ensure that entities have only the necessary permissions, minimizing potential security risks.
- **Use of Managed Identities:** Leverage managed identities for automatic handling of credentials and seamless integration with Azure services.

By implementing these access control mechanisms, developers can ensure that their Event Hubs-based applications are both secure and compliant with organizational policies. 

This setup not only protects sensitive data but also optimizes the management of access permissions, crucial for maintaining the integrity and security of the data flow.

## AZ-204: Develop Event-Based Solutions

### Performing Common Operations with the Event Hubs Client Library

Azure Event Hubs is a highly scalable data streaming platform and event ingestion service, capable of receiving and processing millions of events per second. Below are detailed examples and explanations of common operations you can perform using the `Azure.Messaging.EventHubs` client library to interact efficiently with Event Hubs.

#### Inspecting Event Hubs

- To manage and troubleshoot Event Hubs effectively, you need to understand its structure, especially the partitions which are fundamental for data distribution and parallel processing.
- Here's how you can inspect the available partitions within an Event Hub:

  ```csharp
  var connectionString = "<< CONNECTION STRING FOR THE EVENT HUBS NAMESPACE >>";
  var eventHubName = "<< NAME OF THE EVENT HUB >>";

  await using (var producer = new EventHubProducerClient(connectionString, eventHubName))
  {
      string[] partitionIds = await producer.GetPartitionIdsAsync();
      // This line fetches an array of partition IDs available within the Event Hub.
  }
  ```

  This operation is critical for applications that may need to interact with specific partitions directly or monitor the health and usage of each partition.

#### Publishing Events to Event Hubs

- Publishing events efficiently is key to leveraging the real-time processing capabilities of Event Hubs.
- Here's how to publish events using the `EventHubProducerClient`, which can automatically route events to maintain load balance across partitions:

  ```csharp
  var connectionString = "<< CONNECTION STRING FOR THE EVENT HUBS NAMESPACE >>";
  var eventHubName = "<< NAME OF THE EVENT HUB >>";

  await using (var producer = new EventHubProducerClient(connectionString, eventHubName))
  {
      using EventDataBatch eventBatch = await producer.CreateBatchAsync();
      eventBatch.TryAdd(new EventData(new BinaryData("First")));
      eventBatch.TryAdd(new EventData(new BinaryData("Second")));

      await producer.SendAsync(eventBatch);
      // This batch of events is sent to the Event Hub, utilizing automatic partition routing.
  }
  ```

  Batching is recommended to ensure efficient network utilization and throughput.
  - The client library handles the complexity of routing and partitioning behind the scenes.

#### Reading Events from an Event Hub

- For applications that process or analyze streaming data, reading events is a fundamental requirement.
- Here's an example of how to read events using the default consumer group:

  ```csharp
  var connectionString = "<< CONNECTION STRING FOR THE EVENT HUBS NAMESPACE >>";
  var eventHubName = "<< NAME OF THE EVENT HUB >>";
  string consumerGroup = EventHubConsumerClient.DefaultConsumerGroupName;

  await using (var consumer = new EventHubConsumerClient(consumerGroup, connectionString, eventHubName))
  {
      using var cancellationSource = new CancellationTokenSource();
      cancellationSource.CancelAfter(TimeSpan.FromSeconds(45));

      await foreach (PartitionEvent receivedEvent in consumer.ReadEventsAsync(cancellationSource.Token))
      {
          // Process each event as it is received.
          // This loop continues indefinitely, processing incoming events in real-time.
      }
  }
  ```

  This approach is suitable for applications that need to process live data as it arrives, such as analytics dashboards or real-time monitoring systems.

#### Processing Events with the Event Processor Client

- For robust, production-ready processing, the `EventProcessorClient` is recommended.
  - It manages state, handles partition distribution, and ensures reliable processing even in complex scenarios:

  ```csharp
  var cancellationSource = new CancellationTokenSource();
  cancellationSource.CancelAfter(TimeSpan.FromSeconds(45));

  var storageConnectionString = "<< CONNECTION STRING FOR THE STORAGE ACCOUNT >>";
  var blobContainerName = "<< NAME OF THE BLOB CONTAINER >>";
  var eventHubsConnectionString = "<< CONNECTION STRING FOR THE EVENT HUBS NAMESPACE >>";
  var eventHubName = "<< NAME OF THE EVENT HUB >>";
  var consumerGroup = "<< NAME OF THE EVENT HUB CONSUMER GROUP >>";

  Task processEventHandler(ProcessEventArgs eventArgs) => Task.CompletedTask;
  Task processErrorHandler(ProcessErrorEventArgs eventArgs) => Task.CompletedTask;

  var storageClient = new BlobContainerClient(storageConnectionString, blobContainerName);
  var processor = new EventProcessorClient(storageClient, consumerGroup, eventHubsConnectionString, eventHubName);

  processor.ProcessEventAsync += processEventHandler;
  processor.ProcessErrorAsync += processErrorHandler;

  await processor.StartProcessingAsync();

  try
  {
      await Task.Delay(Timeout.Infinite, cancellationSource.Token);
  }
  catch (TaskCanceledException)
  {
      // Handle cancellation here
  }

  try
  {
      await processor.StopProcessingAsync();
  }
  finally
  {
      // Clean up handlers here
      processor.ProcessEventAsync -= processEventHandler;
      processor.ProcessErrorAsync -= processErrorHandler;
  }
  ```

  This setup is essential for scenarios requiring high reliability and scale:
  - Processing logs
  - Integrating large scale IoT streams
  - Real-time analytics.

By mastering these operations, developers can build and maintain scalable, efficient event-based solutions using Azure Event Hubs.
