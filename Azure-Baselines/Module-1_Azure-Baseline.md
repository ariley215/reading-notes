# Create an Azure VM baseline

Use to create, assign, and manage policies.

Evaluates your resources for noncompliance with assigned policies.

- For new and existing resources

## Azure VM security recommendations

### Enable and install a VM agent for Microsoft Defender for Cloud data collection - Level 1

Defender for Cloud lets you know when a VM requires a VM agent.

- Installed by default

1. Sign in to the Azure portal. Search for and select Microsoft Defender for Cloud.
2. In the left menu under Management, select Environment settings.
3. In the Environment settings pane, select your subscription.
4. In the left menu under Settings, select Auto provisioning.
5. For the VM agent you want to use, select On. Select a workspace to use.
6. If you change any settings, in the menu bar, select Save.

### Ensure that OS disk are encrypted - Level 1

- Uses the BitLocker feature of Windows and the DM-Crypt feature of Linux to provide volume encryption for the OS and data disks of Azure VMs.
- Integrates with Azure Key Vault to help you control and manage disk encryption keys and secrets.
- Ensures that all data on the VM disks is encrypted at rest when it is in Azure storage.

1. Sign in to the Azure portal. Search for and select Virtual machines.
2. In the left menu under Settings, select Disks.
3. Under OS disk, ensure that the OS disk has an encryption type set.
4. Under Data disks, ensure that each disk has an encryption type set.
5. If you change any settings, in the menu bar, select Save.

### Ensure that only approved VM extensions are installed - Level 1

Small applications that provide post-deployment configuration and automation tasks on Azure VM.

- Requires software installation or antivirus protection or needs to run a script, you can use a VM extension
- Can bundle extensions with a new VM deployment or run them against any existing system

1. Sign in to the Azure portal. Search for and select Virtual machines.
2. In the left menu under Settings, select Extensions + applications.
3. In the Extensions + applications pane, ensure that the extensions that are listed are approved for use.

### Ensure that the OS patches for the VMs are applied - Level 1

Monitors Windows and Linux VMs and computers for missing operating system updates.

- Updates you receive depend on which service you configure on the Windows computer.

1. Sign in to the Azure portal. Search for and select Microsoft Defender for Cloud.
2. In the left menu under General, select Recommendations.
3. In Recommendations, ensure that there are no recommendations for Apply system updates.

### Ensure that VMs have an endpoint protection solution installed and running - Level 1

Microsoft Defender for Cloud monitors the status of antimalware protection. 

- In Endpoint protection issues pane.

Use the same process as described in the preceding recommendation.