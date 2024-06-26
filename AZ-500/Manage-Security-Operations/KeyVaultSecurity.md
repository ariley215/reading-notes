# Azure Key Vault security

## Network Security

- **IP Restrictions**: Control which IP addresses are allowed to access the Key Vault, adding a layer of security by limiting access to specific networks.

- **Virtual Network Service Endpoints**: Integrate Key Vault with Azure Virtual Networks (VNets) to allow access from specific subnets, enhancing security by isolating the network traffic to the Key Vault.

- **Azure Private Link**: Connect to Azure Key Vault using a private endpoint, which is a network interface within your VNet, providing secure and private connectivity to Azure services.

## Transport Layer Security (TLS) and HTTPS

- **TLS Support**: Ensure that all communications to and from the Key Vault are encrypted using Transport Layer Security, which provides confidentiality and integrity to data in transit.

- **HTTPS Requirement**: Access Key Vault exclusively over HTTPS, which is HTTP over TLS/SSL, to secure the communication channel and prevent interception of data.

## Key Vault Authentication Options

- **Application-only Access**: Authenticate to Key Vault using a service principal or managed identity without user intervention, typically used in automated workflows or services.

- **User-only Access**: Access Key Vault directly as an individual user with Azure Active Directory credentials, often used for manual administrative tasks.

- **Application-plus-user Access**: Combine user credentials and application identity to access Key Vault, providing a dual layer of authentication and enabling scenarios such as user-consent workflows.

## Access Model Overview

- **Management Plane**: Interact with the Key Vault at the management level to handle tasks like creating and deleting vaults, updating access policies, and viewing properties.

- **Data Plane**: Perform operations on the contents of the Key Vault, such as adding or retrieving keys, secrets, and certificates, which are the core functionalities of the Key Vault service.

- **Authentication**: Verify the identity of users or applications requesting access to Key Vault through Azure Active Directory, which provides a secure authentication mechanism.

- **Authorization**: Control what authenticated users or applications are allowed to do with Key Vault by using Azure Role-Based Access Control (RBAC) and Key Vault-specific access policies.

## Conditional Access

- **Conditional Access Policies**: Enforce additional access requirements based on conditions such as user location, device compliance, and risk level, further securing Key Vault by preventing unauthorized access.

## Privileged Access

- **Azure RBAC**: Utilize Azure's built-in role-based access control system to assign specific roles to users or groups, determining what administrative actions they can perform on Key Vault.

- **Key Vault Access Policies**: Define fine-grained permissions for Key Vault operations, allowing you to specify which users or applications can perform actions on keys, secrets, or certificates.

## Managing Administrative Access to Key Vault

- **Role Assignments**: Determine who has administrative access to manage Key Vaults by assigning Azure roles at different scopes, such as the subscription, resource group, or individual resource level.

- **Contributor Role Caution**: Be cautious when assigning the Contributor role as it can modify Key Vault access policies, potentially leading to unauthorized data plane access.

## Controlling Access to Key Vault Data

- **Azure RBAC vs Key Vault Access Policies**: Choose between Azure RBAC, which provides a consistent permission model across Azure services, or Key Vault's access policies, which offer more granular control over specific Key Vault operations.

## Logging and Monitoring

- **Key Vault Logging**: Enable and configure logging for Key Vault to audit access and actions, ensuring you have records for compliance and security investigation purposes.

- **Event Grid Integration**: Leverage Azure Event Grid to subscribe to Key Vault events, allowing you to respond to specific activities such as the expiration of secrets or keys.

- **Monitoring**: Set up Azure Monitor to track the performance and health of Key Vault, alerting you to potential issues or anomalies that could indicate security concerns.

## Backup and Recovery

- **Soft-Delete**: Protect against accidental or malicious deletion of Key Vault items by retaining deleted items for a specified period, during which they can be recovered.

- **Purge Protection**: Prevent permanent deletion of Key Vault items until the soft-delete retention period has passed, offering an additional layer of protection against data loss.

- **Backup Strategy**: Implement a strategy for backing up Key Vault contents, such as keys and certificates, to ensure that you can restore them in case of data corruption or loss.

## Real World Application

- Implement IP restrictions to protect a financial institution's Key Vault containing sensitive customer data.
- Use Azure Private Link to ensure a retail company's transaction processing system securely accesses Key Vault.
- Enforce TLS 1.3 for a healthcare application to comply with industry standards for protected health information.
- Apply Conditional Access policies for a law firm to restrict Key Vault access based on location and device state.
- Configure logging and set up alerts for a tech company to monitor and respond to unauthorized Key Vault access attempts.