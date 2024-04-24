# Working with Azure Blob Storage

Objectives:

- Create an application to create and manipulate data by using the Azure Storage client library for Blob storage.
- Manage container properties and metadata by using .NET and REST.

## Azure Blob Storage Client Library

The Azure Storage client libraries for .NET provide a convenient interface for interacting with Azure Blob Storage. The latest version of the client library is 12.x, which is recommended for new applications.

## Important Classes

- **BlobServiceClient**: Represents the storage account and allows you to retrieve and configure account properties, as well as work with blob containers.
- **BlobContainerClient**: Represents a specific blob container and provides operations for working with the container and its blobs.
- **BlobClient**: Represents a specific blob and offers general operations for uploading, downloading, deleting, and creating snapshots of the blob.
- **AppendBlobClient**: Represents an append blob and provides operations specifically for working with append blobs, such as appending log data.
- **BlockBlobClient**: Represents a block blob and offers operations specifically for working with block blobs, such as staging and committing blocks of data.

## Relevant Packages

The following packages contain the classes used to work with Blob Storage data resources:

- **Azure.Storage.Blobs**: Contains the primary classes (client objects) that allow you to operate on the service, containers, and blobs.
- **Azure.Storage.Blobs.Specialized**: Contains classes for performing operations specific to a blob type, such as block blobs.
- **Azure.Storage.Blobs.Models**: Contains utility classes, structures, and enumeration types.

By using the Azure Blob Storage client library, you can effectively interact with Azure Blob Storage and perform various operations on blobs and containers within your storage account.

## Create a Client Object

Working with Azure resources using the Azure SDK begins with creating a client object. This section focuses on creating client objects to interact with storage accounts, containers, and blobs in the storage service.

## Creating a BlobServiceClient Object

To interact with resources at the storage account level, you need to create a BlobServiceClient object. The BlobServiceClient provides methods to retrieve and configure account properties, as well as list, create, and delete containers within the storage account.

Here's an example of how to create a BlobServiceClient object:

```csharp
using Azure.Identity;
using Azure.Storage.Blobs;

public BlobServiceClient GetBlobServiceClient(string accountName)
{
    BlobServiceClient client = new BlobServiceClient(
        new Uri($"https://{accountName}.blob.core.windows.net"),
        new DefaultAzureCredential());

    return client;
}
```

## Creating a BlobContainerClient Object

You can use a BlobServiceClient object to create a BlobContainerClient object, which allows you to interact with a specific container resource. The BlobContainerClient provides methods to create, delete, or configure a container, as well as list, upload, and delete the blobs within it.

Here's an example of how to create a BlobContainerClient object:

```csharp
public BlobContainerClient GetBlobContainerClient(
    BlobServiceClient blobServiceClient,
    string containerName)
{
    BlobContainerClient client = blobServiceClient.GetBlobContainerClient(containerName);
    return client;
}
```

## Creating a BlobClient Object

To interact with a specific blob resource, you can create a BlobClient object from a service client or container client. The BlobClient allows you to perform operations on a specific blob, such as uploading, downloading, deleting, and creating snapshots.

Here's an example of how to create a BlobClient object:

```csharp
public BlobClient GetBlobClient(
    BlobServiceClient blobServiceClient,
    string containerName,
    string blobName)
{
    BlobClient client = blobServiceClient.GetBlobContainerClient(containerName).GetBlobClient(blobName);
    return client;
}
```

If your work is narrowly scoped to a single container, you might choose to create a BlobContainerClient object directly without using BlobServiceClient.

```C#
public BlobContainerClient GetBlobContainerClient(
    string accountName,
    string containerName,
    BlobClientOptions clientOptions)
{
    // Append the container name to the end of the URI
    BlobContainerClient client = new(
        new Uri($"https://{accountName}.blob.core.windows.net/{containerName}"),
        new DefaultAzureCredential(),
        clientOptions);

    return client;
}
```

## Managing Container Properties and Metadata with .NET

## Objectives

- Understand system properties and user-defined metadata in Blob storage.
- Retrieve container properties using .NET.
- Set and retrieve metadata for containers using .NET.

## System Properties and Metadata Overview

Blob containers in Azure support system properties and user-defined metadata, providing additional context and management capabilities for the data they contain.

- **System Properties:**
  - Readable or writable properties maintained by the Azure Storage client library for .NET.
  - Correspond to standard HTTP headers.
  - Examples: Public access level, Last modified time.

- **User-defined Metadata:**
  - Name-value pairs specified by users for additional resource context.
  - Metadata names must conform to HTTP header restrictions and C# identifiers.
  - Metadata values can contain non-ASCII characters, which should be Base64-encoded or URL-encoded.

## Retrieving Container Properties

