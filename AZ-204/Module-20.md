# Implement Managed Identities

Objectives:

- Explain the differences between the two types of managed identities
- Describe the flows for user- and system-assigned managed identities
- Configure managed identities
- Acquire access tokens by using REST and code

# Exploring Managed Identities

- Managed identities eliminate the need for developers to manage credentials like secrets, certificates, and keys used for secure communication
- Managed identities provide an automatically managed identity in Microsoft Entra ID for applications to authenticate to resources that support Microsoft Entra

## Types of Managed Identities

- System-assigned managed identity:
  - Enabled directly on an Azure service instance
  - Azure creates the identity in the Microsoft Entra tenant
  - Lifecycle tied to the Azure service instance
  - Deleted when the instance is deleted
- User-assigned managed identity:
  - Created as a standalone Azure resource
  - Azure creates the identity in the Microsoft Entra tenant
  - Can be assigned to one or more Azure service instances
  - Lifecycle managed separately from Azure service instances

- Both are represented as service principals locked to only be used with Azure resources
- Deleted managed identity removes the corresponding service principal

## Characteristics of Managed Identities

| Characteristic | System-assigned | User-assigned |
| --- | --- | --- |
| Creation | Created with Azure resource | Created as standalone resource |
| Lifecycle | Shared with Azure resource | Independently managed |
| Sharing | Can't be shared, 1:1 with resource | Can be shared across resources |

## When to Use Managed Identities

