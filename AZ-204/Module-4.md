# Explore Azure App Service Deployment Slots

After completing this module, you'll be able to:

- Describe the benefits of using deployment slots
- Understand how slot swapping operates in App Service
- Perform manual swaps and enable auto swap
- Route traffic manually and automatically

When deploying applications to Azure App Service, you can use separate deployment slots instead of the default production slot. This allows you to test and validate changes in a staging environment before swapping them into production.

Benefits of Deployment Slots:

- Validate app changes in a staging slot before swapping to production
- Swapping slots ensures all instances are warmed up, eliminating downtime
- You can quickly swap back to the "last known good" version if needed
- No extra charge for using deployment slots

Deployment Slot Capabilities:

- Available on Standard, Premium, and Isolated App Service plan tiers
- Each tier supports a different number of deployment slots
- New slots have no content by default, can deploy from different branch/repo
- Slots share the same resource allocation as the App Service plan

Scaling Slot Considerations:

- When scaling your app's tier, ensure the target tier supports the number of slots you are using
  - For example, you can't scale down to Standard tier if you have more than 5 slots

Deploying and Swapping Slots:

- You can deploy app content and configuration to a non-production slot
- Swapping slots makes the staged version the new production environment
- Automated swap workflows can be configured to eliminate downtime

Monitoring Staging Deployments:

- Activity logs track all swap operations and can trigger alerts
- Validate changes thoroughly in the staging slot before production.

## Examine Slot Swapping

Slot Swap Process:

- App settings, connection strings, and other config from target slot are applied to source slot, causing a restart of source instances.
- Wait for all source instances to warm up and respond successfully.
- Swap the routing rules between the two slots, making the source slot the new production environment.
- Repeat the same configuration and warm-up process for the new source slot.

Swapped Settings:

- General settings (framework, bitness, web sockets) are swapped
- App settings and connection strings (can be configured as "slot-specific")
- Handler mappings, public certificates, web jobs

Non-Swapped Settings:

- Publish endpoints, custom domains, SSL certs
- Scale settings, IP restrictions, Always On
- Diagnostic log settings, CORS, virtual network integration
- Managed indentities, Settings that end with the suffix _EXTENSION_VERSION

Swapping Best Practices:

- Always keep production as the target slot to avoid impacting live traffic
- Use "Swap with Preview" to validate changes before final swap
- Configure app settings/connections as "slot-specific" to control swapping
- Use the WEBSITE_OVERRIDE_PRESERVE_DEFAULT_STICKY_SLOT_SETTINGS app setting to make all settings swappable

## Swap Deployment Slots

Manually Swapping Slots

1. Go to the Deployment Slots page and select "Swap"
1. Choose the Source and Target slots (usually production is the Target)
1. Review the configuration changes that will be applied
1. Click "Swap" to complete the swap immediately

Swap with Preview (Multi-Phase Swap)

- Performs the same swap process but pauses after the first phase
- Allows you to validate the app in the source slot with the target slot's settings
- Complete the swap when ready by selecting "Complete Swap"
- Can also cancel a pending swap by selecting "Cancel Swap"

Configuring Auto Swap

- Enables automatically swapping the app to production when changes are deployed to a source slot
- Configured on the "Configuration > General Settings" page
- Specifies the source slot to auto swap from and the target production slot
- Triggers an automatic swap after the source slot is warmed up

Customizing Warm-Up

- Some apps may require custom initialization actions before swapping
- Use the <applicationInitialization> element in web.config to specify initialization paths

```web.config
<system.webServer>
    <applicationInitialization>
        <add initializationPage="/" hostName="[app hostname]" />
        <add initializationPage="/Home/About" hostName="[app hostname]" />
    </applicationInitialization>
</system.webServer>
```

- App settings like WEBSITE_SWAP_WARMUP_PING_PATH and WEBSITE_SWAP_WARMUP_PING_STATUSES can also customize the warm-up behavior

Rollback and Monitoring Swaps

- If issues occur after a swap, you can immediately swap the slots back to the previous state
- Check the Activity Log for details on swap operations and any errors that occurred
- The Activity Log can provide insights into the progress and status of a lengthy swap process


Key Concepts:

1. Default Traffic Routing: By default, all client requests to the app's production URL (http://<app_name>.azurewebsites.net) are routed to the production slot.

1. Automatic Traffic Routing: You can route a portion of the production traffic to another slot. This is useful for getting user feedback on a new update before fully releasing it to production.

- To enable automatic traffic routing, go to the Deployment Slots section of your app's resource page and specify a percentage (between 0 and 100) of traffic to route to another slot.
- Clients are randomly routed to the non-production slot based on the specified percentage.
- Clients are "pinned" to the slot they are routed to for the life of their session, which can be seen in the x-ms-routing-name cookie.

3. Manual Traffic Routing: In addition to automatic traffic routing, App Service can route requests to a specific slot manually.

- This is useful for allowing users to opt in or out of a beta app.
- To route traffic manually, use the x-ms-routing-name query parameter.
- Setting x-ms-routing-name=self will route the user to the production slot.
- Setting x-ms-routing-name=<non-production-slot-name> will route the user to the specified non-production slot.

4. Hiding Deployment Slots: You can "hide" a non-production slot from the public by setting the routing percentage to 0%. This allows internal teams to test changes on the slot, but users cannot access it without using the x-ms-routing-name query parameter.
