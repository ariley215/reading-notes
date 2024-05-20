# Explore Azure App Service

Objectives:

- Describe Azure App Service key components and value.
- Explain how Azure App Service manages authentication and authorization.
- Identify methods to control inbound and outbound traffic to your web app.
- Deploy an app to App Service using Azure CLI commands.

## Examine Azure App Service

### Overview

- **Service Type:** HTTP-based service for hosting web applications, REST APIs, and mobile back ends.
- **Development:** Supports various programming languages and frameworks.
- **Environment:** Runs on both Windows and Linux-based environments.

### Built-in Auto Scale Support

- **Scale Up/Down:**
  - Adjust the resources of the underlying machine (e.g., number of cores, amount of RAM).
- **Scale Out/In:**
  - Increase or decrease the number of machine instances running the web app.

### Continuous Integration/Deployment Support

- **Out-of-the-Box CI/CD:**
  - Integrates with Azure DevOps Services, GitHub, Bitbucket, FTP, or local Git repository.
  - Auto-syncs code and future changes to the web app.

### Deployment Slots

- **Usage:**
  - Available in Standard App Service Plan tier or higher.
- **Features:**
  - Separate deployment slots for staging and production.
  - Live apps with their own host names.
  - Swap app content and configuration between slots.

### App Service on Linux

- **Hosting:**
  - Natively hosts web apps on Linux.
  - Supports custom Linux containers (Web App for Containers).
- **Supported Languages and Frameworks:**
  - Node.js
  - Java (JRE 8 & JRE 11)
  - PHP
  - Python
  - .NET
  - Ruby
- **Custom Containers:**
  - Deploy custom runtime environments if not supported by built-in images.
- **Retrieve Supported Languages:**
  - Use the following command in the Cloud Shell:

    ```bash
    az webapp list-runtimes --os-type linux
    ```

### Limitations

- **Pricing Tier:**
  - Not supported on Shared pricing tier.
- **Portal Features:**
  - Only features that work for Linux apps are shown.
  - Features are enabled as they become available.
- **Storage Volume:**
  - Built-in images use a storage volume backed by Azure Storage.
  - Disk latency is higher and more variable than container filesystem.
  - Heavy read-only access apps may benefit from custom container option for better performance.

## Examine Azure App Service Plans

### Overview

- **Purpose:**
  - An App Service plan defines a set of compute resources for a web app.
  - Multiple apps can run on the same computing resources within an App Service plan.

### Configuration

- **Region:**
  - Defines where the compute resources are located (e.g., West Europe, East US).

- **Operating System:**
  - Windows or Linux.

- **VM Instances:**
  - Number and size (Small, Medium, Large).

- **Pricing Tier:**
  - Determines features and cost.
  - Options include Free, Shared, Basic, Standard, Premium, PremiumV2, PremiumV3, Isolated, IsolatedV2.

### Pricing Tiers

- **Shared Compute:**
  - **Free and Shared:**
    - Runs on the same Azure VM as other App Service apps.
    - Allocates CPU quotas, no scale-out capability.
    - Intended for development and testing.

- **Dedicated Compute:**
  - **Basic, Standard, Premium, PremiumV2, PremiumV3:**
    - Runs on dedicated Azure VMs.
    - Only apps in the same App Service plan share resources.
    - Higher tiers provide more VM instances for scale-out.

- **Isolated:**
  - **Isolated and IsolatedV2:**
    - Runs on dedicated Azure VMs within dedicated Azure Virtual Networks.
    - Provides network and compute isolation.
    - Maximum scale-out capabilities.

### App Running and Scaling

- **Free and Shared Tiers:**
  - App receives CPU minutes on a shared VM instance.
  - No scale-out capability.

- **Other Tiers:**
  - App runs on all VM instances configured in the App Service plan.
  - Multiple apps share the same VM instances.
  - Deployment slots and additional features (logs, backups, WebJobs) use the same resources.

- **Scaling:**
  - The App Service plan is the scale unit.
  - All apps in the plan scale together based on settings.
  - Autoscaling affects all apps in the plan.

### Enhancing Capabilities and Features

- **Scaling Up/Down:**
  - Change the pricing tier to scale the App Service plan.
  
- **Isolating Apps:**
  - Move resource-intensive apps to a separate App Service plan.
  - Scale apps independently.
  - Allocate resources in different geographical regions.