![Managed Identity Senarios](https://learn.microsoft.com/en-us/training/wwl-azure/implement-managed-identities/media/managed-identities-use-case.png)

- Scenarios where an app needs to access other Azure services without managing credentials
- Example: Azure App Service app accessing Azure Storage without credential management

## Supported Azure Services

- Managed identities can authenticate to any Azure service that supports Microsoft Entra
- Full list available in the "Services that support managed identities for Azure resources" docs
- Examples in this module use Azure VMs, but concepts apply to any supported resource

# Discover the Managed Identities Authentication Flow

## Completed

100 XP
3 minutes

This unit explains how managed identities work with Azure virtual machines (VMs).

## System-Assigned Managed Identity Flow

1. Azure Resource Manager receives a request to enable system-assigned managed identity on an Azure VM.
2. Azure Resource Manager creates a service principal in Microsoft Entra ID for the VM's identity, in the Microsoft Entra tenant trusted by the subscription.
3. Azure Resource Manager configures the identity on the VM by updating the Azure Instance Metadata Service identity endpoint with the service principal client ID and certificate.
4. Grant the VM's service principal access to Azure resources using role-based access control in Microsoft Entra ID.
5. Code running on the VM requests a token from the Azure Instance Metadata Service endpoint (`http://169.254.169.254/metadata/identity/oauth2/token`).
6. Microsoft Entra ID returns a JWT access token.
7. The code sends the access token on calls to services that support Microsoft Entra authentication.

## User-Assigned Managed Identity Flow

1. Azure Resource Manager receives a request to create a user-assigned managed identity.
2. Azure Resource Manager creates a service principal in Microsoft Entra ID for the user-assigned identity, in the Microsoft Entra tenant trusted by the subscription.
3. Azure Resource Manager configures the user-assigned identity on an Azure VM by updating the Azure Instance Metadata Service identity endpoint with the service principal client ID and certificate.
4. Grant the user-assigned identity's service principal access to Azure resources using role-based access control in Microsoft Entra ID.
5. Code running on the VM requests a token from the Azure Instance Metadata Service identity endpoint (`http://169.254.169.254/metadata/identity/oauth2/token`).
6. Microsoft Entra ID returns a JWT access token.
7. The code sends the access token on calls to services that support Microsoft Entra authentication.

> **Note**: Step 4 can also be done before step 3.

**The key difference between the two flows is that a system-assigned identity is created and managed along with the Azure resource, while a user-assigned identity is a standalone resource that can be assigned to one or more Azure resources.**

# Configuring Managed Identities in Azure

## Overview

Managed identities in Azure provide a way for Azure services to authenticate to other Azure services without the need for hardcoded credentials. This simplifies the authentication process and improves security by eliminating the need to manage and store credentials.

## Configuring Managed Identities

### System-Assigned Managed Identity

To configure a system-assigned managed identity on an Azure virtual machine, you can use the following steps:

1. **Create a VM with System-Assigned Managed Identity**: Use the Azure CLI command `az vm create` with the `--assign-identity` parameter to create a new VM with a system-assigned managed identity.

   ```bash
   az vm create --resource-group myResourceGroup \
                --name myVM \
                --image win2016datacenter \
                --generate-ssh-keys \
                --assign-identity \
                --role contributor \
                --scope mySubscription \
                --admin-username azureuser \
                --admin-password myPassword12
   ```

2. **Enable System-Assigned Managed Identity on an Existing VM**: Use the `az vm identity assign` command to assign the system-assigned identity to an existing virtual machine.

   ```bash
   az vm identity assign -g myResourceGroup -n myVm
   ```

### User-Assigned Managed Identity

Configuring a user-assigned managed identity involves two steps:

1. **Create a User-Assigned Managed Identity**: Use the `az identity create` command to create a user-assigned managed identity.

   ```bash
   az identity create -g myResourceGroup -n myUserAssignedIdentity
   ```

2. **Assign the User-Assigned Managed Identity to a VM**: Use the `az vm create` or `az vm identity assign` commands to assign the user-assigned managed identity to a new or existing VM.

   ```bash
   az vm create --resource-group <RESOURCE GROUP> \
                --name <VM NAME> \
                --image Ubuntu2204 \
                --admin-username <USER NAME> \
                --admin-password <PASSWORD> \
                --assign-identity <USER ASSIGNED IDENTITY NAME> \
                --role <ROLE> \
                --scope <SUBSCRIPTION>
   ```

   ```bash
   az vm identity assign -g <RESOURCE GROUP> \
                         -n <VM NAME> \
                         --identities <USER ASSIGNED IDENTITY>
   ```

## Azure SDK Support

Azure provides managed identity support in various programming languages through its SDK offerings. Here are some examples:

- .NET: [Manage resource from a virtual machine enabled with managed identities for Azure resources](https://docs.microsoft.com/en-us/azure/active-directory/managed-identities-azure-resources/how-to-use-vm-token#c)
- Java: [Manage storage from a virtual machine enabled with managed identities for Azure resources](https://docs.microsoft.com/en-us/azure/active-directory/managed-identities-azure-resources/how-to-use-vm-token#java)
- Node.js: [Create a virtual machine with system-assigned managed identity enabled](https://docs.microsoft.com/en-us/azure/active-directory/managed-identities-azure-resources/how-to-use-vm-token#nodejs)
- Python: [Create a virtual machine with system-assigned managed identity enabled](https://docs.microsoft.com/en-us/azure/active-directory/managed-identities-azure-resources/how-to-use-vm-token#python)
- Ruby: [Create Azure virtual machine with an system-assigned identity enabled](https://docs.microsoft.com/en-us/azure/active-directory/managed-identities-azure-resources/how-to-use-vm-token#ruby)

# Acquiring an Access Token with Managed Identities

## Overview

- Managed identities provide a way for Azure services to authenticate to other Azure services without hardcoded credentials.
- This includes the ability to acquire access tokens that can be used to authenticate to other resources.

## Using DefaultAzureCredential

- Recommended method to acquire access tokens using managed identities.
- DefaultAzureCredential automatically attempts authentication via multiple mechanisms:
  - Environment variables: Reads account information from environment variables and uses it to authenticate.
  - Managed identity: If the application is deployed to an Azure host with Managed Identity enabled, it authenticates with that account.
  - Visual Studio, Azure CLI, Azure PowerShell: Authenticates with accounts configured in these tools.
  - Interactive browser: Can be used to interactively authenticate the developer (disabled by default).
- Example:

  ```csharp
  // Create a secret client using the DefaultAzureCredential
  var client = new SecretClient(new Uri("https://myvault.vault.azure.net/"), new DefaultAzureCredential());
  ```

  This creates a SecretClient from the Azure.Security.KeyVault.Secrets library and authenticates it using the DefaultAzureCredential.

## Specifying a User-Assigned Managed Identity

- Configure DefaultAzureCredential to use a specific user-assigned managed identity.
- Useful when deploying to an Azure host.
- Example:

  ```csharp
  // When deployed to an azure host, the default azure credential will authenticate the specified user-assigned managed identity.
  string userAssignedClientId = "<your managed identity client Id>";
  var credential = new DefaultAzureCredential(new DefaultAzureCredentialOptions { ManagedIdentityClientId = userAssignedClientId });
  var blobClient = new BlobClient(new Uri("https://myaccount.blob.core.windows.net/mycontainer/myblob"), credential);
  ```

  This creates a BlobClient from the Azure.Storage.Blobs library and authenticates it using the DefaultAzureCredential configured to use the specified user-assigned managed identity.

## Customizing the Authentication Flow

- ChainedTokenCredential allows combining multiple credential instances to define a customized authentication flow.
- Useful for advanced scenarios where you want more control over the authentication process.
- Example:

  ```csharp
  // Authenticate using managed identity if it is available; otherwise use the Azure CLI to authenticate.
  var credential = new ChainedTokenCredential(new ManagedIdentityCredential(), new AzureCliCredential());
  var eventHubProducerClient = new EventHubProducerClient("myeventhub.eventhubs.windows.net", "myhubpath", credential);
  ```

  This creates an EventHubProducerClient from the Azure.Messaging.EventHubs library and authenticates it using a ChainedTokenCredential that first tries to authenticate with managed identity, and falls back to the Azure CLI if managed identity is not available.

## Key Takeaways

- Use DefaultAzureCredential to simplify access token acquisition.
- Configure DefaultAzureCredential to use a specific user-assigned managed identity.
- Customize authentication flow with ChainedTokenCredential for advanced scenarios.
- Refer to latest Azure Identity library documentation for updates and additional examples.

