# Explore Microsoft Graph

Objectives:

- Explain the benefits of using Microsoft Graph
- Perform operations on Microsoft Graph by using REST and SDKs
- Apply best practices to help your applications get the most out of Microsoft Graph

## Introduction to Microsoft Graph

- Definition of Microsoft Graph
- Key components:
  - Microsoft Graph API
  - Microsoft Graph Connectors

## Microsoft Graph API

- Accessing data and intelligence from Microsoft 365 services
- Supported operations (read, write, update, delete)
- Authentication and authorization
- SDKs and client libraries
- Common use cases and scenarios

## Microsoft Graph Connectors

- Purpose and benefits of Microsoft Graph Connectors
- Connecting external data sources to Microsoft 365
- List of available connectors (Box, Google Drive, Jira, Salesforce, etc.)
- Configuration and management of connectors
- Use cases for enhancing Microsoft 365 experiences

## Best Practices and Considerations

- Authentication and authorization best practices
- Handling data privacy and compliance requirements
- Performance and scalability considerations
- Error handling and troubleshooting techniques

# Querying Microsoft Graph using REST

## Introduction

Microsoft Graph is a RESTful web API that allows you to access and interact with Microsoft Cloud service resources.

- To make requests to the Microsoft Graph API, register your app and obtain authentication tokens for a user or service.

## Constructing a REST API Request

A typical Microsoft Graph REST API request consists of the following components:

1. **HTTP Method**: The HTTP method used to perform the operation (GET, POST, PATCH, PUT, DELETE).
2. **Endpoint**: The base URL for the Microsoft Graph API, `https://graph.microsoft.com/{version}/{resource}`.
   - `{version}`: The version of the Microsoft Graph API you're using (v1.0 or beta).
   - `{resource}`: The resource in Microsoft Graph that you're interacting with (e.g., users, messages, files).
3. **Query Parameters**: Optional OData query options or REST method parameters to customize the response.

Here's an example of a GET request to retrieve a user's messages:

```
GET https://graph.microsoft.com/v1.0/me/messages?$filter=emailAddress eq 'jon@contoso.com'
```

## HTTP Methods

Microsoft Graph supports the following HTTP methods:

- **GET**: Read data from a resource.
- **POST**: Create a new resource or perform an action.
- **PATCH**: Update a resource with new values.
- **PUT**: Replace a resource with a new one.
- **DELETE**: Remove a resource.

For GET and DELETE methods, no request body is required. For POST, PATCH, and PUT methods, you'll need to include a request body in JSON format with the necessary data.

## Versioning

Microsoft Graph currently supports two versions:

- **v1.0**: Includes generally available APIs, recommended for production apps.
- **beta**: Includes preview APIs, should only be used for testing apps in development.

## Resources and Relationships

Microsoft Graph defines various resources, such as users, groups, files, and emails. These resources often have relationships that you can access, like `me/messages` or `me/drive`.

Each resource might require different permissions to access or perform CRUD operations on it.

## Query Parameters

You can use OData system query options to customize the response, such as:

- `$filter`: Filter the response to only include items matching a custom query.
- `$select`: Include only the specified properties in the response.
- `$orderby`: Sort the response based on a property.

For example, to filter messages by email address:

```
GET https://graph.microsoft.com/v1.0/me/messages?$filter=emailAddress eq 'jon@contoso.com'
```

## Tools for Building and Testing Requests

- **Graph Explorer**: A web-based tool for interacting with the Microsoft Graph API.
- **Postman**: A popular API client that can be used to build and test Microsoft Graph requests.

# Querying Microsoft Graph Using SDKs

## Introduction

The Microsoft Graph SDKs are designed to simplify the process of building high-quality, efficient, and resilient applications that access Microsoft Graph. The SDKs include two main components:

1. **Service Library**: Provides models and request builders generated from the Microsoft Graph metadata, offering a rich and discoverable experience.
2. **Core Library**: Includes features that enhance working with Microsoft Graph services, such as retry handling, secure redirects, transparent authentication, and payload compression.

