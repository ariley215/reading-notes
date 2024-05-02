# Implement Authentication by using the Microsoft Authentication Library

Objectives:

- Explain the benefits of using Microsoft Authentication Library and the application types and scenarios it supports.
- Instantiate both public and confidential client apps from code.
- Register an app with the Microsoft identity platform.
- Create an app that retrieves a token by using the MSAL.NET library.

Sure, here is a more concise version of the study guide on the Microsoft Authentication Library (MSAL):

# Microsoft Authentication Library (MSAL) Study Guide

## Key Benefits of MSAL

- Abstracts away low-level OAuth protocols
- Handles token acquisition and caching
- Allows specifying target audience (e.g., Microsoft Graph)
- Simplifies application setup from configuration
- Provides troubleshooting tools like exceptions, logging, telemetry

## Supported Application Types and Platforms

- Web apps, web APIs
- Single-page apps (JavaScript)
- Mobile and native apps
- Daemons and server-side apps
- Supports .NET, JavaScript, Java, Python, Android, iOS, and more

## Authentication Flows

- Authorization code: Secures tokens for native/web apps
- Client credentials: For service apps without user interaction
- On-behalf-of: App calls a service/API, which then calls Microsoft Graph
- Implicit: Used in browser-based apps
- Device code: Enables sign-in on a device using another device
- Integrated Windows: Domain-joined Windows PCs get tokens silently
- Interactive: Mobile/desktop apps call Microsoft Graph on behalf of user
- Username/password: App signs in user with credentials

## Application Types

1. **Public Client Applications**:
   - Run on devices, desktop, or in browser
   - Can't safely store secrets, so only access APIs on behalf of user
   - Don't have client secrets

2. **Confidential Client Applications**:
   - Run on servers (web apps, APIs, services/daemons)
   - Can keep application secrets
   - Have distinct configurations with client ID and secret


# Initializing MSAL Client Applications

The Microsoft Authentication Library (MSAL) provides two main types of client applications: public client applications and confidential client applications. To initialize these client applications, MSAL.NET 3.x recommends using the `PublicClientApplicationBuilder` and `ConfidentialClientApplicationBuilder` classes.

## Registering the Application

Before initializing a client application, you need to register it with the Microsoft identity platform. During the registration process, you'll obtain the following information:

- Client ID (a GUID)
- Authority (the identity provider URL and sign-in audience)
- Tenant ID (for line-of-business applications)
- Application secret or certificate (for confidential client apps)
- Redirect URI (for web apps and some public client apps)

## Initializing Public Client Applications

To initialize a public client application, use the `PublicClientApplicationBuilder`:

```csharp
IPublicClientApplication app = PublicClientApplicationBuilder.Create(clientId).Build();
```

This creates a public client application that can sign in users with their work/school accounts or personal Microsoft accounts in the Microsoft Azure public cloud.

## Initializing Confidential Client Applications

To initialize a confidential client application (e.g., a web app), use the `ConfidentialClientApplicationBuilder`:

```csharp
string redirectUri = "https://myapp.azurewebsites.net";
IConfidentialClientApplication app = ConfidentialClientApplicationBuilder.Create(clientId)
    .WithClientSecret(clientSecret)
    .WithRedirectUri(redirectUri)
    .Build();
```

This creates a confidential client application that can acquire tokens for users in the Microsoft Azure public cloud, using the provided client secret.

## Builder Modifiers

The application builder classes provide various modifiers that can be used to further configure the client application:

| Modifier | Description |
| --- | --- |
| `.WithAuthority()` | Sets the default authority (Azure cloud, audience, tenant) |
| `.WithTenantId(string)` | Overrides the tenant ID or tenant description |
| `.WithClientId(string)` | Overrides the client ID |
| `.WithRedirectUri(string)` | Overrides the default redirect URI |
| `.WithComponent(string)` | Sets the name of the library using MSAL.NET (for telemetry) |
| `.WithDebugLogging()` | Enables debug logging |
| `.WithLogging()` | Sets a custom logging callback |
| `.WithTelemetry(TelemetryCallback)` | Sets a custom telemetry callback |

## Confidential Client Modifiers

Confidential client applications have additional modifiers:

| Modifier | Description |
| --- | --- |
| `.WithCertificate(X509Certificate2)` | Sets the certificate identifying the application |
| `.WithClientSecret(string)` | Sets the client secret (app password) |

