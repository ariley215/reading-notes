# Other baseline security considerations

## More security recommendations

### Set an expiration date on all keys in Azure Key Vault - Level 1

These attributes might be specfied for a key in Azure Key Vault.

In a JSON request, an attribute's keyword and braces { } are required, even if no attribute is specified.

We recommend that you rotate your keys in your key vault and set an explicit expiry time for each key

- Key Vault stores and manages secrets as sequences of 8-bit bytes called *octets*, with a maximum size of 25 KB each for each key. 

1. Sign in to the Azure portal. Search for and select Key vaults.
2. In the left menu under Objects, select Keys.
3. In the Keys pane for the key vault, ensure that each key in the vault has an Expiration date set as appropriate.
4. If you change any settings, in the menu bar, select Save.

### Set an expiration date on all secrets in Azure Key Vault - Level 1

- Sign in to the Azure portal. Search for and select Key vaults.
- In the left menu under Objects, select Secrets.
- In the Secrets pane for the key vault, ensure that each secret in the vault has Expiration date set as appropriate.

### Set resource locks for mission-critical Azure resources - Level 2

To lock a subscription, resource group, or resource to prevent other users from accidentally deleting or modifying a critical resource

- Lock levels are Read-only and Delete.
- Unlike role-based access control, you use management locks to apply a restriction to all users and roles.
- Locks apply only to operations that happen in the management plane.
- Resource changes are restricted, but resource operations aren't restricted.