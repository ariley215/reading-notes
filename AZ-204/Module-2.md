# Configure Web App Settings

- Create application settings that are bound to deployment slots.
- Explain the options for installing SSL/TLS certificates for your app.
- Enable diagnostic logging for your app to aid in monitoring and debugging.
- Create virtual app to directory mappings

In App Service, app settings are variables passed as environment variables to the application code.

- For **Linux** apps and custom containers, App Service passes app settings to the container using the --env flag to set the environment variable in the container.

App settings are always encrypted when stored (encrypted-at-rest).

## Adding and editing settings

To add a new app setting, select New application setting. If you're using deployment slots you can specify if your setting is swappable or not. In the dialog, you can stick the setting to the current slot.

In a default, or custom, Linux container any nested JSON key structure in the app setting name like ApplicationInsights:InstrumentationKey needs to be configured in App Service as ApplicationInsights__InstrumentationKey for the key name. In other words, any : should be replaced by__ (double underscore).

To add or edit app settings in bulk, select the Advanced edit button.

this is the format:

```json
[
  {
    "name": "<key-1>",
    "value": "<value-1>",
    "slotSetting": false
  },
  {
    "name": "<key-2>",
    "value": "<value-2>",
    "slotSetting": false
  },
  ...
]
```

## Configure connection strings

For ASP.NET and ASP.NET Core developers, the values you set in App Service override the ones in Web.config.

For others, better to use app settings.

- connection strings require special formatting in the variable keys in order to access the values.
- Connection strings are always encrypted when stored (encrypted-at-rest)
- Adding and editing connection strings follow the same principles as other app settings
*There is one case where you may want to use connection strings instead of app settings for non-.NET languages: certain Azure database types are backed up along with the app only if you configure a connection string for the database in your App Service app.*

ex.
```json
[
  {
    "name": "name-1",
    "value": "conn-string-1",
    "type": "SQLServer",
    "slotSetting": false
  },
  {
    "name": "name-2",
    "value": "conn-string-2",
    "type": "PostgreSQL",
    "slotSetting": false
  },
  ...
]
```

## General Settings

A list of the currently available settings:

1. Stack Setting

- The software stack to run the app, including the language and SDK versions. For Linux apps and custom container apps, you can also set an optional start-up command or file.

1. Platform settings: settings for the hosting platform

- Bitness: 32-bit or 64-bit.
- WebSocket protocol
- Always On
  - Keep the app loaded even when there's no traffic.
  - By default, Always On isn't enabled and the app is unloaded after 20 minutes without any incoming requests.
- Managed pipline version- IIS pipeline mode
- HTTP Version
- ARR affinity

1. Debugging
1. Incoming client certificates

- require client certificates in mutual authentication. TLS mutual authentication is used to restrict access to your app by enabling different types of authentication for it.

## Configure path mappings

Configure handler mappings, and virtual application and directory mappings.

### Windows apps (uncontainerized)

**Handler mappings** let you add custom script processors to handle requests for specific file extensions.

settings:

- Extension
- Script processor
- arguments

Each app has the default root path (/) mapped to **D:\home\site\wwwroot**, where your code is deployed by default. 

If your app root is in a different folder, or if your repository has more than one application, you can edit or add virtual applications and directories.

### Linux and containerized apps

You can add custom storage for your containerized app(all linux apps and any systems custom container).

**Name:** The display name.

**Configuration options:** Basic or Advanced.

**Storage accounts:** The storage account with the container you want.

**Storage type:** Azure Blobs or Azure Files. Windows container apps only support Azure Files.

**Storage container:** For basic configuration, the container you want.

**Share name:** For advanced configuration, the file share name.

**Access key:** For advanced configuration, the access key.

**Mount path:** The absolute path in your container to mount the custom storage.

## Enable diagnostic logging

