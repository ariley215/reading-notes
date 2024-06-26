# AZ-500: Azure Security Technologies - Manage Certificates, Secrets, and Keys

## Azure Key Vault Certificate Management

- **Creation and Import**: Certificates can be created within Azure Key Vault or imported. Imported certificates can be either self-signed or issued by a Certificate Authority (CA).
- **Key Material Interaction**: Azure Key Vault allows the management of X.509 certificates without the need for users to handle the private key material directly, enhancing security.
- **Lifecycle Management Policy**: Users can define a policy that instructs Azure Key Vault on how to automatically manage the lifecycle of a certificate, including tasks such as renewal and revocation.
- **Notification Contacts**: Users can set up contact information within Azure Key Vault to receive notifications about important lifecycle events of a certificate, such as expiration dates and renewal times.
- **Automatic Renewal**: Azure Key Vault supports the automatic renewal of certificates with selected issuers, including partner X.509 certificate providers and CAs.

## Composition of a Certificate in Azure Key Vault

- **Key and Secret**: When a certificate is created in Key Vault, it automatically creates an addressable key and a secret with the same name. The key supports key operations, and the secret allows retrieval of the certificate value.
- **Public X.509 Certificate Metadata**: This includes information such as the issuer, subject, and validity period.
- **Identifier and Versioning**: Certificates, keys, and secrets in Azure Key Vault each have a unique identifier and version. When a certificate is created or updated, a specific version of the related key and secret is made available.

## Exportable vs. Non-Exportable Keys

- **Exportable Keys**: Can be exported in PFX or PEM format if the creation policy allows it, giving users the flexibility to manage the private key outside of Azure Key Vault.
- **Non-Exportable Keys**: The private key cannot be exported and remains securely stored within Azure Key Vault. This increases security but may limit flexibility in some scenarios.

Supported key types and their security standards:

| Key Type    | Description                           | Security Standard          |
|-------------|---------------------------------------|----------------------------|
| RSA         | Commonly used software-protected RSA key | FIPS 140-2 Level 1         |
| RSA-HSM     | RSA key protected by a Hardware Security Module (Premium SKU only) | FIPS 140-2 Level 2 HSM     |
| EC          | Software-protected Elliptic Curve key | FIPS 140-2 Level 1         |
| EC-HSM      | Elliptic Curve key protected by HSM (Premium SKU only) | FIPS 140-2 Level 2 HSM     |
| octet       | Software-protected symmetric key      | FIPS 140-2 Level 1         |

## Certificate Attributes and Tags

- **Attributes**: Include settings like `enabled`, which specifies whether the certificate data can be retrieved or used for operations. Additional read-only attributes like `created` and `updated` provide timestamps for tracking.
- **Tags**: Customizable key/value pairs that can be used for organizing and identifying certificates within Azure Key Vault.

## Certificate Policy Details

- **X.509 Certificate Properties**: Specifies details such as the subject name, alternative names, and other X.509 attributes.
- **Key Properties**: Defines the type of key (RSA, RSA-HSM, EC, etc.), key size, whether the key is exportable, and if the key should be reused upon renewal.
- **Secret Properties**: Determines the content type of the secret that is created alongside the certificate.
- **Lifetime Actions**: Defines actions such as `emailContacts` or `autoRenew` that should be taken when certain conditions are met, like a specific number of days before expiration.

## Mapping X.509 Key Usage to Key Vault Operations

| X.509 Key Usage Flags | Key Vault Key Operations | Default Behavior in Key Vault |
|-----------------------|--------------------------|-------------------------------|
| DataEncipherment      | encrypt, decrypt         | Not applicable                |
| DigitalSignature      | sign, verify             | Used for signing/verification |
| KeyEncipherment       | wrapKey, unwrapKey       | Used for key wrapping/unwrapping |

## Certificate Issuer Integration

- **Supported Providers**: Azure Key Vault integrates with DigiCert and GlobalSign, which are available in all public cloud and Azure Government locations.
- **Configuration**: Before using a certificate issuer, an organization must set up an account with the CA and configure Key Vault with the necessary credentials to request and manage certificates.

## Certificate Contacts and Access Control

- **Contacts**: Shared contact information for receiving notifications about all certificates within a Key Vault.
- **Access Control**: Azure Key Vault allows for fine-grained access control policies specific to certificates, separate from keys and secrets.

## Real-World Use Cases and Examples

- **Secure Communication**: Use TLS certificates from Azure Key Vault to encrypt communications for web applications and services.
- **Authentication**: Leverage certificates to authenticate devices and services, ensuring that only trusted entities can access resources.
- **Code Signing**: Secure your code by signing it with a certificate, ensuring the integrity and origin of the code when distributed.

### Practical Example: Securing an Azure Web Application

- **Scenario**: You want to secure an Azure-hosted web application with a TLS certificate.
- **Implementation Steps**:
  1. Create or import a certificate into Azure Key Vault.
  2. Configure a policy for automatic renewal with a supported CA.
  3. Link the web application to the TLS certificate stored in Key  Vault.
  4. Set up contact information for renewal and expiration notifications.
  5. Define access policies in Key Vault to control which users or applications can manage or access the certificate.