## Installing the Microsoft Graph .NET SDK

The Microsoft Graph .NET SDK is available through the following NuGet packages:

- `Microsoft.Graph`: Contains models and request builders for accessing the v1.0 endpoint.
- `Microsoft.Graph.Beta`: Contains models and request builders for accessing the beta endpoint.
- `Microsoft.Graph.Core`: The core library for making calls to Microsoft Graph.

## Creating a Microsoft Graph Client

You can use a single client instance for the lifetime of the application.

The authentication provider handles acquiring access tokens for the application.

- The different application providers support different client scenarios.

[Choose an Authentication Provider](https://learn.microsoft.com/en-us/graph/sdks/choose-authentication-providers)

```csharp
var scopes = new[] { "User.Read" };
var tenantId = "common";
var clientId = "YOUR_CLIENT_ID";

var deviceCodeCredential = new DeviceCodeCredential(
    tenantId: tenantId,
    clientId: clientId);

var graphClient = new GraphServiceClient(deviceCodeCredential, scopes);
```

## Reading Information from Microsoft Graph

To read information from Microsoft Graph, you'll create a request object and use the `GetAsync()` method to execute the request.

```csharp
var user = await graphClient.Me
    .GetAsync();
```

## Retrieving a List of Entities

To retrieve a list of entities, you can use additional query parameters like `$filter` and `$orderBy` to customize the response.

```csharp
var messages = await graphClient.Me.Messages
    .Request()
    .Select(m => new { m.Subject, m.Sender })
    .Filter("<filter condition>")
    .OrderBy("receivedDateTime")
    .GetAsync();
```

## Deleting an Entity

To delete an entity, you'll construct the request similar to how you would retrieve an entity, but use the `DeleteAsync()` method.

```csharp
string messageId = "AQMkAGUy...";
await graphClient.Me.Messages[messageId]
    .Request()
    .DeleteAsync();
```

## Creating a New Entity

To create a new entity, you can use the `AddAsync()` method on the collection you want to add the new item to.

```csharp
var calendar = new Calendar { Name = "Volunteer" };
var newCalendar = await graphClient.Me.Calendars
    .Request()
    .AddAsync(calendar);
```

## Additional Resources

- [Microsoft Graph REST API v1.0 reference](https://docs.microsoft.com/en-us/graph/api/overview?view=graph-rest-1.0)

# Applying Best Practices with Microsoft Graph

## Authentication

- Use the Microsoft Authentication Library (MSAL) to acquire OAuth 2.0 access tokens for Microsoft Graph.
- Present the access token to Microsoft Graph either in the HTTP `Authorization` header or the `GraphServiceClient` constructor.

## Consent and Authorization

- Request only the permissions your application needs, using the least privileged permissions.
- Use delegated permissions for interactive applications with signed-in users, and application permissions for non-interactive applications like background services.
- Consider the end-user and admin experience when designing your consent flow.
- Expect various application and consent control states in a multi-tenant environment.

## Handling Responses

- Implement pagination handling when querying resource collections, using the `@odata.nextLink` property.
- Handle "evolvable enumerations" by default, and opt-in to receive unknown members if needed.

## Storing Data Locally

- Retrieve data from Microsoft Graph in real-time as necessary.
- Only cache or store data locally:
  - If it's covered by your terms of use and privacy policy 
  - And doesn't violate the Microsoft APIs Terms of Use.
- Implement proper data retention and deletion policies.

## Additional Best Practices

- Follow Microsoft Graph versioning guidance, using the stable `v1.0` version for production applications.
- Handle errors and retries gracefully, using the features provided by the Microsoft Graph SDKs.
- Monitor and log your application's usage of Microsoft Graph for better insights and troubleshooting.
- Stay up-to-date with the latest Microsoft Graph news, updates, and guidance from Microsoft.
