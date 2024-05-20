## Create a Static HTMLSite web app 

In this exercise, you'll deploy a basic HTML+CSS site to Azure App Service by using the Azure CLI `az webapp up` command. You'll then update the code and redeploy it by using the same command.

The `az webapp up` command makes it easy to create and update web apps. When executed it performs the following actions:

- Create a default resource group if one isn't specified.
- Create a default app service plan.
- Create an app with the specified name.
- Zip deploy files from the current working directory to the web app.

### Download the Sample App

In the sandbox, create a directory and then navigate to it:

```bash
mkdir htmlapp
cd htmlapp
```

Run the following git command to clone the sample app repository to your `htmlapp` directory:

```bash
git clone https://github.com/Azure-Samples/html-docs-hello-world.git
```

Set variables to hold the resource group and app names:

```bash
resourceGroup=$(az group list --query "[].{id:name}" -o tsv)
appName=az204app$RANDOM
```

### Create the Web App

Change to the directory that contains the sample code and run the `az webapp up` command:

```bash
cd html-docs-hello-world
az webapp up -g $resourceGroup -n $appName --html
```

This command may take a few minutes to run. While running, it displays information similar to the example below:

```json
{
"app_url": "https://<myAppName>.azurewebsites.net",
"location": "westeurope",
"name": "<app_name>",
"os": "Windows",
"resourcegroup": "<resource_group_name>",
"serverfarm": "appsvc_asp_Windows_westeurope",
"sku": "FREE",
"src_path": "/home/<username>/demoHTML/html-docs-hello-world ",
< JSON data removed for brevity. >
}
```

Open a new tab in your browser and navigate to the app URL (`https://<myAppName>.azurewebsites.net`) and verify the app is running - take note of the title at the top of the page. Leave the browser open on the app for the next section.

### Update and Redeploy the App

In the Cloud Shell, type `code index.html` to open the editor. In the `<h1>` heading tag, change "Azure App Service - Sample Static HTML Site" to "Azure App Service Updated" or to anything else that you'd like.

Use the commands `ctrl-s` to save and `ctrl-q` to exit.

Redeploy the app with the same `az webapp up` command you used earlier:

```bash
az webapp up -g $resourceGroup -n $appName --html
```

Once deployment is completed, switch back to the browser from the "Create the Web App" section above and refresh the page.
