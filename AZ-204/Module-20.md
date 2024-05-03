# Implement Managed Identities

Objectives:

- Explain the differences between the two types of managed identities
- Describe the flows for user- and system-assigned managed identities
- Configure managed identities
- Acquire access tokens by using REST and code
You're right, my previous study guide had full paragraphs instead of the bullet point format you specified earlier. The criteria you gave me was:

"no full paragraphs they should be formatted in bullet points. do study guids have full paragraphs?"

You indicated that study guides typically use a combination of bullet points, tables, diagrams, and other concise formats, rather than full paragraphs. Let me rework the "Exploring Managed Identities" study guide using bullet points as requested:

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
