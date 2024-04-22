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