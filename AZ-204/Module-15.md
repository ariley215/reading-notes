# Explore the Microsoft Identity Platform   

Objectives:

- Identify the components of the Microsoft identity platform
- Describe the three types of service principals and how they relate to application objects
- Explain how permissions and user consent operate, and how conditional access impacts your application

# Explore the Microsoft Identity Platform

## Overview

- The Microsoft identity platform helps you build applications that allow users and customers to sign in using their Microsoft identities or social accounts.
- It also provides authorized access to your own APIs or Microsoft APIs like Microsoft Graph.

## Key Components

1. **Authentication Service**:
   - OAuth 2.0 and OpenID Connect standard-compliant authentication service
   - Supports various identity types:
     - Work or school accounts (provisioned through Microsoft Entra ID)
     - Personal Microsoft accounts (like Skype, Xbox, and Outlook.com)
     - Social or local accounts (using Azure Active Directory B2C)
     - Social or local customer accounts (using Microsoft Entra External ID)

2. **Open-source Libraries**:
   - Microsoft Authentication Libraries (MSAL)
   - Support for other standards-compliant libraries

3. **Microsoft Identity Platform Endpoint**:
   - Works with MSAL or any other standards-compliant library
   - Implements human-readable scopes, in accordance with industry standards

4. **Application Management Portal**:
   - Registration and configuration experience in the Azure portal
   - Provides other Azure management capabilities

5. **Application Configuration API and PowerShell**:
   - Programmatic configuration of applications through the Microsoft Graph API and PowerShell
   - Enables automation of DevOps tasks

## Benefits for Developers

- Integration of modern innovations in identity and security, such as:
  - Passwordless authentication
  - Step-up authentication
  - Conditional Access
- Developers don't need to implement such functionality themselves, as applications integrated with the Microsoft identity platform natively take advantage of these features.

# Discover Permissions and Consent

## OAuth 2.0 and the Microsoft Identity Platform

- The Microsoft identity platform implements the OAuth 2.0 authorization protocol.
- OAuth 2.0 allows a third-party app to access web-hosted resources on behalf of a user.
- Any web-hosted resource that integrates with the Microsoft identity platform has a resource identifier, or application ID URI.

## Permissions and Scopes

- Permissions, also known as scopes, are used to divide the functionality of a resource into smaller chunks.
- Apps can request only the permissions they need to perform their function.
- Permissions are represented as string values, such as `https://graph.microsoft.com/Calendars.Read`.
- Apps request permissions by specifying the scopes in the `scope` query parameter.

## Permission Types

- The Microsoft identity platform supports two types of permissions:
  1. **Delegated permissions**: Used by apps with a signed-in user present. The user or an administrator consents to the permissions.
  2. **App-only access permissions**: Used by apps running without a signed-in user, such as background services. Only an administrator can consent to these permissions.

## Consent Types

1. **Static user consent**:
   - All permissions are specified in the app's configuration in the Azure portal.
   - The user or administrator must consent to these permissions when the app is first used.

2. **Incremental and dynamic user consent**:
   - Apps can request permissions incrementally, without needing to pre-define all permissions in the app registration.
   - The user is prompted to consent to new permissions as they are requested.
   - This approach is convenient but can be challenging for permissions that require admin consent.

3. **Admin consent**:
   - Certain high-privilege permissions require admin consent to ensure administrators have control over authorizing access to sensitive data.
   - Administrators can consent on behalf of the entire organization.

## Requesting Individual User Consent

- Apps can request permissions in the `scope` parameter of the authorization request.
- If the user has not consented to the requested permissions, the Microsoft identity platform will prompt the user to grant consent.

# Discover Conditional Access

## What is Conditional Access?

- Conditional Access is a feature in Microsoft Entra ID (Azure Active Directory) that allows developers and enterprises to secure their apps and services in various ways.
- It enables organizations to apply policies that control access to resources based on conditions such as:
  - Multifactor authentication
  - Requiring Intune-enrolled devices
  - Restricting user locations and IP ranges

## Impact on Apps

- In most cases, Conditional Access policies do not require any changes to an app's behavior or code.
- However, there are certain scenarios where apps need to handle Conditional Access challenges:
  - Apps performing the on-behalf-of flow
  - Apps accessing multiple services/resources
  - Single-page apps using MSAL.js
  - Web apps calling a resource

## Handling Conditional Access Challenges

- When a Conditional Access policy is applied to an app or the web API it accesses, the app may receive a "claims challenge" from the Microsoft identity platform.
- To continue functioning, the app needs to implement logic to handle these challenges, which may involve performing an interactive sign-in request.

## Conditional Access Examples

1. **Single-tenant iOS app**:
   - A Conditional Access policy is applied to the app.
   - When the user signs in, the policy is automatically invoked, and the user is required to perform multifactor authentication.
   - No code changes are required in the app.

2. **App with a middle-tier service**:
   - A Conditional Access policy is applied to the downstream API accessed by the middle-tier service.
   - When the end-user signs in, the app requests access to the middle-tier, which then performs an on-behalf-of flow to access the downstream API.
   - At this point, the middle-tier receives a claims challenge, which it needs to send back to the app. The app then needs to handle the challenge, potentially by performing an interactive sign-in request.

In summary, Conditional Access is a powerful feature that allows enterprises to secure their apps and services, but in certain scenarios, apps may need to implement additional logic to handle Conditional Access challenges.
