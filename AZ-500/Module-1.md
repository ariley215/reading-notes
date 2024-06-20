# Create Security Baselines

![Secure Azure Workload with CIS](https://learn.microsoft.com/en-us/training/modules/create-security-baselines/media/cis-benchmark.png)

**CIS benchmark** - a document that details CIS best practices, for Azure security services and tools to facilitate security and compliance for customer applications running on Azure services.

- [CIS Azure Security Benchmark](https://www.cisecurity.org/benchmark/azure/)
  - This guide was tested against the listed Azure services as of February 2021.
  - The scope of this benchmark is to establish the foundational level of security for anyone who adopts Azure.

CIS have 2 implementation levels:

- Level 1 - Recommended minimum security settings
  - These settings should be configured on all systems.
  - These settings should cause little or no interruption of services or reduced functionality.
- Level 2 - Recommendations for highly secure environments
  - These settings might result in reduced functionality.

| Technology Group                   | Description                                                                 | # of Recommendations |
|------------------------------------|-----------------------------------------------------------------------------|----------------------|
| Identity & Access Management (IAM) | Recommendations related to IAM policies                                     | 23                   |
| Microsoft Defender for Cloud       | Recommendations related to the configuration and use of Microsoft Defender for Cloud | 19                   |
| Storage accounts                   | Recommendations for setting storage account policies                        | 7                    |
| Azure SQL Database                 | Recommendations for helping to secure Azure SQL databases                   | 8                    |
| Logging and monitoring             | Recommendations for setting logging and monitoring policies for your Azure subscriptions | 13                   |
| Networking                         | Recommendations for helping to securely configure Azure networking settings and policies | 5                    |
| VMs                                | Recommendations for setting security policies for Azure compute services, and specifically VMs | 6                    |
| Other                              | Recommendations regarding general security and operational controls, including recommendations related to Azure Key Vault and resource locks | 3                    |
| **Total recommended**              |                                                                             | **84**               |

## Create an Identity & Access Management baseline

IAM recommendations that are in CIS Microsoft Azure Foundations Security Benchmark v. 1.3.0. Included with each recommendation are the basic steps to complete in the Azure portal

*Level 2 options might restrict some features or activities, so carefully consider which security options you decide to enforce.*

### Restrict access to the Microsoft Entra administration portal - Level 1

1. Sign in to the Azure portal. Search for and select Microsoft Entra ID.
2. In the left menu under Manage, select Users.
3. In the left menu, select User settings.
4. In User settings, under Administration portal, ensure that Restrict access to Microsoft Entra administration portal is set to Yes. 
    1. Setting this value to Yes prevents all non-administrators from accessing any data in the Microsoft Entra administration portal. 
    1. The setting doesn't restrict access to using PowerShell or another client, such as Visual Studio.
5. If you change any settings, in the menu bar, select Save.

### Enable multifactor authentication for Microsoft Entra users

1. Sign in to the Azure portal. Search for and select Microsoft Entra ID.
1. In the left menu under Manage, select Users.
1. In the All users menu bar, select Per-user MFA.
1. In the multifactor authentication window, ensure that multifactor authentication Status is set to Enabled for all users. To enable multifactor authentication, select a user. Under quick steps, select Enable > enable multifactor authentication.

Users can bypass subsequent verifications for a specified number of days after they've successfully signed in to a device by using MFA