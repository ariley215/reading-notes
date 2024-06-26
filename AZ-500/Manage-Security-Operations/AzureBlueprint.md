# Configure security settings by using Azure Blueprint

## Understanding Azure Blueprints

- **Definition**: Azure Blueprints is a service that enables users to define a repeatable set of Azure resources that adhere to an organization's standards, patterns, and requirements.

- **Components**:
  - Role Assignments
  - Policy Assignments
  - Azure Resource Manager (ARM) templates
  - Resource Groups

- **Backed by**: Azure Cosmos DB for low latency, high availability, and consistent access across regions.

## Azure Blueprints vs. ARM Templates

- **ARM Templates**: Used for deploying Azure resources, no active connection to the template post-deployment.

- **Blueprints**: Preserves the relationship between blueprint definition and assignment, supporting tracking and auditing.

- **Integration**: Blueprints can include ARM template artifacts, allowing reuse of existing ARM templates.

## Azure Blueprints vs. Azure Policy

- **Blueprints**: Packages for setting up specific sets of standards and requirements, maintaining consistency and compliance.

- **Policy**: Focuses on resource properties, ensuring resources adhere to organizational standards.

## Blueprint Artifacts

| Resource                       | Hierarchy Options | Description |
| ------------------------------ | ----------------- | ----------- |
| Resource Groups                | Subscription      | Organizes resources and provides scope for policy/role assignments and ARM templates. |
| ARM Template                   | Subscription, Resource Group | Composes complex environments, supports nested and linked templates. |
| Policy Assignment              | Subscription, Resource Group | Assigns a policy or initiative, supports parameters. |
| Role Assignment                | Subscription, Resource Group | Assigns built-in roles to users/groups for appropriate access levels. |

## Blueprint Definition Locations

- Blueprints can be saved to a management group or subscription with Contributor access.

## Blueprint Parameters

- Parameters can be passed to policies/initiatives or ARM templates, with defined values or allowing assignment-time decision.

## Blueprint Publishing

- **Draft Mode**: Initial state of a blueprint before it's ready for assignment.

- **Publishing**: Requires a version string and optional change notes, allowing version tracking and assignment.

## Blueprint Assignment

- Published versions can be assigned to a management group or subscription, with parameters defined during the assignment process.

## Permissions in Azure Blueprints

- **Reading/Viewing**: Requires read access to the scope where the blueprint is located.

- **Creating**: Requires `Microsoft.Blueprint/blueprints/write` and `Microsoft.Blueprint/blueprints/artifacts/write`.

- **Publishing**: Requires `Microsoft.Blueprint/blueprints/versions/write`.

- **Deleting**: Requires `Microsoft.Blueprint/blueprints/delete` and related artifact and version delete permissions.

- **Assigning/Unassigning**: Requires `Microsoft.Blueprint/blueprintAssignments/write` and `Microsoft.Blueprint/blueprintAssignments/delete`.

## Built-in Roles

- **Owner**: Full Azure Blueprints permissions.
- **Contributor**: Create and delete blueprint definitions, no assignment permissions.
- **Blueprint Contributor**: Manage blueprints without assignment capabilities.
- **Blueprint Operator**: Assign published blueprints, cannot create new definitions.

## Custom Roles

- Consider creating custom roles if built-in roles do not meet specific security requirements.

## Managed Identities

- **System-assigned**: Requires Owner role on the assigned subscription for deployment.
- **User-assigned**: Only requires `Microsoft.Blueprint/blueprintAssignments/write` permission for the user creating the assignment.

## Additional Resources

- [Azure Blueprints Documentation](https://docs.microsoft.com/en-us/azure/governance/blueprints/)
- [Understanding Azure Policy](https://docs.microsoft.com/en-us/azure/governance/policy/)
- [Azure Resource Manager Templates](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/)