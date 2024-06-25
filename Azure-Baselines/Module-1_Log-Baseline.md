# Create a logging and monitoring baseline

A proper logging policy ensures that it can be determine when a security violation has occurred.

- Potentially identify responsible party.
- Provide data about external access to a resource,
- Diagnostic logs
  - Information about the operation of a specific resource

**An Azure activity log is a subscription log that provides insight into subscription-level events that occurred in Azure. By using the activity log, you can determine the what, who, and when for any write operations that occurred on the resources in your subscription.**

## Logging Policy Recommendations

### Ensure that a diagnostic setting exists - Level 1

The activity log previously was called an audit log or an operational log.

The Administrative category reports control-plane events for your subscriptions.

Each Azure subscription has a single activity log

Diagnostic logs are emitted by a resource.

- Provide information about the operation of the resource.
- Must enable diagnostic settings for each resource.

1. Sign in to the Azure portal. Search for and select Monitor.
2. In the left menu, select Activity log.
3. In the Activity log menu bar, select Export activity logs.
4. If no settings are shown, select your subscription, and then select Add diagnostic setting.
5. Enter a name for your diagnostic setting, and then select log categories and destination details.
6. In the menu bar, select Save.

### Create an activity log alert for creating a policy assignment - Level 1

- Can see which users can create policies.
  -Might help you detect a breach or misconfiguration

1. Sign in to the Azure portal. Search for and select Monitor.
2. In the left menu, select Alerts.
3. In the Alerts menu bar, select the Create dropdown, and then select Alert rule.
4. In the Create an alert rule pane, select Select scope.
5. In the Select a resource pane, in the Filter by resource type dropdown, select Policy assignment (policyAssignments).
6. Select a resource to monitor.
7. Select Done.
8. To finish creating the alert, complete the steps that are described in Create an alert rule from the [Azure Monitor Alerts](https://learn.microsoft.com/en-us/azure/azure-monitor/alerts/alerts-activity-log#create-an-alert-rule-from-the-azure-monitor-alerts-pane) pane.

### Create an activity log alert for creating, updating, or deleting a network security group - Level 1

By default, no monitoring alerts are created when NSGs are created, updated, or deleted.

- changing/ deleting NSGs can allow internal resources to be accessed from improper sources or for unexpected outbound network traffic.

1. Sign in to the Azure portal. Search for and select Monitor.
2. In the left menu, select Alerts.
3. In the Alerts menu bar, select the Create dropdown, and then select Alert rule.
4. In the Create an alert rule pane, select Select scope.
5. In the Select a resource pane, in the Filter by resource type dropdown, select Network security groups.
6. Select Done.
7. To finish creating the alert, complete the steps that are described in Create an alert rule from the Azure Monitor Alerts pane.

### Create an activity log alert for creating or updating a SQL Server firewall rule - Level 1

Provides insight into network access changes, and it might reduce the time it takes to detect suspicious activity.

1. Sign in to the Azure portal. Search for and select Monitor.
2. In the left menu, select Alerts.
3. In the Alerts menu bar, select the Create dropdown, and then select Alert rule.
4. In the Create alert rule pane, select Select scope.
5. In the Select a resource pane, in the Filter by resource type dropdown, select SQL servers.
6. Select Done.
7. To finish creating the alert, complete the steps that are described in Create an alert rule from the Azure Monitor Alerts pane.
