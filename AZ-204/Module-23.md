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

Sure, here's the content edited to adhere to study guide best practices:

### **Azure Event Grid Event Delivery Durability**

#### Event Delivery Basics

- Azure Event Grid ensures durable delivery by attempting to deliver each event at least once for each subscription
- Immediate delivery is tried first
- If the subscriber's endpoint does not acknowledge receipt of an event or there's a delivery failure, Event Grid implements a retry mechanism according to a fixed schedule and policy

#### Retry Schedule and Policy

- Event Grid analyzes the type of error received during event delivery attempts to decide on subsequent actions
- Errors like `404 Not Found` or `400 Bad Request` may lead to the event being dead-lettered or dropped if no dead-letter configuration exists
- For recoverable errors, Event Grid uses an exponential backoff retry policy, waiting for 30 seconds before the first retry

#### Configuration Examples

- Set the maximum number of retry attempts using the Azure CLI:

  ```bash
  az eventgrid event-subscription create \
    -g gridResourceGroup \
    --topic-name <topic_name> \
    --name <event_subscription_name> \
    --endpoint <endpoint_URL> \
    --max-delivery-attempts 18
  ```

#### Output Batching

- Event Grid supports event batching to enhance HTTP performance, especially in high-throughput scenarios
- Options include:
  - Max events per batch: Limits the number of events per batch
  - Preferred batch size in kilobytes: Sets the maximum batch size in kilobytes

#### Delayed Delivery

- As an endpoint experiences delivery failures, Event Grid begins to delay the delivery and retry of events to that endpoint
- This delay is to prevent overwhelming an already struggling endpoint and to protect the overall system stability

#### Dead-lettering

- If Event Grid cannot deliver an event within the specified time-to-live (TTL) or after a set number of retries, it can send the undelivered event to a dead-letter storage account
- Dead-lettering ensures that events that cannot be delivered are not lost

#### Non-retriable Error Codes

| Endpoint Type | Error Codes |
| --- | --- |
| Azure Resources | `400 Bad Request`, `413 Request Entity Too Large`, `403 Forbidden` |
| Webhook | `400 Bad Request`, `413 Request Entity Too Large`, `403 Forbidden`, `404 Not Found`, `401 Unauthorized` |

#### Custom Delivery Properties

- Custom HTTP headers can be added to events delivered by Event Grid
- This allows for the specification of headers required by the destination endpoint, facilitating smoother integration and handling of incoming events

#### Setting Dead-letter Location

- To enable dead-lettering, specify a storage account during the event subscription setup
- This account will store events that fail to deliver, allowing for later analysis or reprocessing

#### Event Grid's Approach to Managing Delivery Failures

- Event Grid's mechanisms, such as delayed delivery, output batching, and dead-lettering, are designed to handle delivery interruptions and failures efficiently
- This ensures data is not lost and systems are not overwhelmed

# Controlling Access to Azure Event Grid

## Overview

- Azure Event Grid allows you to control the level of access given to different users for various management operations
- Such as:
  - Listing event subscriptions
  - Creating new event subscriptions
  - Generating keys
- Event Grid uses Azure role-based access control (Azure RBAC) to manage permissions

## Built-in Roles

Event Grid provides the following built-in roles:

| Role | Description |
| --- | --- |
| Event Grid Subscription Reader | Lets you read Event Grid event subscriptions. |
| Event Grid Subscription Contributor | Lets you manage Event Grid event subscription operations. |
| Event Grid Contributor | Lets you create and manage Event Grid resources. |
| Event Grid Data Sender | Lets you send events to Event Grid topics. |

The `Event Grid Subscription Reader` and `Event Grid Subscription Contributor` roles are important for managing event subscriptions, especially when implementing event domains, as they grant users the permissions to subscribe to topics.

The `Event Grid Contributor` role allows you to create and manage Event Grid resources.

## Permissions for Event Subscriptions

When creating an event subscription, you need the appropriate permissions based on the type of event source:

### System Topics

For system topics, you need the `Microsoft.EventGrid/EventSubscriptions/Write` permission on the resource publishing the event. The resource format is:

```
/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{resource-provider}/{resource-type}/{resource-name}
```

### Custom Topics

For custom topics, you need the `Microsoft.EventGrid/EventSubscriptions/Write` permission on the scope of the Event Grid topic. The resource format is:

```
/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.EventGrid/topics/{topic-name}
```

This permissions check ensures that only authorized users can send events to your event handler resources.

### **Controlling Access to Events in Azure Event Grid**

**Azure Role-Based Access Control (Azure RBAC):**
Azure Event Grid uses Azure RBAC to control the level of access various users have to perform management operations such as listing event subscriptions, creating new ones, and generating keys.

**Built-in Roles:**
Azure Event Grid provides several built-in roles that specify the level of access users have:

- **Event Grid Subscription Reader:** Allows reading of Event Grid event subscriptions.
- **Event Grid Subscription Contributor:** Enables management of Event Grid event subscription operations.
- **Event Grid Contributor:** Permits creation and management of Event Grid resources.
- **Event Grid Data Sender:** Allows sending events to Event Grid topics.

