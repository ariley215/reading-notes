# Explore the Microsoft 365 Permission Models

## Microsoft 365 Admin Center
- **Purpose**: Centralized management of permissions for Microsoft 365 services.
- **Key Features**:
  - **User and Group Management**: Create/manage user accounts; assign roles and permissions.
  - **Azure RBAC**: Implement predefined roles; assign to users for controlled access.
  - **Service-specific Permissions**: Manage access to Exchange Online, SharePoint Online, etc.
  - **Application Permissions**: Control third-party app access to Microsoft 365 data.

## Microsoft Defender Portal
- **Purpose**: Centralized security management for threat protection and incident response.
- **Key Features**:
  - **Threat Investigation**: Analyze security threats; respond to alerts.
  - **Incident Management**: Track and manage security incidents collaboratively.
  - **Advanced Hunting**: Use Kusto Query Language for proactive threat hunting.
  - **Threat Analytics**: Gain security insights; improve organizational security posture.
  - **Microsoft 365 Integration**: Connect with Defender for Endpoint, Office 365, etc.

## Microsoft Purview Compliance Portal
- **Purpose**: Assist organizations in meeting regulatory and compliance requirements.
- **Key Features**:
  - **Compliance Management**: Define compliance policies; monitor activities.
  - **Data Protection**: Implement DLP policies; manage sensitivity labels.
  - **Risk Assessment**: Utilize tools for compliance posture assessments.
  - **E-Discovery and Legal Hold**: Manage electronically stored information for legal compliance(legal request and litigation requirements).
  - **Compliance Reporting**: Generate and customize compliance reports.

## Administrator Roles
- **Context**: Azure AD is now Microsoft Entra ID; roles manage Microsoft 365 products.
- **Azure RBAC**: Simplify permission management via role assignments.
- **Admin Roles**: Predefined roles for administrative tasks; assign responsibly.
- **Role Management**: Manage role groups in Defender portal; requires specific permissions.

## Members, Roles, and Role Groups
- **Roles**: Define permissions for tasks in Microsoft 365 admin centers.
- **Role Groups**: Collections of roles that empower users to perform their job functions.
- **Usage**: Assign users to default role groups for task-specific permissions.
- **Best Practice**: Add users individually to role groups for Microsoft Defender and Purview compliance portals.

## Types of Roles and Role Groups in Microsoft 365
- **Microsoft Entra Roles**: Central roles for permissions across Microsoft 365 services.
- **Email and Collaboration Roles**: Specific to Defender portal and Purview compliance portal.
- **Cloud Apps Roles**: Manage permissions for cloud app content and actions.
- **Microsoft Purview Solutions**: Manage role groups within the Purview compliance portal.

# Microsoft 365 Admin Roles Study Guide

## Overview
Microsoft 365 subscriptions include a set of admin roles for assigning to users in your organization through the Microsoft 365 admin center. Each admin role corresponds to common business functions and grants permissions for specific tasks in admin centers.

### Note
- Manage Microsoft Entra and Microsoft Intune roles from the Microsoft 365 admin center.
- Microsoft Entra admin center and the Intune admin center provide a broader set of roles.

## Security Guidelines for Assigning Roles

### Recommendations and Importance
- **Limit Global Administrators**: Have 2-4 Global admins to minimize risk and ensure account recovery options.
- **Assign Least Permissive Role**: Give admins only the necessary access for their tasks.
- **Require MFA for Administrators**: Use Multi-Factor Authentication to protect sensitive data access.

## Common Microsoft 365 Admin Center Roles

### Role Assignment and Permissions
- Navigate to **Role assignments** in the admin center to manage roles.
- Use the **Permissions tab** to view tasks available to each role.
- **Assigned** or **Assigned admins tab**: Add users to roles.

### Commonly Assigned Roles