- **Cost Management:**
  - Multiple apps in one plan can save money.
  - Ensure understanding of resource capacity and expected load.

### When to Isolate Apps

- App is resource-intensive.
- Need to scale the app independently.
- Require resources in a different region.
- Greater control over app resources and performance.

## Deploy to App Service

### Overview

App Service supports both automated and manual deployment methods to meet the unique requirements of development teams.

### Automated Deployment

- **Purpose:**
  - Push out new features and bug fixes quickly and repetitively.
  - Minimize the impact on end users.

- **Supported Sources:**
  - **Azure DevOps Services:**
    - Push code, build, run tests, generate release, and deploy to Azure Web App.
  - **GitHub:**
    - Connect GitHub repository for automatic deployment upon changes to the production branch.
  - **Bitbucket:**
    - Similar to GitHub, configure automated deployment.

### Manual Deployment

- **Options:**
  - **Git:**
    - Use App Service web app's Git URL as a remote repository. Push to deploy.
  - **CLI:**
    - `az webapp up` command packages and deploys your app. Can create a new web app if not existing.
  - **Zip Deploy:**
    - Use `curl` or a similar HTTP utility to send a ZIP file of your application to App Service.
  - **FTP/S:**
    - Use FTP or FTPS to push your code traditionally.

### Use Deployment Slots

- **Purpose:**
  - Deploy new production builds with minimal downtime.
  
- **Features:**
  - Available in Standard App Service Plan tier or better.
  - Deploy to a staging environment, then swap with production.
  - Swap operation warms up necessary worker instances to match production scale, eliminating downtime.

## Explore Authentication and Authorization in App Service

### Built-in Authentication and Authorization

- **Purpose:**
  - Sign in users and access data with minimal or no code.
  - Supports web apps, RESTful APIs, mobile back ends, and Azure Functions.

### Benefits of Built-in Authentication

- **Ease of Use:**
  - Integrate auth capabilities without coding.
  - Built into the platform, no SDKs or security expertise required.

- **Multiple Providers:**
  - Microsoft Entra ID, Facebook, Google, Twitter.
  - Focus on application development rather than auth implementation.

### Identity Providers

