# Practie Exam Questions and Answers

You have a Microsoft 365 E5 subscription that includes a user named User1.

You need to ensure all email sent to User1, including malicious email, is excluded from filtering by Microsoft Defender for Office 365.

What should you configure?

Select only one answer.

- a quarantine policy
- **a SecOps mailbox**
- a Tenant Allow/Block List
- Enhanced Filtering

*A SecOps mailbox is a dedicated mailbox that is used by security teams to collect and analyze unfiltered messages (both good and bad). Filters in EOP and Defender for Office 365 take no action on email messages sent to a SecOps mailbox.*

*Quarantine policies allow an administrator to control what users can do to quarantined messages based on why the message was quarantined.*

*Enhanced Filtering allows that the IP address and sender information is preserved.*

*The Tenant Allow/Block List in the Microsoft 365 Defender portal gives you a way to manually override Microsoft 365 filtering verdicts. The Tenant Allow/Block List is used during mail flow for incoming messages (this does not apply to intra-org messages) and at the time of user clicks.*

You have a Microsoft 365 E5 subscription.

You need to ensure that you can identify and protect sensitive documents in the subscription. The solution must ensure that you can prevent users from sending sensitive information to users in external organizations by using Microsoft Teams or Slack. The solution must use real-time content inspection.

What should you deploy?

Select only one answer.

- **Microsoft Defender for Cloud Apps**
- Microsoft Defender for Containers
- Microsoft Defender for Endpoint
- Microsoft Defender for Office 365

*Defender for Cloud Apps is a Cloud Access Security Broker (CASB) that adds safeguards to your organization's use of cloud services by enforcing enterprise security policies. CASB acts as a gatekeeper to broker access in real time between your enterprise users and the cloud resources they use, wherever your users are located and regardless of the device they are using.*

*Defender for Office 365 is a cloud-based email filtering service that protects your business from threats to email and collaboration tools.*

*Defender for Endpoint is an enterprise endpoint security platform designed to help enterprise networks prevent, detect, investigate, and respond to advanced threats.*

*Defender for Containers is a cloud-native solution used to secure your containers so that you can improve, monitor, and maintain the security of your clusters, containers, and their applications.*

You have a Microsoft 365 E5 subscription that contains a user named User1.

You need to prevent User1 from sending more than 20 email messages per day.

What should you configure in Microsoft Defender for Office 365?

Select only one answer.

- an anti-phishing policy
- **an anti-spam policy**
- the Tenant Allow/Block Lists
- Enhanced Filtering

*In an outbound anti-spam policy, you can configure users or groups for which the policy applies, as well as a daily message limit, which limits the number of email messages a user can send.*

*An anti-phishing policy provides spoof protection and mailbox intelligence for recipients.*

*The Tenant Allow/Block List in the Microsoft Defender portal gives you a way to manually override Microsoft 365 filtering verdicts. The Tenant Allow/Block List is used during mail flow for incoming messages (this does not apply to intra-org messages) and at the time of user clicks.*

*Enhanced Filtering allows that the IP address and sender information is preserved*

You have a Microsoft Sentinel workspace.

You need to create a variable named WLVar1 that will contain a list of available watchlists.

Which query should you run?

Select only one answer.

- _GetWatchlist('WLVar1')
- let WLVar1 = _GetWatchlist();
- let WLVar1 = _GetWatchlist('All');
- **let WLVar1 = Watchlist;**

*The Watchlist command lists all the available watchlist in the workspace. When saved to a variable, the information can be used to select the appropriate watchlist as part of the query.*

*let WLVar1 = _GetWatchlist(); requires the name of the watchlist to be present or provided in the rest of the query.*

*let WLVar1 = _GetWatchlist('All'); will get the watchlist called ‘All’.*

*_GetWatchlist('WLVar1') will get the watchlist called ‘WLVar1’.*

You have a Microsoft Sentinel workspace.

You need to create an analytics rule that will use entity mapping.

Which type of rule should you create?

Select only one answer.

