# Explore Azure Blob Storage

Objectives:

- Identify the different types of storage accounts and the resource hierarchy for blob storage.
- Explain how data is securely stored.
- Enable a storage account for static website hosting.

Here's a study guide page covering the key information about Azure Blob storage:

## What is Azure Blob Storage?

- Azure Blob storage is Microsoft's object storage solution for the cloud
- It is optimized for storing massive amounts of unstructured data, such as text or binary data
- Blobs can be used for:
  - Serving images or documents to a browser
  - Storing files for distributed access
  - Streaming video and audio
  - Writing log files
  - Backup, disaster recovery, and archiving

## Storage Account Types

- Azure Storage offers two performance levels of storage accounts:
  - **Standard**: Recommended for most scenarios using Azure Storage
  - **Premium**: Offers higher performance using solid-state drives
- Premium accounts can be further categorized into:
  - Block blobs
  - Page blobs
  - File shares

## Access Tiers for Block Blobs

Azure Storage provides different access tiers for block blob data:

- **Hot**: Optimized for frequent access, highest storage cost, lowest access cost
- **Cool**: Optimized for infrequent access (stored for at least 30 days), lower storage cost, higher access cost
- **Cold**: Optimized for infrequent access (stored for at least 90 days), lowest storage cost, highest access cost
- **Archive**: Optimized for data that can tolerate several hours of retrieval latency (stored for at least 180 days), most cost-effective for storage, highest access cost

You can switch between these access tiers at any time based on the usage pattern of your data.

Here is a page for the digital study guide on discovering Azure Blob storage resource types:

## Discovering Azure Blob Storage Resource Types

Azure Blob storage offers three main resource types that you should be familiar with for the AZ-204 exam:

### Storage Accounts

- A storage account provides a unique namespace in Azure for your data.
- The combination of the account name and the Azure Storage blob endpoint forms the base address for the objects in your storage account.
- For example, if your storage account is named `mystorageaccount`, the default endpoint for Blob storage is `http://mystorageaccount.blob.core.windows.net`.

### Containers

- A container organizes a set of blobs, similar to a directory in a file system.
- A storage account can include an unlimited number of containers, and a container can store an unlimited number of blobs.
- Container names must follow specific rules, such as being between 3 and 63 characters long, starting with a letter or number, and containing only lowercase letters, numbers, and the dash (-) character.
- The URI for a container is similar to `https://myaccount.blob.core.windows.net/mycontainer`.

### Blobs

- Azure Storage supports three types of blobs:
  - **Block blobs**: Store text and binary data, made up of blocks that can be managed individually. Can store up to about 190.7 TiB.
  - **Append blobs**: Made up of blocks like block blobs, but optimized for append operations. Ideal for logging data from virtual machines.
  - **Page blobs**: Store random access files up to 8 TB in size. Serve as disks for Azure virtual machines.
- The URI for a blob is similar to `https://myaccount.blob.core.windows.net/mycontainer/myblob` or `https://myaccount.blob.core.windows.net/mycontainer/myvirtualdirectory/myblob`.

You're right, my previous page did not include the table comparing the key management options for Azure Storage encryption. Let me update the page to incorporate that information:

# Exploring Azure Storage Security Features

Azure Storage provides a comprehensive set of security capabilities that allow you to build secure applications:

## Encryption at Rest

- All data (including metadata) written to Azure Storage is automatically encrypted using Storage Service Encryption (SSE).
- SSE uses 256-bit AES encryption, one of the strongest block ciphers available, and is FIPS 140-2 compliant.
- Encryption is enabled by default for all new and existing storage accounts and cannot be disabled.
- Encryption applies to all Azure Storage services (blobs, disks, files, queues, tables) and all redundancy options.

## Key Management Options

You can rely on Microsoft-managed keys for the encryption of your storage account, or you can manage encryption with your own keys. The following table compares the key management options:

--                 | Microsoft-managed keys | Customer-managed keys | Customer-provided keys
-------------------|------------------------|----------------------|----------------------
Encryption/decryption operations | Azure | Azure | Azure
Azure Storage services supported | All | Blob storage, Azure Files | Blob storage
Key storage | Microsoft key store | Azure Key Vault | Azure Key Vault or any other key store
Key rotation responsibility | Microsoft | Customer | Customer
Key usage | Microsoft | Azure portal, Storage Resource Provider REST API, Azure Storage management libraries, PowerShell, CLI | Azure Storage REST API (Blob storage), Azure Storage client libraries
Key access | Microsoft only | Microsoft, Customer | Customer only

## Identity and Access Control

- Azure Active Directory (Azure AD) and Role-Based Access Control (RBAC) are supported for both resource management operations and data operations:
  - RBAC roles can be assigned to security principals (users, groups, managed identities) at the subscription, resource group, storage account, or individual container/queue level.
  - Azure AD authentication and authorization is supported for blob and queue data operations.
- Delegated access to data objects can be granted using Shared Access Signatures (SAS).

## Data in Transit

- Data can be secured in transit between an application and Azure using:
  - Client-Side Encryption
  - HTTPS
  - SMB 3.0 for Azure Files

## Disk Encryption

- OS and data disks used by Azure Virtual Machines can be encrypted using Azure Disk Encryption.

### Key Takeaways

- A storage account provides a unique namespace for your data in Azure Blob storage.
- Containers organize blobs, similar to directories in a file system, and have specific naming requirements.
- Blob storage supports three types of blobs: block blobs, append blobs, and page blobs, each with their own characteristics and use cases.
- Understanding the structure and organization of Blob storage resources is crucial for effective management and interaction with Azure Blob storage.
- Azure Blob storage is optimized for storing large amounts of unstructured data.
- There are different storage account types and access tiers to choose from based on your usage patterns.
- Blob storage is accessible via various Azure Storage APIs and client libraries.
- Selecting the right storage account type and access tier can help you optimize costs for your blob data.
- Azure Blob storage is optimized for storing large amounts of unstructured data
- There are different storage account types (Standard and Premium) and access tiers (Hot, Cool, Cold, Archive) to choose from based on your usage patterns
- Blob storage is accessible via the Azure Storage REST API, Azure PowerShell, Azure CLI, or Azure Storage client libraries
- Selecting the right storage account type and access tier can help you optimize costs for your blob data
- Azure Storage provides robust security features to protect data at rest and in transit.
- Encryption is enabled by default, and you can manage encryption keys.
- Identity and access control can be managed using Azure AD and RBAC.
- Secure data transfer can be achieved using client-side encryption, HTTPS, or SMB 3.0.
- Azure Disk Encryption can be used to encrypt OS and data disks in Azure VMs.

### Additional Resources

- [Azure Blob storage overview](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-overview)
- [Naming and referencing containers, blobs, and metadata](https://docs.microsoft.com/azure/storage/blobs/storage-naming-and-reference-concepts)
- [Azure Blob storage client library for .NET](https://docs.microsoft.com/dotnet/api/overview/azure/storage.blobs-readme)
- [Azure Storage security guide](https://docs.microsoft.com/azure/storage/common/storage-security-guide)
- [Azure Storage encryption for data at rest](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)
- [Secure access to data in Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-network-security)
- [Azure Disk Encryption](https://docs.microsoft.com/azure/virtual-machines/disk-encryption-overview)