| Admin role (alphabetical order) | Who should be assigned this role? |
| -------------------------------- | --------------------------------- |
| Billing administrator            | Users who manage purchases, subscriptions, service requests, and service health. |
| Compliance administrator         | Users responsible for regulatory compliance, eDiscovery cases, and data governance. |
| Exchange administrator           | Users managing email mailboxes, Microsoft 365 groups, and Exchange Online. |
| Global administrator             | Users who need global access to most management features and data across Microsoft services. |
| Global reader                    | Users who need to view (but not edit) admin features and settings. |
| Groups administrator             | Users managing group settings across admin centers. |
| Helpdesk administrator           | Users who reset passwords, manage service requests, and monitor service health. |
| License administrator            | Users who assign and remove licenses, and manage usage locations. |
| Office Apps administrator        | Users managing Office cloud policies and service health. |
| Password administrator           | Users who reset passwords for non-administrators and Password Administrators. |
| Message center reader            | Users who monitor message center notifications and share posts. |
| Power Platform administrator     | Users managing admin features for Power Apps, Power Automate, and DLP policies. |
| Reports reader                   | Users who view usage data and activity reports. |
| Security administrator           | Admins controlling overall security, managing security policies and incidents. |
| Service Support administrator    | Admins or users who open and manage service requests. |
| SharePoint administrator         | Users who access and manage the SharePoint Online admin center. |
| Teams administrator              | Users who access and manage the Teams admin center. |
| User administrator               | Users who manage user properties, licenses, and passwords. |

### Tip
- Use **Show all by Category** at the bottom of the role list for a categorized view of roles.

# Configure Outbound Spam Filtering Policies

## Overview

Outbound spam filtering is critical for maintaining the integrity and reputation of an organization's email system. Exchange Online Protection (EOP) checks outbound emails for spam and unusual activity. Outbound spam may indicate a compromised account.

## Outbound Spam Policies

EOP uses outbound spam policies to help prevent spam. Admins can view, edit, and configure the default policy and create custom policies for specific users, groups, or domains.

### Creating Outbound Spam Policies

Outbound spam policies can be configured in the Microsoft Defender portal or using PowerShell.

#### Microsoft Defender Portal

- **URL**: [Microsoft Defender Portal](https://security.microsoft.com)
- **Navigation**: Email & Collaboration > Policies & Rules > Threat policies > Anti-spam

#### Exchange Online PowerShell

- For organizations with Exchange Online mailboxes.
- Use the `*-HostedOutboundSpamFilterPolicy` and `*-HostedOutboundSpamFilterRule` cmdlets.

#### Standalone EOP PowerShell

- For organizations without Exchange Online mailboxes.
- Use the same cmdlets as Exchange Online PowerShell.

### Elements of an Outbound Spam Policy

- **Outbound Spam Filter Policy**: Specifies actions and notifications.
- **Outbound Spam Filter Rule**: Specifies priority and sender filters.

### Default Policy

- The default policy named "Default" applies to all senders and cannot be deleted.

## Creating Custom Outbound Spam Policies in the Microsoft Defender Portal

1. Navigate to Anti-spam policies.
2. Select "Create policy" and choose "Outbound".
3. Provide a unique name and description for the policy.
4. Specify internal senders the policy applies to (users, groups, domains).
5. Configure message limits and actions for exceeding limits.
6. Control automatic email forwarding to external senders.
7. Set up notifications for suspicious outbound email messages.
8. Review settings and select "Create".

## Configure Outbound Spam Policies in Exchange PowerShell

### Step 1: Create an Outbound Spam Filter Policy

```powershell
New-HostedOutboundSpamFilterPolicy -Name "PolicyName" -AdminDisplayName "Comments" <Additional Settings>
New-HostedOutboundSpamFilterPolicy -Name "Contoso Executives" -RecipientLimitExternalPerHour 400 -RecipientLimitInternalPerHour 800 -RecipientLimitPerDay 800 -ActionWhenThresholdReached BlockUser
```

### Step 2: Create an Outbound Spam Filter Rule

```powershell
New-HostedOutboundSpamFilterRule -Name "RuleName" -HostedOutboundSpamFilterPolicy "PolicyName" <Sender filters> [<Sender filter exceptions>] [-Comments "OptionalComments"]
```

**Example**
```powershell
New-HostedOutboundSpamFilterRule -Name "Contoso Executives" -HostedOutboundSpamFilterPolicy "Contoso Executives"
```

**Best Practices**
- Regularly review and update outbound spam policies.
- Implement stricter settings for sensitive roles or groups within the organization.
- Monitor the effectiveness of policies and adjust as necessary.
- Train users on best practices to prevent account compromise.