- anomaly
- fusion
- machine learning (ML) behavioral analytics
- **scheduled**

*Entity mapping is an integral part of the configuration of scheduled query analytics rules, but it can also be used in near-real-time (NRT) rules. It enriches the rules’ output (alerts and incidents) with essential information that serves as the building blocks of any investigative processes and remedial actions that follow. Entity mapping is available only in scheduled and NRT rules, it is not available with other analytics rule types.*

You have a Microsoft Sentinel workspace that has a Microsoft Defender for Cloud data connector.

You create a Microsoft incident creation rule named Rule1.

You need to ensure that Rule1 creates incidents based on Defender for Cloud alerts.

What should you configure in Rule1?

Select only one answer.

- a rule query
- alert details
- **analytics rule logic**
- entity mapping

*When you are creating a Microsoft incident creation rule, in the analytics rule logic settings, you select the product for which you want to create incidents from alerts, such as Defender for Cloud or Microsoft Entra ID Protection. Rule queries, alert details and entity mapping are available in scheduled riles (queries), but they are unavailable in Microsoft incident creation rules.*

You have a Microsoft Sentinel workspace that contains the following query.

```powershell
OfficeActivity
| where TimeGenerated > ago(7h)
| where Operation !contains "Mail"
| project TimeGenerated, UserId, Operation, OfficeWorkload, RecordType, _ResourceId
| sort by TimeGenerated desc nulls last
The query filters the office activity table to remove results that refer to email, and project selected information from the table in descending order.
```

You need to create a custom parser named Parser1 that will run the query.

What should you do first?

Select only one answer.

- Remove | sort by TimeGenerated desc nulls last.
- Remove the TimeGenerated column from the results.
- **Remove | where TimeGenerated > ago(7h).**
- Replace the !contains operator with the !has operator.

*A parser should not include a filter by time. The query that uses the parser will apply the required time range.
Replacing !contains will not correct the time filter.
Removing TimeGenerated from the results will not correct the time filter.
Removing the sort function will not correct the time filter.*

You have a Microsoft Sentinel workspace.

You need to prevent a built-in Advanced Security Information Model (ASIM) parser from being updated automatically.

Which two actions can you perform? Each answer presents a complete solution.

Select all answers that apply.

- **Build a custom unifying parser and include the built-in parser version**.
- Create a hunting query and reference the built-in parser.
- **Create an analytics rule and include the built-in parser.**
- Redeploy the built-in parser and specify a CallerContext value of Any and a SourceSpecificParser value of Any.
- Redeploy the built-in parser and specify a CallerContext value of Built-in.

*Use the following process to prevent automatic updates for built-in, source-specific parsers:*

*Add the built-in parser version you want to use, such as _Im_Dns_AzureFirewallV02, to the custom unifying parser.
Add an exception for the built-in parser. For example, when you want to entirely opt out from automatic updates, and, therefore, exclude many built-in parsers, add the following:*

*A record with Any as the SourceSpecificParser value to exclude all parsers for CallerContext
A record for Any in the CallerContext and SourceSpecificParser fields to exclude all built-in parsers*

*Creating an analytics rule and including the built-in parser will not prevent the parser from updating.*

*Creating a hunting query and referencing the built-in parser will not prevent the parser from updating.*

*Redeploying the built-in parser and specifying a CallerContext value of Built-in will not prevent the parser from updating.*

You have a Microsoft Sentinel workspace.

You are testing a parser.

You need to improve query performance during the testing.

What should you do?

Select only one answer.

- Add a watchlist.
- **Add filtering parameters.**
- Configure a source-specific parser.
- Define a time filter.

*Using parsers can affect query performance, primarily from filtering the results after parsing. For this reason, many parsers have optional filtering parameters, which enable you to filter before parsing and enhance query performance. With query optimization and pre-filtering efforts, Advanced Security Information Model (ASIM) parsers often provide better performance compared to not using normalization at all.*

*Adding a watchlist will not improve the performance of the parser.*

*Adding a time filter will not improve the performance of the parser.*

