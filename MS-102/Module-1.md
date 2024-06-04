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

|
 Admin role (alphabetical order) 
|
 Who should be assigned this role? 
|
|
---
|
---
|
|
 Billing administrator 
|
 Assign the Billing admin role to users who make purchases, manage subscriptions, service requests, and monitor service health. Billing admins also can: Manage all aspects of billing and create/manage support tickets in the Microsoft Entra admin center. 
|
|
 Compliance administrator 
|
 Assign the Compliance admin role to users who are responsible for helping your organization stay compliant with regulatory requirements, manage eDiscovery cases, maintain data governance policies, and manage compliance alerts. 
|
|
 Exchange administrator 
|
 Assign the Exchange admin role to users who need to view and manage your user's email mailboxes, Microsoft 365 groups, and Exchange Online. 
|
|
 Global administrator 
|
 Assign the Global administrator role to users who need global access to most management features and data across Microsoft online services. 
|
|
 Global reader 
|
 Assign the global reader role to users who need to view admin features and settings in admin centers that the global admin can view. The global reader admin can't edit any settings. 
|
|
 Groups administrator 
|
 Assign the groups admin role to users who need to manage all groups settings across admin centers. 
|
|
 Helpdesk administrator 
|
 Assign the Helpdesk admin role to users who must complete tasks such as resetting passwords and managing service requests. 
|
|
 License administrator 
|
 Assign the License admin role to users who need to assign and remove licenses from users and edit their usage location. 
|
|
 Office Apps administrator 
|
 Assign the Office Apps admin role to users who must manage Office cloud policies and monitor service health. 
|
|
 Password administrator 
|
 Assign the Password admin role to a user who needs to reset passwords for nonadministrators and Password Administrators. 
|
|
 Message center reader 
|
 Assign the Message center reader role to users who must monitor message center notifications and share posts. 
|
|
 Power Platform administrator 
|
 Assign the Power Platform admin role to users who must manage admin features for Power Apps, Power Automate, and DLP policies. 
|
|
 Reports reader 
|
 Assign the Reports reader role to users who must view usage data and activity reports. 
|
|
 Security administrator 
|
 Assign the Security admin role to admins who control your organization's overall security. 
|
|
 Service Support administrator 
|
 Assign the Service Support admin role as an extra role to admins or users who must open and manage service requests. 
|
|
 SharePoint administrator 
|
 Assign the SharePoint admin role to users who need to access and manage the SharePoint Online admin center. 
|
|
 Teams administrator 
|
 Assign the Teams administrator role to users who need to access and manage the Teams admin center. 
|
|
 User administrator 
|
 Assign the User admin role to users who must manage user properties, licenses, and passwords. 
|

### Tip
- Use **Show all by Category** at the bottom of the role list for a categorized view of roles.