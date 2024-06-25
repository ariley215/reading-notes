# Azure Security Policies and Initiatives Study Guide

## Azure Security Policies

### Definition
- **Azure Policy**: A service that enforces rules across Azure resources to ensure compliance with standards and SLAs.

### Components
- **Policy Definition**: Specifies conditions and rules to be enforced.
- **Policy Assignment**: Applies the policy to a specific scope (resources, groups, etc.).
- **Policy Parameters**: Allows customization of policies for specific needs.

### Use Cases
- Enforce consistent rules across resources.
- Ensure uniform tagging for resources.
- Control which resource types can be provisioned.

## Azure Security Initiatives

### Definition
- **Azure Initiatives**: Collections of related Azure Policy definitions aimed at achieving specific compliance goals.

### Components
- **Definitions (Policies)**: Group of policies packaged together.
- **Assignment**: Initiatives are assigned to a scope for enforcement.
- **Parameters**: Customization options for initiative behavior.

### Use Cases
- Attain compliance with standards like PCI-DSS and HIPAA.
- Manage multiple policies as a single unit.

## When to Use Each Tool
- **Azure Policy**: For enforcing specific, individual rules.
- **Azure Initiatives**: For managing multiple policies, even if it's just one policy for simplified management.

## Example
Use an initiative to manage 20 separate policies for PCI-DSS compliance as one unit.

## Creating and Managing Policies for Compliance

### Prerequisites
- An Azure subscription (create a free account if necessary).

### Assign a Policy

1. Go to the Azure portal, to assign policies. Search for and select Policy.
2. Select Assignments on the left side of the Azure Policy page. An assignment is a policy that has been assigned to take place within a specific scope.
3. Select Assign Policy from the top of the Policy - Assignments page.
4. On the Assign Policy page and Basics tab, select the Scope by selecting the ellipsis and selecting either a management group or subscription. Optionally, select a resource group.*A scope determines what resources or grouping of resources the policy assignment gets enforced on.* Then click Select at the bottom of the Scope page. This example uses the Contoso subscription. Your subscription will differ.
5. Resources can be excluded based on the Scope. Exclusions start at one level lower than the level of the Scope. *Exclusions are optional*, so leave it blank for now.
6. Select the Policy definition ellipsis to open the list of available definitions. You can filter the policy definition Type to Built in to view all and read their descriptions.
7. Select Inherit a tag from the resource group if missing. If you can't find it right away, type inherit a tag into the search box and then press ENTER or select out of the search box. Click Select at the bottom of the AvailableDefinitions page once you have found and selected the policy definition.
8. The Assignment name is automatically populated with the policy name you selected, but you can change it. For this example, leave Inherit a tag from the resource group if missing. You can also add an optional Description. The description provides details about this policy assignment.
9. Leave Policy enforcement as Enabled. *When Disabled, this setting allows testing the outcome of the policy without triggering the effect.*
10. *Assigned by is automatically filled based on who is logged in. This field is optional, so custom values can be entered.*
11. Select the Parameters tab at the top of the wizard.
12. For Tag Name, enter Environment.
13. Select the Remediation tab at the top of the wizard.
14. Leave Create a remediation task unchecked. *This box allows you to create a task to alter existing resources in addition to new or updated resources.*
15. Create a Managed Identity is automatically checked since this policy definition uses the modify effect. Permissions is set to Contributor automatically based on the policy definition. For more information, see managed identities and how remediation access control works.
16. Select the Non-compliance messages tab at the top of the wizard.
17. Set the Non-compliance message to This resource doesn't have the required tag.*This custom message is displayed when a resource is denied or for non-compliant resources during regular evaluation.*
18. Select the Review + create tab at the top of the wizard.
19. Review your selections, then select Create at the bottom of the page.

[Link to Azure Policy documentation](https://docs.microsoft.com/en-us/azure/governance/policy/)

### Example: Assigning a Tagging Policy

```bash
az policy definition list
az policy assignment create --name 'assign-tag-policy' --scope '/subscriptions/{subscription-id}' --policy '{policy-definition-id}'
az policy assignment create --name 'assign-tag-policy' --scope '/subscriptions/{subscription-id}' --policy '{policy-definition-id}' --params '{"tagName": {"value": "Environment"}}'
az policy assignment list --query "[].{name:name, complianceState:complianceState}"
az policy remediation create --name 'remediation-task' --policy-assignment 'assign-tag-policy' --scope '/subscriptions/{subscription-id}'
```

## Real-World Application

- Enforce "Cost Center" tags on all resources for budget tracking.
- Apply "Environment" tags (Prod, Dev, Test) to differentiate resource stages.