*Configuring a source-specific parser will not improve the performance of the parser.*

You have a Microsoft 365 E5 subscription.

You are implementing Microsoft Defender for Endpoint.

You need to ensure that you can block specific IP addresses and URLs.

Which advanced feature should you configure?

Select only one answer.

- **Custom network indicators**
- Enable EDR in block mode
- Restrict correlation to within scoped device groups
- Web content filtering

*Custom network indicators configure devices to allow or block connections to IP addresses, domains, or URLs in custom indicator lists.*

*Web content filtering blocks access to websites that contain unwanted content and tracks web activity across all domains. To specify the web content categories you want to block, create a web content filtering policy.*

*When Enable EDR in block mode is turned on, Defender for Endpoint leverages behavioral blocking and containment capabilities by blocking malicious artifacts or behaviors observed through post-breach EDR capabilities.*

*When the Restrict correlation to within scoped device groups setting is turned on, alerts are correlated into separate incidents based on their scoped device group. By default, incident correlation occurs across the entire tenant scope.*

You have a Microsoft 365 E5 subscription.

You plan to deploy Microsoft Defender for Endpoint to meet the following requirements:

Block executable content from email messages.

Block unsigned processes that run from USB drives.

Which Defender for Endpoint capability should you use?

- Attack surface reduction
- Block at first site
- **Controlled folder access**
- Exploit protection

*Incorrect: Attack surface reduction –– Provides rules to target certain software behaviors, such as launching executable files and scripts and running unsigned processes.*
*Incorrect: Block at first site –– A threat protection feature that detects new malware and blocks it within seconds.*
*Correct: Controlled folder access –– Protects data by checking apps against a list of known and trusted apps.*
*Incorrect: Exploit protection –– Helps protect against malware that uses exploits to infect devices and spread.*

You have a Microsoft 365 E5 subscription that uses Microsoft Defender for Cloud Apps.

You need to be notified if a user signs in from a risky IP address. The solution must ensure that the user is also notified.

Which type of policy should you create?

- a Cloud Discovery anomaly detection policy
- a session policy
- an access policy
- **an activity policy**

*Activity policies allow you to enforce a wide range of automated processes by using the app provider's APIs. These policies enable you to monitor specific activities carried out by various users or follow unexpectedly high rates of a certain type of activity. After you set an activity detection policy, it starts to generate alerts. Alerts are only generated for activities that occur after you create the policy.*

*Access policies enable real-time monitoring and control over access to cloud apps based on user, location, device, and app. You can create access policies for any device, including devices that are not hybrid Microsoft Entra joined and not managed by Microsoft Intune, by rolling out client certificates to managed devices or by using existing certificates, such as third-party MDM certificates.*

*A Cloud Discovery anomaly detection policy enables you to set up and configure continuous monitoring of unusual increases in cloud app usage. Increases in downloaded data, uploaded data, transactions, and users are considered for each cloud app. Each increase is compared to the normal usage pattern of the app, as learned from past usage. The most extreme increases trigger security alerts.*

