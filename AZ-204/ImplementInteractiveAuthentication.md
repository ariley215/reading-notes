# Initializing MSAL Client Applications

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

By understanding how to initialize public and confidential client applications, and the various configuration options available, you can set up your MSAL-powered applications to integrate with the Microsoft identity platform.
