# Manage Container Images in Azure Container Registry

Objectives:

- Explain the features and benefits Azure Container Registry offers.
- Describe how to use ACR Tasks to automate builds and deployments.
- Explain the elements in a Dockerfile.
- Build and run an image in the ACR by using Azure CLI.

## Exploring Azure Container Registry (ACR)

Azure Container Registry is a managed, private Docker registry service that allows you to build, store, and deploy container images across various Azure services and beyond.

### Key Features and Benefits of ACR

- **Private Docker Registry**: ACR provides a private, secure place to store and manage your container images.
- **Automated Builds**: ACR Tasks can automatically build and push container images when you commit code or update base images.
- **Multi-Architecture Support**: ACR supports building and storing container images for different CPU architectures (e.g., AMD64, ARM64).
- **Geo-Replication**: The Premium tier of ACR allows you to replicate your registry across multiple Azure regions.
- **Security and Access Control**: ACR integrates with Azure Active Directory for authentication and role-based access control.

### ACR Service Tiers

ACR is available in three main service tiers:

1. **Basic**: A cost-optimized entry point for learning and lower-usage scenarios.
2. **Standard**: Suitable for most production workloads, with increased storage and throughput.
3. **Premium**: Provides the highest storage and concurrency, along with advanced features like geo-replication and content trust.

### Using ACR Tasks for Automated Builds

ACR Tasks is a feature that allows you to automate the build, test, and deployment of container images. Some key use cases for ACR Tasks include:

- Automatically building images when you commit code to a source control repository.
- Rebuilding images when base images (e.g., OS or framework) are updated.
- Executing multi-step tasks to build, test, and push container images.

### Getting Started with ACR

To use ACR, you can:

1. Create an Azure Container Registry instance using the Azure Portal, Azure CLI, or Azure Resource Manager templates.
2. Push your container images to the registry using the Docker CLI or an integrated CI/CD tool.
3. Pull images from the registry to deploy your containerized applications on various Azure services, such as Azure Kubernetes Service (AKS), App Service, or Azure Functions.
4. Configure ACR Tasks to automate your container image build and deployment pipelines.

For more detailed information on working with Azure Container Registry, refer to the [official documentation](https://docs.microsoft.com/en-us/azure/container-registry/).
