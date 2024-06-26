# Recommend when to use a dedicated Hardware Security Module (HSM)

## Overview of Azure Dedicated HSM

Azure Dedicated HSM provides cryptographic key storage in Azure, meeting the most stringent security requirements. It is ideal for customers who require FIPS 140-2 Level 3-validated devices and complete control of the HSM appliance.

### Features of Azure Dedicated HSM

- **Global Deployment**: HSM devices are globally available across several Azure regions.
- **High Availability**: Devices can be provisioned in pairs and configured for high availability, including cross-region failover.
- **Exclusive Access**: Once provisioned, HSM devices are connected to a customer's virtual network and are only accessible by the customer.
- **Full Administrative Control**: Customers have sole administrative access after the initial password change.
- **High Performance**: The Thales Luna 7 HSM model A790 appliance offers excellent performance and supports a broad range of cryptographic algorithms.

## Reasons to Use Azure Dedicated HSM

1. **Compliance**: For organizations requiring FIPS 140-2 Level-3 validated HSMs for regulatory compliance.
2. **Single-Tenant Devices**: For customers needing a dedicated physical device for cryptographic storage.
3. **Full Administrative Control**: For customers who require sole administrative access for security or compliance reasons.
4. **High Performance**: For applications demanding high throughput and low latency in cryptographic operations.
5. **Unique Cloud-Based Offering**: For customers who need a cloud service with FIPS 140-2 Level 3 validation and extensive application integration.

## Is Azure Dedicated HSM Right for You?

### Best Fit Scenarios

- **Lift-and-Shift**: Migrating on-premises applications that require direct access to HSM devices.
- **Migration from AWS**: Transitioning from AWS EC2 and AWS Cloud HSM Classic service to Azure.
- **Shrink-Wrapped Software**: Running applications like SSL Offload, Oracle TDE, and ADCS on Azure VMs.

### Not a Fit Scenarios

- Services that support encryption with customer-managed keys but are not integrated with Azure Dedicated HSM.

### Qualification for Use

- Customers must have a Microsoft Account Manager and meet a minimum of $5M USD in committed Azure revenue annually.

### It Depends Scenarios

- **FIPS 140-2 Level 3 Requirement**: If this is a strict requirement, Azure Dedicated HSM or Azure Key Vault Managed HSM may be necessary.
- **New Code in Azure VMs**: Consider if Azure Key Vault suffices or if Dedicated HSM is needed.
- **SQL Server TDE**: When using Transparent Data Encryption in an Azure VM.
- **Client-Side Encryption**: For Azure Storage client-side encryption requirements.
- **Always Encrypted**: For SQL Server and Azure SQL Database Always Encrypted feature.

## Decision Factors

- Assess the mix of requirements, including compliance, performance, and control.
- Compare Azure Key Vault and Azure Dedicated HSM based on specific needs.
- Consider the implications of cost versus the benefits of a dedicated HSM.

