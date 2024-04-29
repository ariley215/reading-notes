# Implement Azure Container Apps

Objectives:

- Describe the features benefits of Azure Container Apps
- Deploy container app in Azure by using the Azure CLI
- Utilize Azure Container Apps built-in authentication and authorization
- Create revisions and implement app secrets

## Explore Azure Container Apps

## Overview

- Azure Container Apps is a serverless platform that runs on top of Azure Kubernetes Service (AKS) and enables you to run microservices and containerized applications.
- Common use cases include deploying API endpoints, hosting background processing applications, handling event-driven processing, and running microservices.
- Applications built on Azure Container Apps can dynamically scale based on HTTP traffic, event-driven processing, CPU or memory load, and any KEDA-supported scaler.

## Key Features

- Run multiple container revisions and manage the container app's application lifecycle
- Autoscale apps based on any KEDA-supported scale trigger (most apps can scale to zero)
- Enable HTTPS ingress without having to manage other Azure infrastructure
- Split traffic across multiple versions of an app for Blue/Green deployments and A/B testing
- Use internal ingress and service discovery for secure internal-only endpoints with built-in DNS-based service discovery
- Build microservices with Dapr and access its rich set of APIs
- Run containers from any registry, public or private, including Docker Hub and Azure Container Registry (ACR)
- Manage apps using the Azure CLI extension, Azure portal, or ARM templates
- Provide an existing virtual network when creating an environment for your container apps
- Securely manage secrets directly in your application
- Monitor logs using Azure Log Analytics

## Azure Container Apps Environments

- Individual container apps are deployed to a single Container Apps environment, which acts as a secure boundary around groups of container apps.
- Reasons to deploy container apps to the same environment include managing related services, deploying different apps to the same virtual network, and instrumenting Dapr applications that communicate via the Dapr service invocation API.
- Reasons to deploy container apps to different environments include ensuring two applications never share the same compute resources and preventing Dapr applications from communicating via the Dapr service invocation API.

## Microservices with Azure Container Apps

- Microservice architectures allow you to independently develop, upgrade, version, and scale core areas of functionality in an overall system.
- Azure Container Apps provides the foundation for deploying microservices with features like independent scaling, versioning, upgrades, service discovery, and native Dapr integration.
- Dapr integration provides a richer microservices programming model with features like observability, pub/sub, and service-to-service invocation with mutual TLS, retries, and more.

# Containers in Azure Container Apps

## Container Configuration

- Containers can use any runtime, language, or stack
- Supports Linux-based x86-64 (linux/amd64) container images
- Containers are automatically restarted if they crash

