# Create Microsoft Defender for Cloud baseline

These recommendations will set various security policies in Azure.

- The policies define the set of controls that are recommended for tour resources in Azure.

## Recommendations

### Enable enhanced security features - Level 2

2 modes

- Without enhanced security features(free)
- With enhanced security features.
  - Provides unified security management and threat protection across hybrid cloud workloads.
  - Advanced threat protection
    - Built-in behavioral analytics and machine learning to identify attacks and zero-day exploits.
    - Access and application controls to reduce exposure to network attacks and malware.

Resources supported by threat potection:

- Azure Virtual Machines
- Virtual machine scale sets
- Azure App Service
- Azure SQL Server
- Azure Storage

1. Sign in to the Azure portal. Search for and select Microsoft Defender for Cloud.
1. In the left menu under General, select Getting started.
1. Select the Upgrade tab, and then select the subscription to upgrade. The Resources pane shows the resources that will be protected and the billing cost per resource.
1. Select the Upgrade button.

### View Microsoft Defender for Cloud built-in security policies

1. Sign in to the Azure portal. Search for and select Microsoft Defender for Cloud.
1. In the left menu under Management, select Environment settings.
1. Select the subscription to open the Policy settings pane.

### Enable automatic provision of the Log Analytics agent - Level 1

installs the Azure Log Analytics agent on all supported Azure VMs and on any new VMs create.

**- Automatic provisioning is strongly recommended.**

1. Sign in to the Azure portal. Search for and select Microsoft Defender for Cloud.
1. In the left menu under General, select Getting started.
1. Select the Install agents tab, and then select the subscription to install agents on.
1. Select the Install agents button.

### Enable System Updates - Level 1

1. Daily monitors Windows and Linux VMs and computers for missing operating system updates.
1. Defender for Cloud recommends that apply system updates.
1. Sign in to the Azure portal. Search for and select Microsoft Defender for Cloud.
1. In the left menu under Management, select Environment settings.
1. Select the subscription.
1. In the left menu, select Security policy.
1. Under Default initiative, select a subscription or management group.
1. Select the Parameters tab.
1. Ensure that the Only show parameters that need input or review box is cleared.
1. Ensure that System updates should be installed on machines is one of the policies listed.
    1. If the resource isn't deployed, NotExists appears.
    1. If System updates should be installed on machines is enabled, Audit appears. If deployed but disabled, Disabled appears.
1. If change any settings, select the Review + Save tab, and then select Save.

### Enable Security Configurations - Level 1

Monitors security configurations by applying a set of more than 150 recommended rules for hardening the OS.

- Related to firewalls, auditing, password policies,
- If theres a vulnerable configuration, Defender for Cloud generates a security recommendation.

1. Sign in to the Azure portal. Search for and select Microsoft Defender for Cloud.
2. In the left menu under Management, select Environment settings.
3. Select the subscription.
4. In the left menu, select Security policy.
5. Under Default initiative, select a subscription or management group.
6. Select the Parameters tab.
7. Ensure that Vulnerabilities in security configuration on the virtual machine scale sets should be remediated is one of the policies.
8. If you change any settings, select the Review + Save tab, and then select Save.

### Policies in the Parameters Tab

Enable endpoint protection - Level 1

- Recommended for all VMs.

Enable disk encryption - Level 1

- Recommends to use Azure Disk Encryption on Windows or Linux VM disks.
- Encrypt the Linux and Windows IaaS.

Enabe Network Security Groups - Level 1

- Recommends enableing a network security group
- Contain a list of Access Control List rules. 
  - Allow or deny network traffic to VM instances.
- Associated either with subnets or with individual VM instances within that subnet.
- Traffic to an individual VM can be restricted by associating directly with the VM

Enable a web application firewall - Level 1

- Might recommend to add a web application firewall.

Enable vulnerability assessment - Level 1 

- Part of the Defender for Cloud VM recommendations
- If a vulnerability assessment solution is missing, it is recommended one is installed.
- The partner's management platform provides vulnerability and health monitoring data back to Defender for Cloud.

Enable storage encryption - Level 1

- When storage encryption is enabled, any new data in Azure Blob Storage and Azure Files is encrypted.

Enable JIT network access

- Can be used to lock down inbound traffic to Azure VMs.
- Reduces exposure to attacks while providing easy access to connect to VMs.

Enable adaptive application control - Level 1

- Intelligent, automated end-to-end approved application listing solution.
- Helps control which applications can run on Azure and non-Azure VMs.
  - helps harden VMs against malware.
- Machine learning to analyze the applications

Enable SQL auditing and threat detection - Level 1

- Recommends that turn on auditing and threat detection for all databases on servers that run Azure SQL
  -Helps maintain regulatory compliance, understand database activity, and gain insight into discrepancies and anomalies 

Enable SQL encryption - Level 1

- TDE protects data and helps meet compliance requirements by encrypting database, associated backups, and transaction log files at rest.

### Set security contact email and phone number - Level 1

- Recommends providing security contact details.

1. Sign in to the Azure portal. Search for and select Cost Management + Billing. Depending on your subscriptions, you'll either see the Overview pane or the Billing scopes pane.
2. If you see the Overview pane, continue to the next step.
3. If you see the Billing scopes pane, select your subscription to go to the Overview pane.
4. In the Overview pane, in the left menu under Settings, select Properties.
5. Validate the contact information that appears. If you need to update the contact information, select the Update sold to link and enter the new information.

### Enable Send me emails about alerts - Level 1

1. Sign in to the Azure portal. Search for and select Microsoft Defender for Cloud.
2. In the left menu under Management, select Environment settings.
3. Select the subscription.
4. In the left menu under Settings, select Email notifications.
5. In the All users with the following roles dropdown, select your role or, in Additional email addresses (separated by commas), enter your email address.
6. Select the Notify about alerts with the following severity checkbox, select an alert severity, and then select Save.

### Enable Send email also to subscription owners - Level 1

1. In the Email notifications pane described in the preceding section, you can add more email addresses separated by commas.
1. If you change any settings, in the menu bar, select Save.


[Microsoft Learning](https://learn.microsoft.com/en-us/training/modules/create-security-baselines/3-create-an-azure-security-center-baseline)