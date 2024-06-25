# Create an Azure storage accounts baseline

## Azure Storage account security recommendations

The Azure Storage recommendations that are in CIS Microsoft Azure Foundations Security Benchmark

### Require security-enhanced transfers - Level 1

- Encrypts the data between the client and Azure Storage.
- first recommendation is to always use the HTTPS protocol.

1. Sign in to the Azure portal. Search for and select Storage accounts.
2. In the Storage accounts pane, select a storage account.
3. In the left menu under Settings, select Configuration.
4. In the Configuration pane, ensure that Secure transfer required is set to Enabled.
5. If you change any settings, in the menu bar, select Save.

### Enable binary large object (blob) encryption - Level 1

- Blob Storage is optimized to store massive amounts of unstructured data.
  - Data that doesn't adhere to a specific data model or definition
- Encrypts your data as it's written in its datacenters, and it automatically decrypts it for you as you access it.

1. Sign in to the Azure portal. Search for and select Storage accounts.
2. In the Storage accounts pane, select a storage account.
3. In the left menu under Security + networking, select Encryption.
4. In the Encryption pane, see that Azure Storage encryption is enabled for all new and existing storage accounts and that it can't be disabled.

### Periodically regenerate access keys - Level 1

- Azure generates two 512-bit storage access keys for every storage account.
- Rotating these keys periodically ensures that any inadvertent access to or exposure of these keys is limited by time.

1. Sign in to the Azure portal. Search for and select Storage accounts.
2. In the Storage accounts pane, select a storage account.
3. In the left menu, select Activity log.
4. In the Activity log, in the Timespan dropdown, select Custom. Select Start Time and End Time to create a range of 90 days or less.
5. Select Apply.

### Require shared access signature tokens to expire within an hour - Level 1

- Shared access signature is a URI that grants restricted access rights to Azure Storage resources.
- Grants them access to a resource for a specified period of time, with a specified set of permissions.

**Shared access signature token expiry times can't be automatically verified. The recommendation requires manual verification.**

### Require shared access signature tokens to be shared only via HTTPS - Level 1

1. Sign in to the Azure portal. Search for and select Storage accounts.
2. In the Storage accounts pane, select a storage account.
3. In the menu under Security + networking, select Shared access signature.
4. In the Shared access signature pane, under Start and expiry date/time, set the Start and End dates and times.
5. Under Allowed protocols, select HTTPS only.
6. If you change any settings, in the menu bar, select Generate SAS and connection string.

### Enable Azure Files encryption - Level 1

- Encrypts the OS and data disks in IaaS VMs.
- Client-side encryption and server-side encryption (SSE) are both used.

1. Sign in to the Azure portal. Search for and select Storage accounts.
2. In the Storage accounts pane, select a storage account.
3. In the left menu under Security + networking, select Encryption.
4. In the Encryption pane, see that Azure Storage encryption is enabled for all new and existing blob storage and file storage, and that it can't be disabled.

### Require only private access to blob containers - Level 1

- Grant read-only access to these resources without sharing your account key, and without requiring a shared access signature.
- To grant anonymous users read access to a container and its blobs, you can set the container access level to public.
- Security recommendation is to set access to storage containers to private.

1. Sign in to the Azure portal. Search for and select Storage accounts.
2. In the Storage accounts pane, select a storage account.
3. In the left menu under Data storage, select Containers.
4. In the Containers pane, ensure that Public access level is set to Private.
5. If you change any settings, in the menu bar, select Save.
