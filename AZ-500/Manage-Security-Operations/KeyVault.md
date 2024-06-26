# Azure Key Vault

## Overview of Azure Key Vault

- **Purpose**: Azure Key Vault is a service for securely storing and accessing secrets, keys, and certificates.

- **Containers**:
  - **Vaults**: Store software and HSM-backed keys, secrets, and certificates.
  - **Managed HSM pools**: Only support HSM-backed keys.

## Important Terms

- **Tenant**: The organization owning and managing Microsoft cloud services.
- **Vault Owner**: Can create a key vault, control access, and set up auditing.
- **Vault Consumer**: Can access assets in the key vault with granted permissions.
- **Managed HSM Administrators**: Have complete control over a Managed HSM pool.
- **Managed HSM Crypto Officer/User**: Perform cryptographic operations; cannot delete keys.
- **Managed HSM Crypto Service Encryption User**: Assigned to service accounts for data encryption at rest.
- **Resource**: A manageable item in Azure, e.g., VM, storage account, web app.
- **Resource Group**: A container for related Azure resources.
- **Security Principle**: A security identity used by apps and services for accessing Azure resources.
- **Microsoft Entra ID**: Directory service for a tenant.
- **Azure Tenant ID**: Unique identifier for a Microsoft Entra instance within an Azure subscription.
- **Managed Identities**: Automatically managed identities in Microsoft Entra ID for Azure services.

## Authentication to Azure Key Vault

- **Managed Identities for Azure Resources**: Recommended method for automatic identity rotation.
- **Service Principal and Certificate**: Not recommended due to the need for manual certificate rotation.
- **Service Principal and Secret**: Not recommended due to difficulties in secret rotation.

## Encryption of Data in Transit

- **TLS Protocol**: Azure Key Vault enforces TLS for data protection during transit.
- **Perfect Forward Secrecy (PFS)**: Ensures unique keys for each connection.
- **RSA-based Encryption**: Uses 2,048-bit encryption key lengths for secure connections.

## Key Vault Roles and Use Cases

| Role                                | Problem Statement | Azure Key Vault Solution |
| ----------------------------------- | ----------------- | ------------------------ |
| Developer for an Azure application  | Requires external keys for signing/encryption with protection and ease of use. | √ Keys stored and invoked by URI. <br>√ Safeguarded by Azure HSMs. <br>√ Processed in HSMs in Azure datacenters. |
| Developer for SaaS                  | Wants customers to own/manage their keys without liability. | √ Customers manage their keys. <br>√ Key Vault performs cryptographic operations. |
| Chief Security Officer (CSO)        | Needs FIPS 140-2 Level 2/3 HSMs compliance, key lifecycle control, and centralized management. | √ FIPS 140-2 Level 2/3 HSMs support. <br>√ Microsoft does not see keys. <br>√ Real-time logging. <br>√ Single interface for management. |

## Operational Tasks by Azure Administrators

- **Create or import**: Keys or secrets.
- **Revoke or delete**: Keys or secrets.
- **Authorize access**: For users or applications to manage or use keys and secrets.
- **Configure key usage**: Define actions like sign or encrypt.
- **Monitor key usage**: Keep track of how and when keys are used.
![Key Vault Roles](https://learn.microsoft.com/en-us/training/wwl-azure/governance-security/media/azure-key-vault-7aacaffa.png)

## Additional Resources

- [Azure Key Vault Documentation](https://docs.microsoft.com/en-us/azure/key-vault/)
- [Azure Key Vault Best Practices](https://docs.microsoft.com/en-us/azure/key-vault/general/best-practices)
- [Azure Managed Identities](https://docs.microsoft.com/en-us/azure/active-directory/managed-identities-azure-resources/overview)
- [Microsoft Entra](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/active-directory-whatis)

---

**Note**: Azure Key Vault is a pivotal component in managing cryptographic keys and secrets for Azure applications. It is essential for security professionals to understand its capabilities, authentication mechanisms, and best practices for managing keys and secrets in the cloud.