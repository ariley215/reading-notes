# Create Blob storage resources by using the .NET client library

To create a how-to guide for the provided content, you can follow the steps below:

Title: How to Create Blob Storage Resources Using the .NET Client Library

Introduction:
This guide will walk you through the process of creating Blob storage resources using the Azure Blob storage client library in a console app. You will learn how to perform various actions on Azure Blob storage, including creating a container, uploading blobs, listing blobs, downloading blobs, and deleting a container.

Prerequisites:
Before you begin, make sure you have the following:

- An Azure account with an active subscription. If you don't have one, you can sign up for a free trial at [https://azure.com/free](https://azure.com/free).
- Visual Studio Code installed on one of the supported platforms.
- .NET 6 as the target framework.
- The C# extension for Visual Studio Code.
- Azure CLI installed.

Step 1: Setting up

1. Start Visual Studio Code and open a terminal window.
2. Login to Azure by running the command `az login` in the terminal.
3. Create a resource group for the exercise by running the command `az group create --location <myLocation> --name az204-blob-rg`. Replace `<myLocation>` with a region near you.
4. Create a storage account by running the command `az storage account create --resource-group az204-blob-rg --name <myStorageAcct> --location <myLocation> --sku Standard_LRS`. Replace `<myLocation>` with the same region you used for the resource group and `<myStorageAcct>` with a unique name.
5. Get the credentials for the storage account from the Azure portal.

Step 2: Prepare the .NET project

1. Open the terminal in Visual Studio Code and navigate to a directory where you want to store your project.
2. Run the command `dotnet new console -n az204-blob` to create a new console app named `az204-blob`.
3. Switch to the newly created `az204-blob` folder by running the command `cd az204-blob`.
4. Build the app by running the command `dotnet build`.
5. Create a folder named `data` inside the `az204-blob` folder by running the command `mkdir data`.
6. Install the Azure Blob Storage client library for .NET package by running the command `dotnet add package Azure.Storage.Blobs`.
7. Open the `Program.cs` file in your editor and replace its contents with the provided code.

Step 3: Build the full app

1. Set the `storageConnectionString` variable in the `ProcessAsync` method to the connection string you copied from the portal.
2. Copy and append each code snippet from the "Build the full app" section to the previous snippet in the `ProcessAsync` method of the `Program.cs` file.
3. Save the changes.

Step 4: Run the code

1. In the terminal, run the command `dotnet build` to build the app.
2. Run the command `dotnet run` to run the app.
3. Follow the prompts in the app to perform various actions on Azure Blob storage.

Step 5: Clean up other resources

1. After you have finished running the app, you can delete the resources created for this exercise by running the command `az group delete --name az204-blob-rg --no-wait`. Confirm the deletion when prompted.

Conclusion:
In this guide, you learned how to create Blob storage resources using the Azure Blob storage client library in a console app. You performed actions such as creating a container, uploading blobs, listing blobs, downloading blobs, and deleting a container.