To fetch container properties, utilize methods provided by the `BlobContainerClient` class in .NET:

- `GetProperties`
- `GetPropertiesAsync`

### Example

```csharp
private static async Task ReadContainerPropertiesAsync(BlobContainerClient container)
{
    try
    {
        var properties = await container.GetPropertiesAsync();
        Console.WriteLine($"Properties for container {container.Uri}");
        Console.WriteLine($"Public access level: {properties.Value.PublicAccess}");
        Console.WriteLine($"Last modified time in UTC: {properties.Value.LastModified}");
    }
    catch (RequestFailedException e)
    {
        Console.WriteLine($"HTTP error code {e.Status}: {e.ErrorCode}");
        Console.WriteLine(e.Message);
    }
}
```

## Setting and Retrieving Metadata

- **Setting Metadata:**
  - Construct an `IDictionary<string, string>` object containing metadata name-value pairs.
  - Call `SetMetadataAsync` method of `BlobContainerClient` to write metadata values.
- **Retrieving Metadata:**
  - Use `GetProperties` or `GetPropertiesAsync` methods to retrieve metadata.
  - Iterate through the metadata items to access keys and values.

### Example Setting Metadata

```csharp
public static async Task AddContainerMetadataAsync(BlobContainerClient container)
{
    try
    {
        IDictionary<string, string> metadata = new Dictionary<string, string>();
        metadata.Add("docType", "textDocuments");
        metadata.Add("category", "guidance");
        await container.SetMetadataAsync(metadata);
    }
    catch (RequestFailedException e)
    {
        Console.WriteLine($"HTTP error code {e.Status}: {e.ErrorCode}");
        Console.WriteLine(e.Message);
    }
}
```

### Example Retrieving Metadata

```csharp
public static async Task ReadContainerMetadataAsync(BlobContainerClient container)
{
    try
    {
        var properties = await container.GetPropertiesAsync();
        Console.WriteLine("Container metadata:");
        foreach (var metadataItem in properties.Value.Metadata)
        {
            Console.WriteLine($"\tKey: {metadataItem.Key}");
            Console.WriteLine($"\tValue: {metadataItem.Value}");
        }
    }
    catch (RequestFailedException e)
    {
        Console.WriteLine($"HTTP error code {e.Status}: {e.ErrorCode}");
        Console.WriteLine(e.Message);
    }
}
```

## Key Takeaways

- Understand the distinction between system properties and user-defined metadata.
- Utilize .NET methods to retrieve container properties and manage metadata effectively.
- Practice setting and retrieving metadata to understand their behavior in different scenarios.

## Additional Resources

- [Azure Blob storage documentation](https://docs.microsoft.com/azure/storage/blobs/)
- [Azure SDK for .NET documentation](https://docs.microsoft.com/azure/developer/dotnet/)
- [Azure Blob Storage .NET quickstarts](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-dotnet/)

## Setting and Retrieving Blob Properties and Metadata

Blobs in Azure Blob Storage support custom metadata, represented as HTTP headers. You can set and retrieve these metadata headers using REST API operations.

### Metadata Header Format

Metadata headers are name-value pairs, with the following format:

```
x-ms-meta-name:string-value
```

Metadata names must adhere to the naming rules for C# identifiers and are case-insensitive. If multiple headers with the same name are submitted, the Blob service will return a 400 (Bad Request) status code.

The total size of all metadata pairs cannot exceed 8KB.

### Retrieving Properties and Metadata

You can retrieve metadata headers for a container or blob using the `GET` or `HEAD` operations. The URI syntax is:

For containers:

```
GET/HEAD https://myaccount.blob.core.windows.net/mycontainer?restype=container
```

For blobs:

```
GET/HEAD https://myaccount.blob.core.windows.net/mycontainer/myblob?comp=metadata
```

These operations only return the headers, not the content of the resource.

### Setting Metadata Headers

You can set metadata headers on a container or blob using the `PUT` operation. This overwrites any existing metadata on the resource. To clear all metadata, call `PUT` without any headers.

The URI syntax is:

For containers:

```
PUT https://myaccount.blob.core.windows.net/mycontainer?comp=metadata&restype=container
```

For blobs:

```
PUT https://myaccount.blob.core.windows.net/mycontainer/myblob?comp=metadata
```

### Standard HTTP Properties

Containers and blobs also support certain standard HTTP properties, such as:

- For containers: `ETag`, `Last-Modified`
- For blobs: `ETag`, `Last-Modified`, `Content-Length`, `Content-Type`, `Content-MD5`, `Content-Encoding`, `Content-Language`, `Cache-Control`, `Origin`, `Range`

These standard HTTP properties are represented as standard HTTP headers, rather than the custom `x-ms-meta-` prefix used for metadata.


