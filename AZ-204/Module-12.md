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

## Exploring Azure Container Registry Storage Capabilities

Azure Container Registry (ACR) provides advanced storage features to secure and protect your container image data.

### Encryption-at-Rest

All container images stored in your ACR are encrypted at rest. Azure automatically encrypts images before storing them, and decrypts them on-the-fly when they are pulled.

### Regional Storage

ACR stores data in the same region where the registry is created, to meet data residency and compliance requirements. In most regions, data may also be replicated to a paired region within the same geography.

If a regional outage occurs, the registry data may become unavailable and is not automatically recovered. Customers who need multi-region resiliency should enable the geo-replication feature.

### Zone Redundancy (Premium Tier)

The Premium tier of ACR provides zone redundancy, which replicates your registry data across a minimum of three separate availability zones within an enabled region.

### Scalable Storage

ACR allows you to create as many repositories, images, layers, and tags as needed, up to the storage limit of your registry.

However, a large number of repositories and tags can impact registry performance. It's recommended to periodically delete unused resources as part of your registry maintenance.

### Data Deletion and Recovery

Deleted registry resources like repositories, images, and tags cannot be recovered after deletion. It's important to carefully manage your container image lifecycle and retention policies.

For more information on ACR storage capabilities and best practices, refer to the [official documentation](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-storage).

## Automating Container Builds and Deployments with ACR Tasks

Azure Container Registry (ACR) provides a powerful set of features called ACR Tasks, which allow you to automate the build, test, and deployment of container images.

### Key ACR Tasks Capabilities

- **Quick Tasks**: Quickly build and push a single container image on-demand, without needing a local Docker engine.
- **Automated Triggered Tasks**: Set up tasks to automatically build and push images when:
  - Source code is updated
  - Base images are updated
  - On a scheduled interval
- **Multi-Step Tasks**: Define complex, multi-stage workflows to build, test, and deploy container-based applications.
- **Multi-Architecture Support**: Build container images for different OS and CPU architectures (e.g., Linux/AMD64, Linux/ARM64, Windows/AMD64).

### Using ACR Tasks

You can create and manage ACR Tasks using the Azure CLI, Azure Portal, or Azure Resource Manager templates. Some key steps include:

1. **Define the Task**: Specify the container image build and push operations, as well as any additional steps, in a task definition. This can be a simple "quick task" or a more complex "multi-step task" defined in a YAML file.
2. **Set up Triggers**: Configure the task to be automatically triggered by source code updates, base image updates, or on a schedule.
3. **Execute the Task**: Manually run the task or let it be triggered automatically based on the defined events.

### Task Execution Environments

ACR Tasks can build container images for a variety of platforms, including:

- Linux (AMD64, ARM, ARM64, 386)
- Windows (AMD64)

You can specify the target platform when defining the task.

### Benefits of ACR Tasks

- **Automated Container Lifecycle**: Streamline the entire container build, test, and deployment process.
- **Centralized Management**: Manage all your container image building and deployment from a single, cloud-hosted service.
- **Reduced Operational Overhead**: Eliminate the need to manage your own build infrastructure.
- **Improved Security**: Leverage the security features of Azure Container Registry to protect your container images.

For more details on using ACR Tasks, refer to the [official documentation](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-tasks-overview).
Here is another study guide page on exploring elements of a Dockerfile:

Exploring Elements of a Dockerfile

A Dockerfile is a script that contains a series of instructions used to build a Docker image. The main elements of a Dockerfile include:

1. **Base Image**: The starting point for your image, specified using the `FROM` instruction. This is typically an OS image like Ubuntu or Alpine.

2. **Installation and Configuration**: Commands like `RUN`, `COPY`, and `ADD` are used to install packages, copy files, and configure the image.

   - `RUN` executes commands in a new layer on top of the current image.
   - `COPY` copies files from the host machine into the container image.
   - `ADD` is similar to `COPY` but can also download remote files and extract compressed archives.

3. **Ports**: The `EXPOSE` instruction informs Docker that the container will listen on the specified network ports at runtime.

4. **Volumes**: The `VOLUME` instruction creates a mount point for directories within the container to store data.

5. **Environment Variables**: The `ENV` instruction sets environment variables in the container that can be used at runtime.

6. **Working Directory**: The `WORKDIR` instruction sets the working directory for any `RUN`, `CMD`, `ENTRYPOINT`, `COPY`, and `ADD` instructions that follow it.

7. **Startup Command**: The `CMD` instruction specifies the default command to run when starting the container. There can only be one `CMD` instruction in a Dockerfile.

8. **Startup Arguments**: The `ENTRYPOINT` instruction configures the container to run as an executable, allowing you to set arguments for the default command.

9. **Build Arguments**: The `ARG` instruction defines a variable that users can pass at build-time to the builder with the `--build-arg` flag.

10. **Label Metadata**: The `LABEL` instruction adds metadata to an image, such as the author, version, or license.

Here's an example Dockerfile that incorporates many of these elements:

```Dockerfile
# Use the latest Ubuntu image as the base
FROM ubuntu:latest 

# Install required packages
RUN apt-get update && apt-get install -y \
    nginx \
    git \
    curl

# Copy the application code
COPY . /var/www/myapp

# Set the working directory
WORKDIR /var/www/myapp

# Build the application
RUN npm install && npm run build

# Expose port 80 to the host
EXPOSE 80

# Set an environment variable
ENV APP_ENV production

# Define the startup command
CMD ["nginx", "-g", "daemon off;"]
```

This Dockerfile:

1. Uses the latest Ubuntu image as the base.
2. Installs Nginx, Git, and Curl using `RUN`.
3. Copies the application code to the `/var/www/myapp` directory.
4. Sets the working directory to `/var/www/myapp`.
5. Builds the application using `npm install` and `npm run build`.
6. Exposes port 80 to the host.
7. Sets an environment variable `APP_ENV` to `production`.
8. Defines the startup command to run Nginx in the foreground.

Understanding the different elements of a Dockerfile is crucial for building and managing Docker images effectively.
