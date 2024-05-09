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