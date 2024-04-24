# Explore Azure Functions

## Learning Objectives

- Explain functional differences between Azure Functions, Azure Logic Apps, and WebJobs
- Describe Azure Functions hosting plan options
- Describe how Azure Functions scale to meet business needs

## What is Azure Functions?

- Azure Functions is a serverless compute service that allows you to run code (functions) on-demand without having to manage any infrastructure.
- It enables you to build applications with more focus on writing useful code instead of worrying about the underlying infrastructure.
- Azure Functions supports various programming languages, including C#, F#, JavaScript, Python, Java, and PowerShell.

## Key Concepts

1. **Triggers:** Triggers are the events that cause a function to run, such as an HTTP request, a timer, or a message being added to a queue.
2. **Bindings:** Bindings are a declarative way to connect data to your function. They handle the details of how the function interacts with various data stores.
3. **Serverless Architecture:** Azure Functions follows a serverless architecture, where the cloud infrastructure automatically provisions, scales, and manages the resources needed to run your code.
4. **Orchestration:** Azure Functions supports creating complex orchestrations, which are a collection of functions or actions that are executed to accomplish a complex task.

## Comparing Azure Functions and Azure Logic Apps

- Azure Functions is a code-first (imperative) approach, while Azure Logic Apps is a designer-first (declarative) approach.
- Functions offers more flexibility in terms of connectivity and custom actions, while Logic Apps provides a larger collection of pre-built connectors and actions.
- Functions runs in Azure, locally, or on-premises, while Logic Apps runs primarily in Azure.

## Comparing Functions and WebJobs

- Both are built on Azure App Service and share many of the same event triggers and connections to Azure services.

**Key differences:**

- Functions offers a serverless app model with automatic scaling, while WebJobs does not.
- Functions provide a browser-based development and testing experience, while WebJobs does not.
- Functions have a pay-per-use pricing model, while WebJobs does not.
- Functions integrate with Logic Apps, while WebJobs do not.

### Azure Functions Hosting Plans

When you create a function app in Azure, you must choose a hosting plan for your app. There are three main hosting plans available for Azure Functions:

1. **Consumption Plan**

   - This is the default hosting plan.
   - It scales automatically, and you only pay for compute resources when your functions are running.
   - Instances of the Functions host are dynamically added and removed based on the number of incoming events.

2. **Premium Plan**

   - Automatically scales based on demand using pre-warmed workers. 
      - run applications with no delay after being idle.
   - Runs on more powerful instances and connects to virtual networks.

3. **Dedicated Plan (App Service Plan)**

   - Run your functions within an App Service plan at regular App Service plan rates.
   - Best for long-running scenarios where Durable Functions can't be used.

### Hosting Plan Comparison

| Plan             | Scaling Behavior        | Max Instances |
|------------------|-------------------------|--------------|
| Consumption Plan | Event-driven, automatic scaling | Windows: 200, Linux: 100 |
| Premium Plan     | Event-driven, automatic scaling | Windows: 100, Linux: 20-100 |
| Dedicated Plan   | Manual/auto-scale       | 10-20        |

### Other Hosting Options

In addition to the main hosting plans, there are two other hosting options for Azure Functions:

- **App Service Environment (ASE):** Provides a fully isolated and dedicated environment for securely running App Service apps at high scale.
- **Kubernetes (Direct or Azure Arc):** Provides a fully isolated and dedicated environment running on top of the Kubernetes platform.

### Function App Timeout Duration

The `functionTimeout` property in the `host.json` project file specifies the timeout duration for functions in a function app. The default and maximum values vary by hosting plan:

| Plan             | Default (minutes) | Maximum (minutes) |
|------------------|-------------------|-------------------|
| Consumption Plan | 5                 | 10                |
| Premium Plan     | 30                | Unlimited         |
| Dedicated Plan   | 30                | Unlimited         |

### Storage Account Requirements

On any plan, a function app requires a general Azure Storage account that supports:

- Blob  
- Queue
- Files
- Table storage

The same storage account can be used by your triggers and bindings to store application data. 

- For storage-intensive operations, you should use a separate storage account.

## Key Takeaways

- Understand the different hosting plans available for Azure Functions and their scaling behaviors.
- Determine the appropriate hosting plan based on your function app's requirements, such as scalability, performance, and networking needs.
- Be aware of the timeout duration limits and storage account requirements for each hosting plan.
- Evaluate the use of other hosting options, such as App Service Environment or Kubernetes, for your specific use cases.

### Scaling Overview

- In the Consumption and Premium plans, Azure Functions scales CPU and memory resources by adding more instances of the Functions host.
- The number of instances is determined by the number of events that trigger a function.

### Instance Specifications

- Each instance of the Functions host in the Consumption plan is limited to 1.5 GB of memory and one CPU.
- An instance of the host is the entire function app, meaning all functions within a function app share resources within an instance and scale at the same time.
- In the Premium plan, the plan size determines the available memory and CPU for all apps in that plan on that instance.

### Function Code Storage

- Function code files are stored on Azure Files shares on the function's main storage account.
- Deleting the main storage account of the function app will delete the function code files, and they cannot be recovered.

### Runtime Scaling

- Azure Functions uses a scale controller component to monitor the rate of events and determine whether to scale out or scale in.
- The scale controller uses heuristics for each trigger type, such as queue length and message age for Azure Queue storage triggers.
- The unit of scale for Azure Functions is the function app. When the function app is scaled out, more resources are allocated to run multiple instances of the Azure Functions host.
- As compute demand is reduced, the scale controller removes function host instances, eventually scaling in to zero instances when no functions are running.

### Scaling Behaviors

- Maximum instances: A single function app only scales out to a maximum of 200 instances (Windows) or 100 instances (Linux).
- New instance rate: For HTTP triggers, new instances are allocated at most once per second. For non-HTTP triggers, new instances are allocated at most once every 30 seconds.

### Limiting Scale-Out

- You can restrict the maximum number of instances a function app can scale out to by modifying the `functionAppScaleLimit` value.
- This is commonly done to limit the impact on downstream components with limited throughput.
- The `functionAppScaleLimit` can be set to 0 or null for unrestricted scaling, or a valid value between 1 and the app maximum.

### Key Takeaways

1. Understand how Azure Functions scales CPU and memory resources by adding more instances of the Functions host.
2. Recognize the instance specifications and resource sharing within a function app for the Consumption and Premium plans.
3. Be aware of the function code storage and the implications of deleting the main storage account.
4. Familiarize yourself with the runtime scaling behavior, including the use of the scale controller and the scaling limits.
5. Learn how to limit the maximum scale-out of a function app to manage the impact on downstream components.
