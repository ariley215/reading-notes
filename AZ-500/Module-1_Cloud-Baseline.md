# Create Microsoft Defender for Cloud baseline

These recommendations will set various security policies in Azure.

- The policies define the set of controls that are recommended for tour resources in Azure.

## Recommendations

### Enable enhanced security features - Level 2

2 modes

- Without enhanced security features(free)
- With enhanced security features.
  - Provides unified security management and threat protection across your hybrid cloud workloads.
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

installs the Azure Log Analytics agent on all supported Azure VMs and on any new VMs you create.

**- Automatic provisioning is strongly recommended.**

1. Sign in to the Azure portal. Search for and select Microsoft Defender for Cloud.
1. In the left menu under General, select Getting started.
1. Select the Install agents tab, and then select the subscription to install agents on.
1. Select the Install agents button.

### Enable System Updates - Level 1

1. Daily monitors Windows and Linux VMs and computers for missing operating system updates.
1. Defender for Cloud recommends that you apply system updates.
1. Sign in to the Azure portal. Search for and select Microsoft Defender for Cloud.
1. In the left menu under Management, select Environment settings.
1. Select the subscription.
1. In the left menu, select Security policy.
1. Under Default initiative, select a subscription or management group.
1. Select the Parameters tab.
1. Ensure that the Only show parameters that need input or review box is cleared.
1. Ensure that System updates should be installed on your machines is one of the policies listed.
    1. If the resource isn't deployed, NotExists appears.
    1. If System updates should be installed on your machines is enabled, Audit appears. If deployed but disabled, Disabled appears.
1. If you change any settings, select the Review + Save tab, and then select Save.

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
7. Ensure that Vulnerabilities in security configuration on your virtual machine scale sets should be remediated is one of the policies.
8. If you change any settings, select the Review + Save tab, and then select Save.

### Policies in the Parameters Tab

Enable endpoint protection - Level 1

- Recommended for all VMs.

Enable disk encryption - Level 1

- Recommends you use Azure Disk Encryption on Windows or Linux VM disks.
- Encrypt the Linux and Windows IaaS.