*Session policies enable real-time, session-level monitoring, affording you granular visibility into cloud apps and the ability to take different actions depending on the policy you set for a user session. Instead of allowing or blocking access completely, with session control, you can allow access while monitoring the session and/or limit specific session activities by using the reverse proxy capabilities of Conditional Access App Control.*
[Learn More](https://learn.microsoft.com/training/modules/microsoft-cloud-app-security/)

You have a Microsoft 365 E5 subscription that uses Microsoft Defender for Cloud Apps.

You need to create a Defender for Cloud Apps policy that will generate alerts based on a trainable classifier.

Which type of policy should you create?

Select only one answer.

- access policy
- activity policy
- **file policy**
- Cloud Discovery anomaly detection policy

*File policies allow you to enforce a wide range of automated processes by using the cloud provider's APIs. Policies can be set to provide continuous compliance scans, legal eDiscovery tasks, data loss prevention (DLP) for sensitive content shared publicly, and many more use cases. Defender for Cloud Apps can monitor any file type based on more than 20 metadata filters, such as access level, file type, and trainable classifiers.*

*Activity policies allow you to enforce a wide range of automated processes by using the app provider's APIs. These policies enable you to monitor specific activities performed by various users or follow unexpectedly high rates of a certain type of activity. After you set an activity policy, it starts to generate alerts. Alerts are only generated on activities that occur after you create the policy.*

*Access policies enable real-time monitoring and control over access to cloud apps based on user, location, device, and app. You can create access policies for any device, including devices that are not hybrid Microsoft Entra-joined and not managed by using Microsoft Intune, by rolling out client certificates to managed devices or by using existing certificates, such as third-party MDM certificates.*

*A Cloud Discovery anomaly detection policy enables you to set up and configure continuous monitoring of unusual increases in cloud app usage. Increases in downloaded data, uploaded data, transactions, and users are considered for each cloud app. Each increase is compared to the normal usage pattern of the app as learned from past usage. The most extreme increases trigger security alerts.*.

Your company uses Microsoft Defender for Cloud.

You need to configure the continuous export of Defender for Cloud data.

To which two destinations can you export the data? Each correct answer presents a complete solution.

Select all answers that apply.

- **a Log Analytics workspace**
- a Microsoft Sentinel workspace
- a storage account
- an Azure Synapse Analytics workspace
- **an event hub**

*Defender for Cloud generates detailed security alerts and recommendations. You can view them in the portal or through programmatic tools. You might also need to export some or all of this information for tracking by using other monitoring tools in your environment. If you want to configure continuous export in Defender for Cloud, you can use two destinations, an event hub and a Log Analytics workspace. Other destinations cannot be used with continuous export.*
[Learn More](https://learn.microsoft.com/azure/defender-for-cloud/continuous-export?tabs=azure-portal)

You have a Microsoft 365 E5 subscription that uses Microsoft Defender for Endpoint.

You deploy an app named App1. App1 clears logs from an application server.

When App1 runs, it triggers a Microsoft Defender alert.

You need to prevent App1 from triggering alerts. The solution must ensure that other apps will still trigger alerts.

What should you configure?

Select only one answer.

- a detection rule
- **a suppression rule**
- an alert policy
- an automated investigation

*You can create suppression rules for specific alerts that are known to be innocuous, such as known tools or processes in your organization. You can control the context for when an alert is suppressed by specifying the alert title, indicator of compromise (IoC), and conditions. After specifying the context, you can configure the action and scope on the alert.*

*Automated investigation and response (AIR) capabilities are designed to examine alerts and take immediate action to resolve breaches. AIR capabilities significantly reduce alert volume, allowing security operations to focus on more sophisticated threats and other high-value initiatives.*

*Alert policies allow you to track user and admin activities, malware threats or data loss incidents within your organization.*

*Custom detection rules are rules that you can design and tweak by using Advanced Hunting queries. These rules let you proactively monitor various events and system states, including suspected breach activity and misconfigured endpoints. You can set them to run at regular intervals, generating alerts and taking response actions whenever there are matches.*

You have a Microsoft Sentinel playbook named Playbook1.

Playbook1 is triggered when a new Microsoft Sentinel incident is created.

You create a custom near-real-time (NRT) analytics rule named NRTRule1.

You need to ensure that NRTRule1 can use Playbook1.

What should you do?

Select only one answer.

- **Add an automation rule to NRTRule1.**
- Configure Alert Automation (Classic) on NRTRule1.
- Configure the Incident settings for NRTRule1.
- Recreate NRTRule1 as a scheduled query rule.

*For playbooks that are triggered by an incident creation and receive incidents as their inputs (their first step is “When a Microsoft Sentinel Incident is triggered”), create an automation rule and define a Run playbook action in the rule to trigger the playbook.*

*Configuring the Incident settings for NRTRule1 does not allow for the addition of a playbook for automation.*

*Recreating the query by using a scheduled query rule is unnecessary as the option is available on the NRT rule.*

*Configuring Alert Automation (Classic) on NRTRule1 is reserved for playbooks that have been configured with the Microsoft Sentinel alert trigger.*[Learn More](https://learn.microsoft.com/training/modules/automation-microsoft-sentinel/)

You have a Microsoft Sentinel workspace that contains the following rules:

A near-real-time (NRT) rule named Rule1
A fusion rule named Rule2
A scheduled rule named Rule3
A machine learning (ML) behavior analytics rule named Rule4
You need to review the query logic used for individual rules.

Which rules can you review?

Select only one answer.

- **Rule1 and Rule3 only**
- Rule1, Rule2, and Rule3 only
- Rule1, Rule2, Rule3, and Rule4
- Rule2 and Rule3 only
- Rule3 only

*Fusion rules use a correlation engine based on scalable ML algorithms to automatically detect multistage attacks. Because they are based on proprietary ML algorithms, you cannot see their logic.NRT rules are designed to run once every minute and capture events ingested in the preceding minute, to supply you with information that is as up-to-the-minute as possible.ML behavior analytics queries are based on proprietary Microsoft ML algorithms, so you cannot see or change the query logic.Because only Rule1 and Rule3 are not based on ML, you can only see the query logic for those two rules.*[Learn More](https://learn.microsoft.com/training/modules/analyze-data-in-sentinel/4-analytics-rules?ns-enrollment-type=learningpath&ns-enrollment-id=learn.wwl.sc-200-create-detections-perform-investigations-azure-sentinel)

You have a Microsoft Sentinel workspace.

You deploy a new security device that is connected to the workspace by using the Common Event Format (CEF) data connector.

You need to develop a custom parser to interpret the data from the device.

What should you do first?

Select only one answer.

- **Collect sample logs.**
- Create a playbook.
- Map the source event fields to the identified schema.
- Switch to the Syslog connector.

*To build effective Advanced Security Information Model (ASIM) parsers, you need a representative set of logs, which, in most cases will require setting up the source system and connecting it to Microsoft Sentinel. If you do not have a source device available, cloud pay-as-you-go services let you deploy many devices for development and testing.*

*Mapping the source event fields to the identified schema is the third step in creating a custom parser. You must first collect sample data.*

*Creating a playbook is not part of creating a custom parser.*

*Switching to the Syslog connector is not part of the solution. Any configured data table that meets the minimum requirements can be used as a custom parser.*

You have a Microsoft 365 E5 subscription that contains five Microsoft SharePoint Online sites.

You apply data loss prevention (DLP) policies to the SharePoint Online sites.

You need to review the DLP alerts.

Which two portals can you use? Each correct answer presents a complete solution.

Select all answers that apply.

- **the Microsoft Purview compliance portal**
- the SharePoint admin center
- the Microsoft 365 admin center
- **the Microsoft Defender portal**

*You create DLP policies in the Microsoft Purview compliance portal. DLP alerts can also be viewed in the same portal by selecting Data loss prevention, and then the Alerts tab.*

*The SharePoint admin center enables you to manage SharePoint sites in your organization, for example, creating new sites.*

*The Microsoft 365 admin center enables you to manage your Microsoft 365 subscription, for example, configuring tenant settings or creating new user accounts.*

*The Microsoft Defender portal combines protection, detection, investigation, and response to email, collaboration, identity, device, and cloud app threats in a central place. This includes the ability to view DLP alerts and incidents.*

*You cannot create a DLP policy or view DLP alerts in the SharePoint admin center or Microsoft 365 admin center.*[Learn more](https://learn.microsoft.com/microsoft-365/compliance/dlp-configure-view-alerts-policies?view=o365-worldwide)

You have a Microsoft 365 E5 subscription that contains a user named User1 and a Microsoft SharePoint Online site named Site1.

User1 leaves the company, and the user’s account is deleted.

You need to identify whether User1 downloaded files from Site1 during the 30 days before the account was deleted.

Which type of policy should you create?

Select only one answer.

- a Microsoft Defender for Cloud Apps file policy
- a Microsoft Defender for Office 365 alert policy
- **a Microsoft Purview insider risk management policy**
- a Microsoft Entra Governance access review

*Insider risk policies identify which users are in-scope and which types of risk indicators are configured for alerts. For example, you can create an insider risk policy that triggers an alert if suspicious activity, such as mass file download, is detected up to 90 days before a user account is deleted.*

*Alert policies let you categorize the alerts that are triggered by a policy, apply the policy to all the users in your organization, set a threshold level for when an alert is triggered, and decide whether to receive email notifications when alerts are triggered.*

*You can use an access review to efficiently manage group memberships, access to enterprise applications, and role assignments. Based on input from the user, the user can be removed automatically from the groups.*

*DLP examines email messages and files for sensitive information, such as a credit card number. By using DLP, you can detect sensitive information and take action, such as log the event for auditing purposes, display a warning to the end user, or prevent access to files that contain sensitive information.*[Learn More](https://learn.microsoft.com/microsoft-365/compliance/insider-risk-management-policies?view=o365-worldwide)

You have a hybrid Microsoft Entra tenant and a Microsoft 365 E5 subscription.

You need to review all changes made to the Active Directory Enterprise Admins group during the last 14 days.

What should you use?

Select only one answer.

- the Microsoft Entra ID Provisioning Analysis workbook
- the Microsoft Entra ID Sensitive Operations Report workbook
- the Microsoft Defender for Cloud Apps Cloud Discovery users page
- **the Microsoft Defender for Identity Modifications to sensitive groups report**

*The Defender for Identity Modifications to sensitive groups report lists every time a modification is made to sensitive groups, such as admin or manually tagged accounts or groups.*

*The Microsoft Entra ID Provisioning Analysis workbook provides information on activities performed by the provisioning service, such as the creation of a group in ServiceNow or a user imported from Workday.*

*The Defender for Cloud Apps Identity Security Posture offers proactive identity security posture assessments to detect and recommend actions across on-premises Active Directory configurations.*

*The Defender for Cloud Apps Cloud Discovery dashboard is designed to give you more insight into how cloud apps are being used in your organization. It provides an at-a-glance overview of which kinds of apps are being used, your open alerts, and the risk levels of apps in your organization.*

*The Microsoft Entra ID Sensitive Operations Report workbook is intended to help identify suspicious application and service principal activity that may indicate compromises in your environment.*

You have a hybrid Microsoft Entra tenant and a Microsoft 365 E5 subscription.

You need to identify any high-risk users that have performed a password reset during the last 14 days.

What should you use?

Select only one answer.

- the Identity Protection risk analysis workbook in Microsoft Entra ID
- the Sensitive Operations Report workbook in Microsoft Entra ID
- the Risky sign-ins report in Microsoft Entra ID Protection
- **the Risky users report in Microsoft Entra ID Protection**

*The Risky users report allows you to review risky sign-ins and flag each one as either safe or compromised based on the results of an investigation and the information provided by the console. The information can be filtered by using the filters across the top of the report. The Risky users report has a filter for risk detail that includes the following setting: User performed secured password reset.*

*The Risky sign-ins report displays a list of user accounts that are considered risky.*

*The Microsoft Entra Identity Protection risk analysis workbook helps you analyze the state of risk in your organization.*

*The Microsoft Entra Sensitive Operations Report workbook is intended to help identify suspicious application and service principal activity that may indicate compromises in your environment.*

You have a hybrid Microsoft 365 E5 subscription.

You need to monitor an Active Directory Domain Services (AD DS) domain and identify and investigate any detected threats.

What should you use?

Select only one answer.

- Microsoft Entra ID Protection
- Microsoft Defender for Endpoints
- **Microsoft Defender for Identity**
- Microsoft Defender for Office 365

*Defender for Identity is a cloud-based security solution that leverages your on-premises Active Directory signals to identify, detect, and investigate advanced threats, compromised identities, and malicious insider actions directed at your organization.*

*Microsoft Entra ID Protection uses the learnings acquired by Microsoft from its position in organizations by using Microsoft Entra ID, the consumer space with Microsoft accounts, and in gaming with Xbox to protect your users. Microsoft analyses trillions of signals per day to identify and protect customers from threats.*

*Defender for Endpoint is an enterprise endpoint security platform designed to help enterprise networks prevent, detect, investigate, and respond to advanced threats.*

*Defender for Office 365 is a cloud-based email filtering service that protects your business from threats to email and collaboration tools.*

You have a Microsoft 365 E5 subscription that uses Microsoft Defender for Cloud.

You have a lab environment that uses an IP address range of 14.38.2.0/24.

You receive multiple alerts from the lab environment.

You need to remove all alerts that originate from the lab environment network. The solution must meet the following requirements:

Automatically close all future alerts from the lab environment.
Minimize administrative effort.
What should you create?

Select only one answer.

- a firewall policy
- a logic app
- **a suppression rule**
- an automation rule

*When a single alert is not interesting or relevant, you can manually dismiss it. Alternatively, use suppression rules to automatically dismiss similar alerts in the future. Typically, you use a suppression rule to:*

*Suppress alerts that you identified as false positives.*

*Suppress alerts that are being triggered too often to be useful.*

*Your suppression rules define the criteria for which alerts should be automatically dismissed.*

*An automation rule cannot be configured for Defender for Cloud alerts.*

*A firewall policy will not prevent the creation of all alerts.*

*The triggers and actions available with the Defender for Cloud connector does not allow for the closure of alerts.*

You have an Azure virtual machine named VM1 that runs Windows Server and is protected by using Microsoft Defender for Cloud.

Defender for Cloud generates multiple alerts for VM1. You review the alerts and establish that the alerts are false positives.

You need to prevent new VM1 alerts from being shown in Defender for Cloud for seven days.

What should you do?

Select only one answer.

- Create a suppression rule.
- Modify the alert processing rule.
- Modify the alert rule.
- Modify the diagnostic settings for VM1.

*When a single alert is not interesting or relevant, you can manually dismiss it. Alternatively, you can use the suppression rules to automatically dismiss similar alerts in the future. Typically, you use a suppression rule to suppress alerts that you identified as false positives or alerts that are being triggered too often to be useful.*

*Alert rules and suppression rules are used by Azure Monitor, not by Defender for Cloud.*

*Diagnostic settings control where resource logs are sent (storage account, Log Analytics workspace, or event hub).*
[Learn More](https://learn.microsoft.com/azure/defender-for-cloud/alerts-suppression-rules)

Your company uses Microsoft Defender for Cloud.

You need to create a Defender for Cloud workflow automation.

What should you create first?

Select only one answer.

- **a logic app**
- a storage account
- an App Service app
- an automation account

*Workflow automation in Defender for Cloud can trigger Logic Apps on security alerts, recommendations, and changes to regulatory compliance. For example, you might want Defender for Cloud to email a specific user when an alert occurs. If you want to create workflow automation, you must configure it with an already existing logic app, which will be triggered by the workflow automation. As such, you must first create a logic app, and only after that, you can create a workflow automation in Defender for Cloud.*
[Learn More](https://learn.microsoft.com/azure/defender-for-cloud/workflow-automation)

You have a Microsoft 365 E5 subscription that uses Microsoft Defender for Endpoint and contains a device named Device1.

Device1 triggered a high-severity alert.

You need to review the events that occurred before the alert was triggered.

What should you do in the Microsoft Defender portal?

Select only one answer.

- From the Device page, select Go hunt.
- **From the Device page, select the Timeline tab.**
- From the Microsoft Defender portal, query for Device1 in Advanced Hunting.

*You can review the events that occurred before the high-severity alert was triggered by selecting the Timeline tab from the Device page.*

*Go hunt is used once an event is selected to hunt for other relevant events in Advanced Hunting, and Advanced Hunting allows you to proactively hunt for threats.*
[Learn More](https://learn.microsoft.com/training/modules/mitigate-incidents-microsoft-365-defender/)
