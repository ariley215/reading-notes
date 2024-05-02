# Implement Shared Access Signature

Objectives:

- Identify the three types of shared access signatures
- Explain when to implement shared access signatures
- Create a stored access policy

# Shared Access Signatures (SAS)

## Types of Shared Access Signatures

Azure Storage supports three types of shared access signatures:

1. **User Delegation SAS**:
   - Secured with Microsoft Entra credentials and the permissions specified for the SAS.
   - Applies to Blob storage only.

2. **Service SAS**:
   - Secured with the storage account key.
   - Delegates access to resources in the following storage services: Blob, Queue, Table, or Azure Files.

3. **Account SAS**:
   - Secured with the storage account key.
   - Delegates access to resources in one or more storage services.
   - Provides all the operations available via a service or user delegation SAS.

Microsoft recommends using Microsoft Entra credentials (for user delegation SAS) whenever possible, as it provides superior security compared to using the storage account key.

## How Shared Access Signatures Work

To use a SAS to access data stored in Azure Storage, you need two components:

1. **URI to the resource**: The URI to the resource you want to access.
2. **SAS token**: The SAS token that you've created to authorize access to that resource.

The SAS token is made up of several components:

| Component | Description |
| --- | --- |
| `sp=r` | Controls the access rights (add, create, delete, list, read, write) |
| `st=2020-01-20T11:42:32Z` | The start time when access is granted |
| `se=2020-01-20T19:42:32Z` | The expiration time when access ends |
| `sv=2019-02-02` | The version of the storage API to use |
| `sr=b` | The kind of storage being accessed (e.g., blob) |
| `sig=...` | The cryptographic signature |

## Best Practices

To reduce the potential risks of using a SAS, Microsoft provides the following guidance:

1. Always use HTTPS to securely distribute a SAS and prevent man-in-the-middle attacks.
2. Use a user delegation SAS whenever possible, as it removes the need to store the storage account key in your code.
3. Set the expiration time to the smallest useful value to limit the potential impact if the SAS key is compromised.
4. Apply the rule of minimum-required privileges and only grant the access that's required (e.g., read-only access).
5. In situations with unacceptable risk of using a SAS, consider creating a middle-tier service to manage users and their access to storage.

Okay, got it. Here is the document with just the last section updated:

# When to Use Shared Access Signatures (SAS)

Shared access signatures (SAS) are useful when you want to provide secure access to resources in your storage account to any client who doesn't otherwise have permissions to those resources.

Some common scenarios where a SAS can be beneficial:

## User-Uploaded/Downloaded Data

In a scenario where a storage account stores user data, there are typically two design patterns:

1. **Front-end Proxy Service**:
   - Clients upload and download data via a front-end proxy service, which performs authentication.
   - This allows validation of business rules, but can be expensive or difficult to scale for large amounts of data or high-volume transactions.

2. **SAS Provider Service**:
   - A lightweight service authenticates the client and generates a SAS.
   - The client application then uses the SAS to access the storage account resources directly, with the permissions and interval defined by the SAS.
   - This mitigates the need for routing all data through the front-end proxy service.

Many real-world services may use a hybrid of these two approaches, using the front-end proxy for some data and SAS for other data.

## Copy Operations

A SAS is required to authorize access to the source object in the following copy scenarios:

- Copying blobs between storage accounts
  - can optionally use a SAS to authorize access to the destination
- Copying files between storage accounts
  - can optionally use a SAS to authorize access to the destination
- Copying blobs to files, or files to blobs (even within the same account)

This allows secure, limited access for the copy operation without granting broader permissions.

Here's a summary of the key points about stored access policies for Shared Access Signatures (SAS) in Azure Storage:

## What are Stored Access Policies?

- Stored access policies provide an extra level of control over service-level SAS on the server side.
- They group SAS and provide more restrictions for signatures that are bound by the policy.
- You can use a stored access policy to change the start time, expiry time, or permissions for a SAS, or to revoke it after it is issued.

## Supported Storage Resources

Stored access policies can be used with the following Azure Storage resources:

- Blob containers
- File shares
- Queues
- Tables

## Creating a Stored Access Policy

- The access policy for a SAS consists of start time, expiry time, and permissions.
- These parameters can be specified in the SAS token, the stored access policy, or a combination of both.
- To create or modify a stored access policy, call the "Set ACL" operation for the resource (e.g. Set Container ACL, Set Queue ACL, etc.).
- The request body includes a unique signed identifier and the optional access policy parameters.

## Modifying or Revoking a Stored Access Policy

- To modify the stored access policy, call the "Set ACL" operation to replace the existing policy.
- To revoke a stored access policy, you can:
  - Delete the policy
  - Rename the policy by changing the signed identifier
  - Change the expiry time to a value in the past

Changing or deleting the stored access policy immediately affects all SAS associated with it.
