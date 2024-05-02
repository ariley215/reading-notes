# Implement Azure Key Vault

Objectives:

- Describe the benefits of using Azure Key Vault
- Explain how to authenticate to Azure Key Vault
- Set and retrieve a secret from Azure Key Vault by using the Azure CLI

# Exploring Azure Key Vault

## Overview

Azure Key Vault is a cloud-hosted service that provides a secure and centralized store for application secrets, keys, and certificates. It helps solve the following key challenges:

1. **Secrets Management**: Securely store and tightly control access to tokens, passwords, certificates, API keys, and other secrets.
2. **Key Management**: Create and control the encryption keys used to encrypt your data.
3. **Certificate Management**: Easily provision, manage, and deploy SSL/TLS certificates for use with Azure and internal resources.

## Service Tiers

Azure Key Vault offers two service tiers:

1. **Standard**: Encrypts data with a software key.
2. **Premium**: Includes hardware security module (HSM)-protected keys for added security.

## Key Benefits

1. **Centralized Application Secrets**: Store application secrets securely, and control their distribution to applications.
2. **Secure Storage**: Proper authentication and authorization are required to access a key vault.
3. **Access Monitoring**: Monitor activity by enabling logging for your vaults, and secure the logs by restricting access.
4. **Simplified Administration**: Simplifies the process of managing security information, including scaling, replicating, and automating certain tasks.

## Use Cases

Some common use cases for Azure Key Vault include:

- Storing and managing encryption keys for Azure services and applications.
- Securely storing and accessing sensitive application and service settings, such as connection strings, API keys, and passwords.
- Provisioning and managing SSL/TLS certificates for Azure and on-premises resources.
- Securing confidential data used by applications, services, and automated processes.

By leveraging Azure Key Vault, organizations can improve the security and management of their sensitive data, reduce the overhead of managing on-premises hardware security modules, and ensure high availability of their critical security assets.

# Azure Key Vault Best Practices

## Authentication

- **Preferred**: Use **Managed Identities for Azure Resources** - Assign an identity to your Azure resources and grant that identity access to Key Vault. Azure will automatically rotate the associated service principal client secret.
- Avoid using **Service Principal and Certificate** or **Service Principal and Secret** approaches
    - They require manual certificate rotation or bootstrap secret rotation, respectively.

## Encryption of Data in Transit

- Azure Key Vault enforces **Transport Layer Security (TLS)** protocol to protect data in transit.
- Connections use **Perfect Forward Secrecy (PFS)** and **2,048-bit RSA encryption** for enhanced security.

## Key Vault Configuration Best Practices

1. **Use Separate Vaults**: Use a separate vault per application, per environment (Dev, Pre-Prod, Prod) to isolate secrets and reduce breach impact.
2. **Control Access**: Secure vault access using **Azure RBAC** and **Key Vault access policies**.
3. **Backup Vaults**: Create regular backups of your vaults, especially when updating, deleting, or creating objects.
4. **Enable Logging & Alerts**: Turn on logging and set up alerts to monitor activity and detect suspicious behavior.
5. **Enable Recovery Options**: Enable **soft-delete** and **purge protection** to guard against accidental or malicious deletion.
6. **Rotate Secrets Regularly**: Establish a process to regularly rotate secrets, such as API keys and passwords.
7. **Limit Permissions**: Grant the minimum necessary permissions to applications and users to access the secrets they need.
8. **Integrate with Other Services**: Leverage other Azure services, such as Managed Identities, to simplify authentication and authorization.

Got it, let's include the code snippets for authenticating to Azure Key Vault.

# Authenticating to Azure Key Vault

## Overview

- Azure Key Vault is a cloud-based service for securely storing and accessing sensitive data
- There are two primary ways to authenticate to Key Vault:
  - Managed Identity
  - Application Registration

## Managed Identity

- Recommended approach
- Azure internally manages the application's service principal
- Automatically authenticates the application with other Azure services
- Available for applications deployed to Azure services like App Service, Functions, and Virtual Machines
- Enable a system-assigned managed identity for your application

## Application Registration

- Register the application with your Microsoft Entra tenant
- Creates a service principal that identifies the application across all tenants
- Use if managed identity is not an option

## Authenticating in Application Code

- Use the Azure Identity client library
  - Consistent authentication experience across environments and languages
- .NET, Python, Java, and JavaScript SDKs available
- Example in .NET:

```csharp
using Azure.Identity;
using Azure.Security.KeyVault.Secrets;

var client = new SecretClient(new Uri("https://myvault.vault.azure.net/"), new DefaultAzureCredential());
var secret = await client.GetSecretAsync("my-secret");
```

## Authenticating with REST API

- Include an access token in the `Authorization` header
- If token is missing or invalid, receive HTTP 401 error with `WWW-Authenticate` header
- `WWW-Authenticate` header provides info about the authorization service and resource
- Example HTTP request:

```http
PUT /keys/MYKEY?api-version=<api_version> HTTP/1.1
Authorization: Bearer <access_token>
```

## Additional Resources

- [Azure Key Vault developer's guide](https://docs.microsoft.com/azure/key-vault/general/developers-guide)
- [Azure Key Vault availability and redundancy](https://docs.microsoft.com/azure/key-vault/general/disaster-recovery-guidance)
