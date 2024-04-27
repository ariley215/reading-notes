# Run Container Images in Azure Container Instances

Objectives:

- Describe the benefits of Azure Container Instances and how resources are grouped.
- Deploy a container instance in Azure by using the Azure CLI.
- Start and stop containers using policies.
- Set environment variables in your container instances.
- Mount file shares in your container instances.

## Explore Azure Container Instances

### Overview

- ACI offers the fastest and simplest way to run containers in Azure, without having to manage virtual machines or adopt a higher-level service.
- ACI provides benefits like fast startup, direct internet access, hypervisor-level security, flexible resource allocation, and support for both Linux and Windows containers.
- For scenarios requiring full container orchestration, Azure Kubernetes Service (AKS) is the recommended solution.

### Container Groups

- The top-level resource in ACI is the container group, which is a collection of containers scheduled on the same host machine.
- Containers in a group share a lifecycle, resources, local network, and storage volumes.
- Container groups can include multiple containers, expose public IP addresses and ports, and mount Azure file shares as volumes.
- **Note: Multi-container groups currently support only Linux containers. For Windows containers, Azure Container Instances only supports deployment of a single instance.**

### Deployment

- Container groups can be deployed using either Resource Manager templates or YAML files.
- Resource Manager templates are recommended when deploying additional Azure resources, while YAML files are more concise for container-only deployments.
- **Note: Multi-container groups currently support only Linux containers. For Windows containers, Azure Container Instances only supports deployment of a single instance.**

### Resource Allocation

- ACI allocates resources like CPU, memory, and GPUs (preview) to a container group by adding the resource requests of the instances in the group.

### Networking

- Container groups share an IP address and port namespace, allowing external clients to access containers within the group.
- Containers within a group can reach each other via localhost on the exposed ports, even if those ports aren't exposed externally.

### Storage

- ACI supports mounting external volumes like Azure file shares, secrets, empty directories, and cloned Git repos into container groups.

### Common Scenarios

- Multi-container groups are useful for dividing a single functional task into separate container images, such as:
  - Web app container and content update container
  - Application container and logging container
  - Front-end and back-end containers


## Running Containerized Tasks with Restart Policies in Azure Container Instances

### Overview

- Azure Container Instances (ACI) provides a compelling platform for executing run-once tasks:
  - build
  - test
  - image rendering
- Configurable restart policies, you can specify how containers should behave when their processes complete.
- ACI bills by the second, you're only charged for the compute resources used while the container is running.

### Container Restart Policies

ACI supports three restart policy settings when creating a container group:

1. **Always**: Containers in the group are always restarted (default setting).
2. **Never**: Containers in the group are never restarted and run at most once.
3. **OnFailure**: Containers in the group are restarted only when the process executed in the container fails (non-zero exit code). Containers run at least once.

### Specifying a Restart Policy

To set the restart policy when creating a container, use the `--restart-policy` parameter with the `az container create` command:

```bash
az container create \
    --resource-group myResourceGroup \
    --name mycontainer \
    --image mycontainerimage \
    --restart-policy OnFailure
```

### Run to Completion

When ACI stops a container with a `Never` or `OnFailure` restart policy, the container's status is set to `Terminated`. This indicates the container has run to completion.

### Benefits

- The ability to execute run-once tasks in containers provides a flexible and cost-effective solution.
- By specifying the appropriate restart policy, you can control how containers behave when their processes complete.
- Only paying for the compute resources used while the container is running can result in significant cost savings.

### Use Cases

- Build and test automation
- Batch processing and data transformation
- Rendering and media processing
- Any other short-lived, run-once tasks that can be containerized

## Setting Environment Variables in Container Instances

### Overview

- Setting environment variables in your container instances allows you to provide dynamic configuration for the application or script running in the container.
- Environment variables are similar to the `--env` command-line argument used with `docker run`.
- Azure Container Instances supports secure values for environment variables, which is safer and more flexible than including sensitive information in your container's image.

### Setting Environment Variables

To set environment variables when creating a container, use the `--environment-variables` parameter with the `az container create` command:

```bash
az container create \
    --resource-group myResourceGroup \
    --name mycontainer2 \
    --image mcr.microsoft.com/azuredocs/aci-wordcount:latest
    --restart-policy OnFailure \
    --environment-variables 'NumWords'='5' 'MinLength'='8'
```

### Secure Values

- Secure values are intended to hold sensitive information like passwords or keys for your application.
- Secure environment variables are not visible in your container's properties, and their values can only be accessed from within the container.
- To set a secure environment variable, use the `secureValue` property instead of the regular `value` property.

Example YAML:

```yaml
apiVersion: 2018-10-01
location: eastus
name: securetest
properties:
  containers:
  - name: mycontainer
    properties:
      environmentVariables:
        - name: 'NOTSECRET'
          value: 'my-exposed-value'
        - name: 'SECRET'
          secureValue: 'my-secret-value'
      image: nginx
      ports: []
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
  osType: Linux
  restartPolicy: Always
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

You can deploy this container group using the following command:

```bash
az container create --resource-group myResourceGroup --file secure-env.yaml
```

## Mounting an Azure File Share in Azure Container Instances

### Overview

- Azure Container Instances (ACI) are stateless by default, so you need to mount a volume from an external store to persist state beyond the container's lifetime.
- Azure Container Instances can mount an Azure Files share, which provides fully managed file shares in the cloud accessible via the SMB protocol.
- Using an Azure Files share with ACI provides file-sharing features similar to using it with Azure Virtual Machines.

### Limitations

- Azure file share volume mounts are only supported for Linux containers.
- The Linux container must run as root to mount the Azure file share.
- Azure file share volume mounts are limited to CIFS support.

### Deploying Container and Mounting Volume

To mount an Azure file share as a volume in a container using the Azure CLI, specify the share and volume mount point when creating the container with `az container create`:

```bash
az container create \
    --resource-group $ACI_PERS_RESOURCE_GROUP \
    --name hellofiles \
    --image mcr.microsoft.com/azuredocs/aci-hellofiles \
    --dns-name-label aci-demo \
    --ports 80 \
    --azure-file-volume-account-name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --azure-file-volume-account-key $STORAGE_KEY \
    --azure-file-volume-share-name $ACI_PERS_SHARE_NAME \
    --azure-file-volume-mount-path /aci/logs/
```

### Deploying with YAML

You can also deploy a container group and mount a volume using a YAML template. This is the preferred method when deploying container groups with multiple containers.

Example YAML:

```yaml
apiVersion: '2019-12-01'
location: eastus
name: file-share-demo
properties:
  containers:
  - name: hellofiles
    properties:
      environmentVariables: []
      image: mcr.microsoft.com/azuredocs/aci-hellofiles
      ports:
      - port: 80
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
      volumeMounts:
      - mountPath: /aci/logs/
        name: filesharevolume
  osType: Linux
  restartPolicy: Always
  ipAddress:
    type: Public
    ports:
      - port: 80
    dnsNameLabel: aci-demo
  volumes:
  - name: filesharevolume
    azureFile:
      sharename: acishare
      storageAccountName: <Storage account name>
      storageAccountKey: <Storage account key>
tags: {}
type: Microsoft.ContainerInstance/containerGroups
```

### Mounting Multiple Volumes

To mount multiple volumes in a container instance, you must deploy using an Azure Resource Manager template or a YAML file. Provide the share details and define the volumes in the `volumes` array in the `properties` section.