![Containers in Azure Container Apps](https://learn.microsoft.com/en-us/training/wwl-azure/implement-azure-container-apps/media/azure-container-apps-containers.png)

```json
"containers": [
  {
    "name": "main",
    "image": "[parameters('container_image')]",
    "env": [
      {
        "name": "HTTP_PORT",
        "value": "80"
      },
      {
        "name": "SECRET_VAL",
        "secretRef": "mysecret"
      }
    ],
    "resources": {
      "cpu": 0.5,
      "memory": "1Gi"
    },
    "volumeMounts": [
      {
        "mountPath": "/myfiles",
        "volumeName": "azure-files-volume"
      }
    ],
    "probes": [
      {
        "type": "liveness",
        "httpGet": {
          "path": "/health",
          "port": 8080,
          "httpHeaders": [
            {
              "name": "Custom-Header",
              "value": "liveness probe"
            }
          ]
        },
        "initialDelaySeconds": 7,
        "periodSeconds": 3
      }
    ]
  }
]
```

## Multiple Containers

- You can define multiple containers in a single container app for the sidecar pattern
- Examples of sidecar containers:  - Agent that reads logs from the primary app container and forwards them to a logging service  - Background process that refreshes a cache used by the primary app container

## Container Registries

- Use private container registries by defining credentials in the registries array

```json
{  "registries": [{
    "server": "docker.io",
    "username": "my-registry-user-name",
    "passwordSecretRef": "my-password-secret-name"  }]
}
```

## Limitations

- Cannot run privileged containers
- Only Linux-based (linux/amd64) container images are supported

# Implement Authentication and Authorization in Azure Container Apps

## Overview

- Azure Container Apps provides built-in authentication and authorization features to secure your external ingress-enabled container apps.
- The authentication feature allows you to integrate with various identity providers without writing any custom code.
- This feature should only be used with HTTPS, and `allowInsecure` should be disabled on your container app's ingress configuration.

## Identity Providers

Azure Container Apps supports the following identity providers out of the box:

| Provider | Sign-in Endpoint |
| --- | --- |
| Microsoft Identity Platform | `/.auth/login/aad` |
| Facebook | `/.auth/login/facebook` |
| GitHub | `/.auth/login/github` |
| Google | `/.auth/login/google` |
| Twitter | `/.auth/login/twitter` |
| Any OpenID Connect provider | `/.auth/login/<providerName>` |

You can configure your container app to use one or more of these providers for user authentication.

## Feature Architecture

- The authentication and authorization middleware runs as a sidecar container on each replica in your application.
- When enabled, every incoming HTTP request passes through the security layer before being handled by your application.
- The middleware handles user authentication, session management, and injecting identity information into HTTP request headers.

![Authentication and Authorization in Azure Container Apps](https://learn.microsoft.com/en-us/training/wwl-azure/implement-azure-container-apps/media/container-apps-authorization-architecture.png)

## Authentication Flow

The authentication flow differs depending on whether you want to sign in with the provider's SDK:

1. **Without provider SDK (server-directed flow or server flow)**: The application delegates federated sign-in to Container Apps, which is typical for browser apps.
2. **With provider SDK (client-directed flow or client flow)**: The application signs users in to the provider manually and then submits the authentication token to Container Apps for validation, which is typical for browser-less apps like native mobile apps.

## Configuration

- To restrict app access only to authenticated users, set the `Restrict access` setting to `Require authentication`.
- To authenticate but not restrict access, set the `Restrict access` setting to `Allow unauthenticated access`.


# Manage Revisions and Secrets in Azure Container Apps

## Revisions

- Azure Container Apps implements container app versioning by creating revisions
- A revision is an immutable snapshot of a container app version
- You can use revisions to release a new version or quickly revert to an earlier version
- New revisions are created when you update your application with revision-scope changes, such as:
  - Changing the container image
  - Modifying environment variables
  - Updating compute resources (CPU, memory)
  - Scaling parameters
- You can customize the revision name by setting the revision suffix
- You can list all revisions associated with a container app using the `az containerapp revision list` command

```bash
# Update container app with a new image, creating a new revision
az containerapp update \
  --name <APPLICATION_NAME> \
  --resource-group <RESOURCE_GROUP_NAME> \
  --image <IMAGE_NAME>

# List all revisions for a container app
az containerapp revision list \
  --name <APPLICATION_NAME> \
  --resource-group <RESOURCE_GROUP_NAME> \
  -o table
```

## Secrets

- Azure Container Apps allows your application to securely store sensitive configuration values as secrets
- Secrets are scoped to an application, outside of any specific revision
- Adding, removing, or changing secrets doesn't generate new revisions
- You can reference secrets in environment variables for your container app
- Container Apps doesn't support direct Azure Key Vault integration, but you can use managed identity to access Key Vault from your app

```bash
# Create a container app with a secret
az containerapp create \
  --resource-group "my-resource-group" \
  --name queuereader \
  --environment "my-environment-name" \
  --image demos/queuereader:v1 \
  --secrets "queue-connection-string=$CONNECTION_STRING"

# Reference a secret in an environment variable
az containerapp create \
  --resource-group "my-resource-group" \
  --name myQueueApp \
  --environment "my-environment-name" \
  --image demos/myQueueApp:v1 \
  --secrets "queue-connection-string=$CONNECTIONSTRING" \
  --env-vars "QueueName=myqueue" "ConnectionString=secretref:queue-connection-string"
```

- When a secret is updated or deleted, you should deploy a new revision that no longer references the old secret, then deactivate all revisions that reference the deleted secret.
- Ensure you deploy a new revision before deleting a secret to avoid breaking existing revisions that rely on the secret.

# Explore Dapr Integration with Azure Container Apps

## Dapr Overview

- Dapr is a distributed application runtime that simplifies the development of microservice-based applications.
- Dapr provides capabilities for enabling application intercommunication through messaging, service-to-service calls, state management, and more.
- Dapr is an open-source, Cloud Native Computing Foundation (CNCF) project.

## Dapr Integration in Azure Container Apps

- Azure Container Apps provides a managed and supported Dapr integration, handling Dapr version upgrades seamlessly.
- Dapr is exposed to each container app through a Dapr sidecar, allowing your app to invoke Dapr APIs via HTTP or gRPC.

![Dapr Integration in Azure Container Apps](https://learn.microsoft.com/en-us/training/wwl-azure/implement-azure-container-apps/media/azure-container-apps-distributed-application-runtime-building-blocks.png)

## Dapr APIs

Azure Container Apps supports the following stable Dapr APIs:

| Dapr API | Description |
| --- | --- |
| Service-to-service Invocation | Discover services and perform reliable, direct service-to-service calls with automatic mTLS authentication and encryption. |
| State Management | Provides state management capabilities for transactions and CRUD operations. |
| Pub/Sub | Allows publisher and subscriber container apps to intercommunicate via an intermediary message broker. |
| Bindings | Trigger your applications based on events. |
| Actors | Dapr actors are message-driven, single-threaded, units of work designed to quickly scale. |
| Observability | Send tracing information to an Application Insights backend. |
| Secrets | Access secrets from your application code or reference secure values in your Dapr components. |

## Dapr Enablement

You can configure Dapr in Azure Container Apps using:

- Container Apps CLI
- Infrastructure as Code (IaC) templates (Bicep or ARM)
- Azure Portal

## Dapr Components and Scopes

- Dapr uses a modular design where functionality is delivered as a component.
- Dapr components are environment-level resources that can be shared across container apps or scoped to specific container apps.
- Application scopes can be used to ensure components are loaded at runtime by only the appropriate container apps.
