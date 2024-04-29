# Deploy a Container Instance using Azure CLI

## Sign-in to Azure and create the resource group

Sign-in to the Azure portal and open the Cloud Shell. When the shell opens, be sure to select the Bash environment.

Create a new resource group with the name `az204-aci-rg` so that it's easier to clean up these resources when you're finished with the module. Replace `<myLocation>` with a region near you.

```bash
az group create --name az204-aci-rg --location <myLocation>
```

## Create a container

You create a container by providing a name, a Docker image, and an Azure resource group to the `az container create` command. You expose the container to the Internet by specifying a DNS name label.

Create a DNS name to expose your container to the Internet. Your DNS name must be unique, run this command from Cloud Shell to create a variable that holds a unique name.

```bash
DNS_NAME_LABEL=aci-example-$RANDOM
```

Run the following `az container create` command to start a container instance. Be sure to replace the `<myLocation>` with the region you specified earlier. It takes a few minutes for the operation to complete.

```bash
az container create --resource-group az204-aci-rg \
    --name mycontainer \
    --image mcr.microsoft.com/azuredocs/aci-helloworld \
    --ports 80 \
    --dns-name-label $DNS_NAME_LABEL --location <myLocation>
```

In the previous command, `$DNS_NAME_LABEL` specifies your DNS name. The image name, `mcr.microsoft.com/azuredocs/aci-helloworld`, refers to a Docker image that runs a basic Node.js web application.

## Verify the container is running

When the `az container create` command completes, run `az container show` to check its status.

```bash
az container show --resource-group az204-aci-rg \
    --name mycontainer \
    --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" \
    --out table
```

You see your container's fully qualified domain name (FQDN) and its provisioning state.

> [!NOTE]
> If your container is in the Creating state, wait a few moments and run the command again until you see the Succeeded state.

From a browser, navigate to your container's FQDN to see it running. You may get a warning that the site isn't safe.

## Clean up resources

When no longer needed, you can use the `az group delete` command to remove the resource group, the container registry, and the container images stored there.

```bash
az group delete --name az204-aci-rg --no-wait
```