| Provider                      | Sign-in Endpoint           | How-To Guidance                                |
|-------------------------------|----------------------------|------------------------------------------------|
| Microsoft identity platform   | `/.auth/login/aad`         | [App Service Microsoft identity platform login](https://docs.microsoft.com/azure/app-service/app-service-authentication-overview) |
| Facebook                      | `/.auth/login/facebook`    | [App Service Facebook login](https://docs.microsoft.com/azure/app-service/app-service-authentication-overview) |
| Google                        | `/.auth/login/google`      | [App Service Google login](https://docs.microsoft.com/azure/app-service/app-service-authentication-overview) |
| Twitter                       | `/.auth/login/twitter`     | [App Service Twitter login](https://docs.microsoft.com/azure/app-service/app-service-authentication-overview) |
| Any OpenID Connect provider   | `/.auth/login/<provider>`  | [App Service OpenID Connect login](https://docs.microsoft.com/azure/app-service/app-service-authentication-overview) |
| GitHub                        | `/.auth/login/github`      | [App Service GitHub login](https://docs.microsoft.com/azure/app-service/app-service-authentication-overview) |

### How It Works

- **Authentication and Authorization Module:**
  - Runs in the same sandbox as your application code.
  - Handles HTTP requests before reaching the application code.
  - Functions:
    - Authenticate users and clients.
    - Validate, store, and refresh OAuth tokens.
    - Manage authenticated sessions.
    - Inject identity information into HTTP request headers.
  - Configurable via Azure Resource Manager or configuration files.
  - No SDKs or specific programming languages required.

- **Linux and Containers:**
  - Module runs in a separate container.
  - No direct integration with specific language frameworks.

### Authentication Flow

| Step                          | Without Provider SDK                     | With Provider SDK                                       |
|-------------------------------|------------------------------------------|---------------------------------------------------------|
| **Sign user in**              | Redirects client to `/.auth/login/<provider>`. | Client signs in with provider's SDK and gets a token. |
| **Post-authentication**       | Provider redirects client to `/.auth/login/<provider>/callback`. | Client posts token to `/.auth/login/<provider>` for validation. |
| **Establish authenticated session** | App Service adds authenticated cookie to response. | App Service returns its own authentication token. |
| **Serve authenticated content** | Client includes authentication cookie in subsequent requests. | Client presents authentication token in `X-ZUMO-AUTH` header. |

### Authorization Behavior

- **Options:**
  - **Allow Unauthenticated Requests:**
    - Defers authorization to application code.
    - Passes authentication information in HTTP headers for authenticated requests.
  - **Require Authentication:**
    - Rejects unauthenticated traffic.
    - Redirects browser clients to `/.auth/login/<provider>`.
    - Returns HTTP 401 Unauthorized or HTTP 403 Forbidden for all requests.

  > **Caution:**
  > Restricting access applies to all calls to your app, which may not be desirable for apps wanting a publicly available home page, such as single-page applications.

### Token Store

- **Built-In Token Store:**
  - Repository of tokens associated with users.
  - Immediately available when authentication is enabled.

### Logging and Tracing

- **Application Logging:**
  - Collects authentication and authorization traces in log files.
  - Useful for diagnosing unexpected authentication errors.

## Discover App Service Networking Features

### Overview

- **Internet Access:**
  - Apps hosted in App Service are accessible directly through the internet.
  - Can only reach internet-hosted endpoints by default.

### Deployment Types

- **Multitenant Public Service:**
  - Hosts App Service plans in Free, Shared, Basic, Standard, Premium, PremiumV2, and PremiumV3 pricing SKUs.

- **Single-Tenant App Service Environment (ASE):**
  - Hosts Isolated SKU App Service plans directly in your Azure virtual network.

### Multi-Tenant App Service Networking Features

- **System Roles:**
  - **Front Ends:** Handle incoming HTTP/HTTPS requests.
  - **Workers:** Host customer workloads.

- **Network Configuration:**
  - Multitenant network shared by many customers.
  - Cannot directly connect App Service network to your private network.

### Networking Features

- **Inbound Features:**
  - **App-assigned Address:** Provides dedicated IP addresses for SSL and unique inbound needs.
  - **Access Restrictions:** Restricts access to your app from specified IP addresses.
  - **Service Endpoints:** Connect securely to Azure services within a virtual network.
  - **Private Endpoints:** Provide private access to your app within your virtual network.

- **Outbound Features:**
  - **Hybrid Connections:** Connects your app to on-premises resources.
  - **Gateway-required Virtual Network Integration:** Integrates your app with virtual networks via a gateway.
  - **Virtual Network Integration:** Directly integrates your app with virtual networks.

### Use Cases for Inbound Features

| Inbound Use Case                                       | Feature                  |
|--------------------------------------------------------|--------------------------|
| Support IP-based SSL needs for your app                | App-assigned address     |
| Support unshared dedicated inbound address for your app| App-assigned address     |
| Restrict access to your app from specific addresses    | Access restrictions      |

### Default Networking Behavior

- **Scale Units:**
  - Support multiple customers per deployment.
  - Free and Shared SKU plans use multitenant workers.
  - Basic and higher plans use dedicated workers.

- **VM Instances:**
  - Apps in a Standard App Service plan run on the same worker VM.
  - Scaling out replicates apps on new worker VMs.

### Outbound Addresses

- **Worker VM Types:**
  - Different App Service plans use different VM types.
  - Changing VM family results in different outbound IP addresses.

- **Outbound IP Addresses:**
  - Addresses shared by all apps on the same worker VM family.
  - Property `possibleOutboundIpAddresses` lists all potential outbound addresses.

### Finding Outbound IP Addresses

- **Azure Portal:**
  - Navigate to **Properties** in your app's left-hand navigation.

- **Azure CLI Commands:**
  - **Current Outbound IP Addresses:**

    ```bash
    az webapp show \
        --resource-group <group_name> \
        --name <app_name> \
        --query outboundIpAddresses \
        --output tsv
    ```

  - **All Possible Outbound IP Addresses:**

    ```bash
    az webapp show \
        --resource-group <group_name> \
        --name <app_name> \
        --query possibleOutboundIpAddresses \
        --output tsv
    ```

### Notes

- **App Service Plans:**
  - Free and Shared plans run on shared virtual machines.
  - Basic and higher plans run on dedicated virtual machines.
  - PremiumV2 and PremiumV3 plans use more advanced VM types.

- **Caution:**
  - Outbound IP addresses can change when scaling or changing the VM family.
