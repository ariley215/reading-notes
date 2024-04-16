# Exploring Azure Functions Development

## Key Concepts

- **Function Content and Configuration**: Every Azure Function contains two important components - your code (written in various languages) and the function.json configuration file. The configuration file defines the function's trigger, bindings, and other settings.
- **Function.json File Structure**: The function.json file includes the following key properties:
  - `disabled`: Indicates whether the function is disabled.
  - `bindings`: An array that configures both triggers and bindings. Each binding requires a `type`, `direction`, and `name`.

```json
{
    "disabled":false,
    "bindings":[
        // ... bindings here
        {
            "type": "bindingType",
            "direction": "in",
            "name": "myParamName",
            // ... more depending on binding
        }
    ]
}
```

- **Function App**: A function app provides the execution context in Azure for your functions. It is the unit of deployment and management for your functions, and all functions in a function app share the same pricing plan, deployment method, and runtime version.
- **Folder Structure**: The code and configuration for a function app is organized in a root project folder, which contains a host.json file for runtime-specific configurations, a bin folder for packages and libraries, and language-specific folder structures.
- **Local Development**: Functions can be developed and tested locally using your favorite code editor and development tools, which can connect to live Azure services. The local development approach depends on your language and tooling preferences.

## Key Takeaways

- Azure Functions have two main components: your code and the function.json configuration file.
- The function.json file defines the trigger, bindings, and other configuration settings for the function.
- A function app is the unit of deployment and management for your functions, and all functions in an app share the same runtime and resources.
- Functions can be developed and tested locally using a variety of tools and languages, and then deployed to Azure.
- Avoid mixing local development with portal development in the same function app.

----------------------------------------------------------------------------------------

# Triggers and Bindings in Azure Functions

## Key Concepts

- **Triggers**: Triggers define how a function is invoked. A function must have exactly one trigger, which provides the function with data (the function's payload).
- **Bindings**: Bindings are a declarative way of connecting other resources to a function. Bindings can be input bindings, output bindings, or both.
- **Trigger and Binding Definitions**: The way triggers and bindings are configured depends on the development language:
  - C# class library: Use C# attributes
  - Java: Use Java annotations
  - JavaScript/PowerShell/Python/TypeScript: Update the function.json file

## Trigger and Binding Configuration

- **function.json Configuration**: For languages that use the function.json file (JavaScript, PowerShell, Python, TypeScript), the bindings are defined in this file. The `bindings` property is where you configure both triggers and bindings.
- **Binding Direction**: Triggers always have an `in` direction. Input and output bindings use `in` and `out` directions respectively. Some bindings support an `inout` direction.
- **Binding Data Types**: For dynamically typed languages, use the `dataType` property in function.json to specify the data type (e.g., `binary`, `stream`, `string`).

## Examples

Let's look at an example of a function that writes a new row to Azure Table storage whenever a new message appears in Azure Queue storage:

**function.json**

```json
{
  "bindings": [
    {
      "type": "queueTrigger",
      "direction": "in",
      "name": "order",
      "queueName": "myqueue-items",
      "connection": "MY_STORAGE_ACCT_APP_SETTING"
    },
    {
      "type": "table",
      "direction": "out",
      "name": "$return",
      "tableName": "outTable",
      "connection": "MY_TABLE_STORAGE_ACCT_APP_SETTING"
    }
  ]
}
```

**C# Script Example**

```csharp
#r "Newtonsoft.Json"

using Microsoft.Extensions.Logging;
using Newtonsoft.Json.Linq;

public static Person Run(JObject order, ILogger log)
{
    return new Person() { 
            PartitionKey = "Orders", 
            RowKey = Guid.NewGuid().ToString(),  
            Name = order["Name"].ToString(),
            MobileNumber = order["MobileNumber"].ToString() };  
}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
    public string MobileNumber { get; set; }
}
```

## Key Takeaways

- Triggers define how a function is invoked, and functions must have exactly one trigger.
- Bindings are a declarative way to connect other resources to a function, and a function can have multiple input and output bindings.
- The way triggers and bindings are configured depends on the development language, with C# using attributes, Java using annotations, and script languages using the function.json file.
- Binding directions are `in`, `out`, and sometimes `inout`, and data types can be specified for dynamically typed languages.
- Triggers and bindings allow you to avoid hardcoding access to other services, as the function receives and sends data through the bindings.

Here's a study guide page on "Connecting Azure Functions to Azure Services":

# Connecting Azure Functions to Azure Services

## Key Concepts

- **Connection Configuration**: Azure Functions references connection information by name from its configuration provider, rather than directly accepting the connection details. This allows the connections to be changed across environments.
- **Configuration Providers**: The default configuration provider for Azure Functions uses environment variables that are set in the Application Settings when running in the Azure Functions service, or from the local settings file when developing locally.
- **Identity-based Connections**: Some connections in Azure Functions can be configured to use an identity instead of a secret. This depends on the extension using the connection.
  - When hosted in the Azure Functions service, identity-based connections use a managed identity (system-assigned or user-assigned).
  - When run in other contexts, like local development, the developer's identity is used.

## Configuring Identity-based Connections

1. **Grant Permissions to the Identity**: The identity being used must have the necessary permissions to perform the intended actions. This is typically done by:
   - Assigning a role in Azure RBAC
   - Specifying the identity in an access policy, depending on the target service
2. **Adhere to Least Privilege**: Ensure that the identity is only granted the required permissions, following the principle of least privilege.

## Example: Connecting to Azure Blob Storage

Let's say you have a function that needs to access Azure Blob Storage. Here's how you can configure the connection:

1. **In the function.json file**, configure the Blob storage input/output bindings:

```json
{
  "bindings": [
    {
      "name": "blob",
      "type": "blob",
      "direction": "in",
      "path": "samples-workitems/{name}",
      "connection": "AzureWebJobsStorage"
    }
  ]
}
```

2. **In the Application Settings (or local.settings.json)**, provide the connection string for the "AzureWebJobsStorage" setting.
3. **Grant the Azure Functions managed identity (or user-assigned identity) the necessary permissions** to access the Blob storage container.

## Key Takeaways

- Azure Functions references connection information by name from its configuration provider, not directly in the code.
- The default configuration provider uses environment variables, which can be set in the Azure portal or the local settings file.
- Identity-based connections can be used, where the managed identity (system-assigned or user-assigned) is used instead of a connection string.
- The identity used must be granted the necessary permissions to access the target Azure service.
- Follow the principle of least privilege when granting permissions to the identity.

## Additional Resources

- [Azure Functions triggers and bindings](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings)
- [Develop Azure Functions using Visual Studio Code](https://docs.microsoft.com/azure/azure-functions/functions-develop-vs-code)
- [Azure Functions developer reference](https://docs.microsoft.com/azure/azure-functions/functions-reference)
- [Azure Functions documentation](https://docs.microsoft.com/azure/azure-functions/)
- [Azure Functions triggers and bindings](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings)
- [Code and test Azure Functions locally](https://docs.microsoft.com/azure/azure-functions/functions-run-local)