The roles of Event Grid Subscription Reader and Event Grid Subscription Contributor are crucial for managing event subscriptions, especially when implementing event domains. They provide the necessary permissions to subscribe to topics within your event domain, focusing specifically on subscription management without granting permissions for actions like creating topics.

The Event Grid Contributor role facilitates broader management capabilities, allowing the creation and oversight of Event Grid resources.

**Permissions for Event Subscriptions:**
If you are using an event handler that is not a WebHook, such as an event hub or queue storage, you need write access to that resource. This is crucial to ensure that unauthorized users cannot send events to your resource.

To subscribe to different types of topics, specific permissions are required:

- **System Topics:** You must have the `Microsoft.EventGrid/EventSubscriptions/Write` permission on the resource that is the event source. This allows you to write a new subscription at the scope of the resource. The format for this is:
  `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{resource-provider}/{resource-type}/{resource-name}`

- **Custom Topics:** You need the same `Microsoft.EventGrid/EventSubscriptions/Write` permission, but at the scope of the Event Grid topic itself. The format for this is:
  `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.EventGrid/topics/{topic-name}`

### **Receiving Events by Using Webhooks in Azure Event Grid**

**Introduction to Webhooks in Event Grid:**
Webhooks are a common method for receiving events from Azure Event Grid. When a new event is available, Event Grid sends an HTTP POST request to the configured webhook endpoint, delivering the event within the request body.

**Endpoint Ownership Verification:**
To ensure security, Event Grid requires verification of ownership for the webhook endpoint before it begins event delivery. This step is crucial to prevent unauthorized users from targeting your endpoint with unwanted events.

**Automatic Validation in Azure Services:**
For certain Azure services, endpoint validation is managed automatically:

- **Azure Logic Apps with Event Grid Connector**
- **Azure Automation via webhook**
- **Azure Functions with Event Grid Trigger**

These services handle the validation process internally, simplifying the setup for event reception.

**Endpoint Validation for Custom Endpoints:**
If you are using custom endpoints, such as an HTTP trigger-based Azure Function that does not automatically handle validation, you need to implement a validation handshake with Event Grid:

- **Synchronous Handshake:** During subscription creation, Event Grid sends a validation event. Your endpoint must validate this event and return the `validationCode` from the event data synchronously.
- **Asynchronous Handshake (Manual Validation):** Available from version 2018-05-01-preview of Event Grid, this method is used when immediate synchronous response with the validation code is not feasible. Event Grid sends a `validationUrl` in the validation event data. You complete the validation by making a GET request to this URL within 5 minutes to confirm subscription setup.

**Procedure for Manual Validation:**

1. Receive the subscription validation event containing the `validationUrl`.
2. Perform a GET request to the `validationUrl` using a REST client or web browser within 5 minutes.
3. Ensure the endpoint initially returns an HTTP status code of 200 to indicate receipt of the validation event.

**Certificate Requirements:**

- Using self-signed certificates for validation of webhook endpoints is not supported.
- Use a certificate signed by a commercial Certificate Authority (CA) to ensure security and compatibility.

**Note:**

- The validation URL provided by Event Grid is valid for only 5 minutes
  - after which manual validation must be initiated again if not completed.
- This ensures that the endpoint is ready and able to receive events securely and effectively.

### **Filtering Events in Azure Event Grid**

**Introduction to Event Filtering:**
Azure Event Grid allows precise control over the events sent to endpoints through three primary filtering mechanisms: event types, subject patterns, and advanced fields with operators.

**Event Type Filtering:**

- **Default Behavior:** All event types from the source are sent to the endpoint.
- **Selective Filtering:** You can choose to receive only certain event types, such as notifications of updates but not deletions. For example, to receive only notifications for resource updates, you can filter for the `Microsoft.Resources.ResourceWriteSuccess` event type.
- **Configuration Example:**

  ```json
  "filter": {
    "includedEventTypes": [
      "Microsoft.Resources.ResourceWriteSuccess"
    ]
  }
  ```

**Subject Filtering:**

- Usage: Filter events based on the start or end of the subject string. This is useful for narrowing down events to specific subjects or categories.
- Examples:
  - Filter events that end with `.txt` to receive notifications related to text file uploads.
  - Filter events that start with `/blobServices/default/containers/testcontainer` to receive all events for that specific container.
- **Configuration Example:**

  ```json
  "filter": {
    "subjectBeginsWith": "/blobServices/default/containers/mycontainer/log",
    "subjectEndsWith": ".jpg"
  }
  ```

**Advanced Filtering:**

- Purpose: Use advanced filtering to apply more granular control by targeting specific values in the event data using operators.
- Details: You can specify the operator type, the key to filter on, and the value or values to match against. This allows for complex filtering scenarios, such as filtering based on numeric values, booleans, or specific strings within the event data.
- **Configuration Example:**

  ```json
  "filter": {
    "advancedFilters": [
      {
        "operatorType": "NumberGreaterThanOrEquals",
        "key": "Data.Key1",
        "value": 5
      },
      {
        "operatorType": "StringContains",
        "key": "Subject",
        "values": ["container1", "container2"]
      }
    ]
  }
  ```
