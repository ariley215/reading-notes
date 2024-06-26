# Azure Key Vault authentication

## Overview of Key Vault Authentication

Authentication with Azure Key Vault works in conjunction with Microsoft Entra ID (formerly known as Azure Active Directory), which authenticates the identity of any given security principal.

### What is a Security Principal?

- A security principal is an object that represents an entity requesting access to Azure resources.
- Azure assigns a unique object ID to every security principal.

### Types of Security Principals

- **User Security Principal**: Identifies an individual user with a profile in Microsoft Entra ID.
- **Group Security Principal**: Identifies a set of users within a group in Microsoft Entra ID. Any assigned roles or permissions are inherited by all members.
- **Service Principal**: Identifies an application or service rather than an individual or group. It uses an object ID and a client secret for authentication.

### Application Authentication Methods

1. **Managed Identity (Recommended)**: Azure manages the application's service principal and automatically authenticates the application with other Azure services.
2. **Application Registration**: If managed identity is not feasible, register the application with your Microsoft Entra tenant to create a service principal.

## Configure the Key Vault Firewall

- Key Vault can restrict access to specified IP ranges, service endpoints, virtual networks, or private endpoints.
- By default, Key Vault is accessible through public IP addresses.

## Key Vault Request Operation Flow

1. **Token Request**: An Azure resource or user requests authentication with Microsoft Entra ID to obtain an OAuth token.
2. **Key Vault API Call**: The authenticated entity makes a call to the Key Vault REST API.
3. **Firewall Check**: Key Vault Firewall evaluates if the call meets any of the following criteria:
   - Firewall is disabled, allowing access from the internet.
   - Caller is a Key Vault Trusted Service, bypassing the firewall.
   - Caller's IP address, VNet, or service endpoint is on the firewall's allow list.
   - Caller has a private link connection to Key Vault.
4. **Token Validation**: If the firewall allows, Key Vault validates the access token with Microsoft Entra ID.
5. **Permission Check**: Key Vault checks if the security principal has the necessary permissions.
6. **Operation Execution**: If permissions are valid, Key Vault performs the requested operation and returns the result.

![Key Vault Request Operation Flow](https://learn.microsoft.com/en-us/training/wwl-azure/governance-security/media/key-vault-operation-flow-73ab0ee9.png)

## Authentication in Application Code

- Azure Key Vault SDKs utilize the Azure Identity client library for authentication.
- The Azure Identity library provides a consistent authentication experience across different environments with the same codebase.

## Azure Identity Client Libraries

- **.NET**: [Azure Identity SDK .NET](https://www.nuget.org/packages/Azure.Identity/)
- **Python**: [Azure Identity SDK Python](https://pypi.org/project/azure-identity/)
- **Java**: [Azure Identity SDK Java](https://search.maven.org/artifact/com.azure/azure-identity)
- **JavaScript**: [Azure Identity SDK JavaScript](https://www.npmjs.com/package/@azure/identity)

## Note on Key Vault SDK Clients

- Key Vault SDK clients for secrets, certificates, and keys may make an initial call without an access token, resulting in a 401 response to retrieve tenant information before authenticating properly.

This guide covers the key concepts of Azure Key Vault authentication that are important for the AZ-500 certification exam. Understanding these concepts is crucial for securing access to Key Vault and managing identities within Azure.