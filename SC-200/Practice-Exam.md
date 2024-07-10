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