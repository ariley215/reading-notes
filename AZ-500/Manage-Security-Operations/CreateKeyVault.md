# Create and configure an Azure Key Vault

## Creating an Azure Key Vault

### Create a Vault

1. Sign in to the [Azure portal](https://portal.azure.com).
2. From the Azure portal menu, or from the Home page, select **Create a resource**.
3. In the Search box, enter `Key Vault`.
4. From the results list, choose **Key Vault**.
5. On the Key Vault section, choose **Create**.
6. On the Create key vault section, provide the following information:
   - **Name**: Enter a unique name. Example: `Contoso-vault2`.
   - **Subscription**: Choose an active subscription.
   - **Resource Group**: Choose to create a new one or use an existing group.
   - **Location**: Choose a location from the pull-down menu.
   - Leave the other options to their defaults.
7. Select **Create**.

### Important Properties

- **Vault Name**: This is the unique name you chose for your Key Vault, e.g., `Contoso-Vault2`.
- **Vault URI**: This is the URI through which your Key Vault will be accessed, e.g., `https://contoso-vault2.vault.azure.net/`.

## Configure Azure Key Vault Networking Settings

1. Navigate to your newly created Key Vault in the Azure portal.

2. Select **Networking** from the side menu.

3. Click on the **Firewalls and virtual networks** tab.

4. Under **Allow access from**, select **Selected networks**.

5. To add existing virtual networks:
   - Click on **+ Add existing virtual networks**.
   - Choose the subscription, virtual networks, and subnets you want to allow.
   - If service endpoints are not enabled, confirm to enable them by selecting **Enable**.

6. Under **IP Networks**, add IPv4 address ranges in CIDR notation or individual IP addresses.

7. If you want to allow Microsoft Trusted Services to bypass the Key Vault Firewall:
   - Select 'Yes' to allow.
   - Review the current list of [Azure Key Vault Trusted Services](https://docs.microsoft.com/azure/key-vault/general/overview-vnet-service-endpoints#trusted-services).

8. Click **Save** to apply the changes.

### Adding New Virtual Networks and Subnets

- You can add new virtual networks by clicking on **+ Add new virtual network** and following the prompts to create and enable service endpoints for them.

## Notes

- Your Azure account is the only one authorized to perform operations on this new vault by default.
- It may take up to 15 minutes for networking changes to take effect.
- Always review the latest Azure documentation and best practices to ensure your Key Vault is configured securely.

This guide outlines the steps to create and configure an Azure Key Vault, which is essential knowledge for the AZ-500 certification exam. It includes instructions on setting up the Key Vault, as well as configuring networking settings to secure access to the vault.