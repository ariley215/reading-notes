# Configure access to Key Vault, including vault access policies and Azure Role Based Access Control

# AZ-500 Study Guide: Configuring Access to Azure Key Vault

## Understanding Azure Role-Based Access Control (RBAC)

Azure RBAC is an authorization system that provides fine-grained access management of Azure resources, including Azure Key Vault.

### Key Concepts

- **Scope Levels**: Permissions can be set at the management group, subscription, resource group, or individual resource levels.
- **Data Plane Operations**: Azure RBAC allows for managing permissions for Keys, Secrets, and Certificates within a Key Vault.
- **Separate Permissions**: Azure RBAC enables separate permissions on individual keys, secrets, and certificates.

## Best Practices for Access Control

- **Application Environment Segregation**: Use a separate vault for each application environment (Development, Pre-Production, Production).
- **Specific Scenarios**: Individual keys, secrets, and certificates permissions should be used for scenarios where sharing is required between applications.

## Azure Built-in Roles for Key Vault

| Role Name | Description | Role ID |
| --- | --- | --- |
| Key Vault Administrator | Perform all data plane operations on a Key Vault. | `00482a5a-887f-4fb3-b363-3b7fe8e74483` |
| Key Vault Certificates Officer | Manage Key Vault certificates. | `a4417e6f-fecd-4de8-b567-7b0420556985` |
| Key Vault Crypto Officer | Manage Key Vault keys. | `14b46e9e-c2b7-41b4-b07b-48a6ebf60603` |
| Key Vault Crypto Service Encryption User | Perform wrap/unwrap operations using keys. | `e147488a-f6f5-4113-8e2d-b22465e65bf6` |
| Key Vault Crypto User | Perform cryptographic operations using keys. | `12338af0-0e69-4776-bea7-57ae8d297424` |
| Key Vault Reader | Read metadata of Key Vault and its objects. | `21090545-7ca7-4776-b22c-e363652d74d2` |
| Key Vault Secrets Officer | Manage Key Vault secrets. | `b86a8fe4-44ce-4948-aee5-eccb2c155cd7` |
| Key Vault Secrets User | Read secret contents and certificates with private key. | `4633458b-17de-408a-b874-0445c86b69e6` |

- **Note**: The Key Vault Contributor role is for management plane operations only and does not grant access to keys, secrets, or certificates.

## Managing Key Vault Access

### Prerequisites

- An Azure subscription.
- Necessary permissions to add role assignments (e.g., Key Vault Data Access Administrator, User Access Administrator, or Owner).

### Steps to Add Role Assignments

1. **Navigate to Key Vault**: Go to the Azure portal and select the Key Vault you want to manage.

2. **Access Control (IAM)**: Click on "Access Control (IAM)" on the Key Vault resource menu.

3. **Add Role Assignment**: Click on "Add" and then "Add role assignment".

4. **Select Role**: Choose the appropriate role based on the responsibilities required.

5. **Select Assignee**: Search for the user, group, or service principal to assign the role to.

6. **Review and Assign**: Review the assignment and click "Save" to apply.

### Example: Granting Key Vault Secrets User Role

```shell
az role assignment create \
  --role "Key Vault Secrets User" \
  --assignee "<principal-id>" \
  --scope "/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.KeyVault/vaults/<key-vault-name>"
```

## Notes

- **New Azure RBAC Permission Model**: An alternative to the traditional vault access policy permissions model.
- **Advantages of Azure RBAC**:
  - Provides more granular control over permissions.
  - Enables the management of permissions across all Key Vaults in a unified manner.
- **Principle of Least Privilege**: 
  - Always adhere to this principle when assigning roles to users or applications.
