# Explore Azure Event Grid

Objectives:

- Describe how Event Grid operates and how it connects to services and event handlers.
- Explain how Event Grid delivers events and how it handles errors.
- Implement authentication and authorization.
- Route custom events to web endpoint by using Azure CLI.

### **Exploring Azure Event Grid**

**Key Concepts:**

1. **Overview of Azure Event Grid:**
   - Azure Event Grid is a serverless event broker that facilitates event-driven architecture by routing events from sources to handlers.
   - Supports events from Azure services, applications, and SaaS products.

2. **Core Concepts in Azure Event Grid:**
   - **Events:** The data indicating that something has happened, containing details like the source, time, and a unique identifier.
   - **Event Sources:** The origin of events, which can be Azure services, third-party services, or your own applications.
   - **Topics:** Endpoints where events are sent. Publishers send events to topics and subscribers listen to these topics.
   - **Event Subscriptions:** Define which events a subscriber wants to receive and where these should be sent. Subscriptions can filter events based on type or subject patterns.
   - **Event Handlers:** Services or applications that act upon the received events. Event handlers can be Azure services, webhooks, or custom applications.

3. **Topics in Azure Event Grid:**
   - **System Topics:** Built-in topics for Azure services that are managed by the publisher.
   - **Custom Topics:** User-defined topics that handle events from your applications or third-party services.

4. **Event Subscriptions and Filtering:**
   - Subscriptions allow for filtering events sent to handlers based on specific criteria, reducing unnecessary traffic and focusing on relevant events.
   - Subscriptions can have expiration times, particularly useful for temporary event handling needs.

5. **Event Delivery and Handling:**
   - Event Grid ensures reliable event delivery with retries until the event is accepted by the handler.
   - Supports multiple handler types, including HTTP webhooks and Azure Storage Queues, adapting delivery methods based on the handler type.

**Using Azure Event Grid:**

- Azure Event Grid integrates smoothly with other Azure services like Blob Storage and Azure Functions, allowing for a wide range of scenarios like image resizing on upload, database updates, and more.
- For custom applications, define your events and handlers to suit your specific needs, ensuring flexible and scalable event-driven architecture.

![how Event Grids connects](https://learn.microsoft.com/en-us/training/wwl-azure/azure-event-grid/media/functional-model.png)

### **Discovering Event Schemas in Azure Event Grid for Azure 204 Certification**

---

**Objective:**
Learn about the event schemas supported by Azure Event Grid to effectively handle and integrate event-driven architectures.

**Event Grid Schemas:**
Azure Event Grid supports two main types of event schemas, which define how events are structured and processed:

1. **Event Grid Event Schema:**
   - Consists of a set of required properties common to all events, detailed below in the table.

2. **Cloud Event Schema:**
   - An implementation of the CloudEvents specification, providing a standardized way of describing event data across services.
   - Simplifies event handling and interoperability across different systems and services.

**Key Concepts in Event Grid Schemas:**

- **Event Sources and Topics:**
  - Events are sent from sources to topics. Sources can be Azure services like Storage or custom applications.
  - Topics collect and forward events to subscriptions based on filters set by subscribers.

- **Event Subscriptions and Handlers:**
  - Subscriptions define which events should be forwarded to which handlers, allowing filtering based on type or subject.
  - Handlers process the events, which can be services like Azure Functions or Logic Apps.

- **Size and Charges:**
  - Each event can be up to 1 MB in size, with the total array of events also capped at 1 MB.
  - Events larger than 64 KB are charged in 64-KB increments.

**Event Properties Table:**
The following table outlines the properties used by all event publishers in the Event Grid schema:

| Property         | Type   | Required | Description                                                                                      |
|------------------|--------|----------|--------------------------------------------------------------------------------------------------|
| `topic`          | string | No       | Full resource path to the event source. This field isn't writeable. Event Grid provides this value. |
| `subject`        | string | Yes      | Publisher-defined path to the event subject.                                                     |
| `eventType`      | string | Yes      | One of the registered event types for this event source.                                         |
| `eventTime`      | string | Yes      | The time the event is generated based on the provider's UTC time.                                |
| `id`             | string | Yes      | Unique identifier for the event.                                                                 |
| `data`           | object | No       | Event data specific to the resource provider.                                                   |
| `dataVersion`    | string | No       | The schema version of the data object. If not included, it's stamped with an empty value.        |
| `metadataVersion`| string | No       | The schema version of the event metadata. If not included, Event Grid stamps onto the event.     |

**Example of an Event in Event Grid Schema:**

```json
[
  {
    "topic": "string",
    "subject": "string",
    "id": "string",
    "eventType": "string",
    "eventTime": "string",
    "data": {
      "object-unique-to-each-publisher"
    },
    "dataVersion": "string",
    "metadataVersion": "string"
  }
]
```

Example of an Azure Blob Storage Event in CloudEvents Format:

```json
{
    "specversion": "1.0",
    "type": "Microsoft.Storage.BlobCreated",
    "source": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-account}",
    "id": "9aeb0fdf-c01e-0131-0922-9eb54906e209",
    "time": "2019-11-18T15:13:39.4589254Z",
    "subject": "blobServices/default/containers/{storage-container}/blobs/{new-file}",
    "dataschema": "#",
    "data": {
        "api": "PutBlockList",
        "clientRequestId": "4c5dd7fb-2c48-4a27-bb30-5361b5de920a",
        "requestId": "9aeb0fdf-c01e-0131-0922-9eb549000000",
        "eTag": "0x8D76C39E4407333",
        "contentType": "image/png",
        "contentLength": 30699,
        "blobType": "BlockBlob",
        "url": "<https://gridtesting.blob.core.windows.net/testcontainer/{new-file}>",
        "sequencer": "000000000000000000000000000099240000000000c41c18",
        "storageDiagnostics": {
            "batchId": "681fe319-3006-00a8-0022-9e7cde000000"
        }
    }
}
```