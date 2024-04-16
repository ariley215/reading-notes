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

## Additional Resources

- [Azure Functions documentation](https://docs.microsoft.com/azure/azure-functions/)
- [Azure Functions triggers and bindings](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings)
- [Code and test Azure Functions locally](https://docs.microsoft.com/azure/azure-functions/functions-run-local)
