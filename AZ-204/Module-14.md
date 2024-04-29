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

![Authentication and Authorization in Azure Container Apps][]

## Authentication Flow

The authentication flow differs depending on whether you want to sign in with the provider's SDK:

1. **Without provider SDK (server-directed flow or server flow)**: The application delegates federated sign-in to Container Apps, which is typical for browser apps.
2. **With provider SDK (client-directed flow or client flow)**: The application signs users in to the provider manually and then submits the authentication token to Container Apps for validation, which is typical for browser-less apps like native mobile apps.

## Configuration

- To restrict app access only to authenticated users, set the `Restrict access` setting to `Require authentication`.
- To authenticate but not restrict access, set the `Restrict access` setting to `Allow unauthenticated access`.
