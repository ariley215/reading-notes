# Enhane your email protection using Microsoft Defender for Office 365


## Introduction to Email Security in Microsoft 365

### Exchange Online Protection (EOP)

#### Features
- **Phishing Protection**: Filters phishing attempts to protect users from fraudulent emails.
- **Spoofing Defense**: Detects and blocks emails that impersonate trusted brands or individuals.
- **Spam Prevention**: Utilizes advanced algorithms to identify and filter out unwanted bulk email.
- **Bulk Email Management**: Allows administrators to manage the flow of mass marketing emails.
- **Malware Detection**: Scans emails for malicious software and removes potential threats.

### Microsoft Defender for Office 365

#### Advanced Threat Protection
- **Zero-day Attack Protection**: Identifies and neutralizes new malware and viruses that have not been previously reported.
- **Safe Attachments**: Analyzes email attachments in a sandbox environment to detect hidden malware.
- **Safe Links**: Provides time-of-click verification of URLs, protecting users from malicious hyperlinks.

#### Integration with EOP
- Together, EOP and Microsoft Defender for Office 365 form a comprehensive anti-malware pipeline, offering protection against both common and sophisticated threats.

### Licensing Note
- Microsoft Defender for Office 365 is included with Microsoft 365 Enterprise E5 subscriptions or as an add-on for other Microsoft 365 Enterprise plans.

## Module Focus: Microsoft Defender for Office 365 Features

### Safe Attachments
- Automatically scans email attachments for malware.
- Uses a virtual environment to detect malicious behavior.

### Safe Links
- Scans URLs in emails and Office documents at the time of click.
- Blocks access to malicious links and redirects to a warning page.

### Outbound Spam Filters
- Allows setting limits on email sending to prevent abuse.
- Blocks users who exceed sending limits and provides a way to unblock them in the Microsoft Defender portal.

### Reporting and Insights
- Generates detailed reports on attacks targeting the organization.
- Provides data on the effectiveness of the protection measures in place.

## Submission and Analysis

### Submitting for Analysis
- Administrators can submit suspicious emails, URLs, and attachments to Microsoft for further analysis.
- Submissions are tracked in the Microsoft Defender portal.

### Tenant Allow/Block List Management
- Manage false positives by reporting them to Microsoft.
- Adjustments to the Tenant Allow/Block List are made based on these reports.

## Additional Resources

- [Exchange Online Protection Overview](https://docs.microsoft.com/en-us/microsoft-365/security/office-365-security/exchange-online-protection-overview)
- [Microsoft Defender for Office 365 Overview](https://docs.microsoft.com/en-us/microsoft-365/security/office-365-security/defender-for-office-365)
- [Safe Attachments in Microsoft Defender for Office 365](https://docs.microsoft.com/en-us/microsoft-365/security/office-365-security/safe-attachments)
- [Safe Links in Microsoft Defender for Office 365](https://docs.microsoft.com/en-us/microsoft-365/security/office-365-security/safe-links)
- [Outbound Spam Filtering in Microsoft Defender for Office 365](https://docs.microsoft.com/en-us/microsoft-365/security/office-365-security/outbound-spam-controls)


## Climb the Security Ladder: From EOP to Microsoft Defender for Office 365

### Overview of Office 365 Security Services

Office 365 security services progress through three main layers:

1. **Exchange Online Protection (EOP)**: Present in any subscription where Exchange Online mailboxes exist, EOP provides foundational email security against common threats.
2. **Microsoft Defender for Office 365 Plan 1 (Defender for Office P1)**: Builds upon EOP with additional protections against sophisticated email threats like zero-day malware and phishing.
3. **Microsoft Defender for Office 365 Plan 2 (Defender for Office P2)**: Includes all features of P1 with added capabilities for post-breach investigation, threat hunting, response, and education.

### Exchange Online Protection (EOP)

- **Goal**: Prevent and detect broad, volume-based, known attacks.
- **Key Features**:
  - Spam and bulk mail filtering
  - Phishing and malware protection
  - Spoof intelligence and impersonation detection
- **Investigation and Response Tools**:
  - Admin Quarantine
  - Email authentication configuration
  - Allow/Block lists for URLs and files

### Microsoft Defender for Office 365 Plan 1

- **Goal**: Extend EOP protections to cover zero-day malware, phishing, and business email compromise.
- **Enhancements over EOP**:
  - Safe Attachments
  - Safe Links with time-of-click protection
  - Protection for SharePoint Online, Teams, OneDrive for Business
  - Anti-phishing with user and domain impersonation protection
- **Investigation Tools**:
  - Real-time detections tool
  - SIEM integration for alerts and detections
  - URL trace

### Microsoft Defender for Office 365 Plan 2

- **Goal**: Provide comprehensive post-breach investigation, hunting, and response tools.
- **Enhancements over Plan 1**:
  - Threat Explorer for hunting
  - Threat Trackers
  - Campaign views
  - Automated Investigation and Response (AIR)
- **Advanced Features**:
  - Attack simulation training
  - Integration with Microsoft Defender XDR for advanced threat hunting and incident investigation

### End-User Involvement

- **EOP and Plan 1**: Focus on awareness with the Report message Outlook add-in.
- **Plan 2**: Shifts to training with Threat Simulator tool and metrics for the Security Operations Center.

### Quick Reference: Defender for Office 365 Plan 1 vs. Plan 2

| Feature | Defender for Office 365 P1 | Defender for Office 365 P2 |
| ------- | -------------------------- | -------------------------- |
| Safe Attachments | ✅ | ✅ |
| Safe Links | ✅ | ✅ |
| Protection for SharePoint, OneDrive, Teams | ✅ | ✅ |
| Anti-phishing protection | ✅ | ✅ |
| Real-time detections | ✅ | ✅ |
| Threat Trackers | ❌ | ✅ |
| Threat Explorer | ❌ | ✅ |
| Automated investigation and response | ❌ | ✅ |
| Attack simulation training | ❌ | ✅ |
| Advanced hunting (Defender XDR) | ❌ | ✅ |
| Incident investigation (Defender XDR) | ❌ | ✅ |
| Alert investigation (Defender XDR) | ❌ | ✅ |

_Note: Microsoft 365 Defender is now known as Microsoft Defender XDR (Extended Detection and Response)._

### Exam Preparation Tips

- Familiarize yourself with the features and capabilities of EOP, Defender for Office P1, and Defender for Office P2.
- Understand the progression of security services from EOP to Defender for Office 365 P2.
- Learn how to configure and manage email authentication to defend against spoofing.
- Practice using investigation and response tools available in EOP and Microsoft Defender for Office 365.
- Explore the additional detection and response features exclusive to Microsoft Defender for Office 365 P2.

---

Remember to configure email authentication if you have EOP, which is essential for defending against spoofing and is a critical component of the MS-102 exam.

# Expand EOP Protections with Safe Attachments and Safe Links

## Safe Attachments

**Objective**: Protect users from malicious email attachments.

**Process**:

1. **Email Reception**: Email with attachment arrives and EOP initially processes it.
2. **Attachment Analysis**: Safe Attachments strips the attachment from the email and uploads it to Microsoft's secure analysis servers.
3. **Sandbox Execution**:
   - The attachment is opened in a virtual environment (sandbox).
   - The system observes the attachment for suspicious behavior.
4. **Delivery Decision**:
   - If safe, the email and attachment are delivered to the recipient's inbox.
   - If malicious, the attachment is blocked, and the email is sent to Junk Email with a notification to the user.

**Advanced Protection**:

- Utilizes machine learning and AI for detecting emerging threats.
- Security experts manually review suspicious files for new threats.

## Safe Links

**Objective**: Protect users from malicious links in emails and documents.

**Process**:

1. **Link Click**: User clicks on a link within an email or document.
2. **URL Analysis**: Safe Links sends the URL to Microsoft's servers for verification.
3. **Threat Comparison**:
   - Compares against a database of known malicious URLs and domains.
   - If a match is found, the link is blocked with user notification.
4. **Behavioral Analysis**:
   - If not in the database, the URL is tested in a virtual environment for malicious behavior.
5. **Access Decision**:
   - If deemed safe, the user is redirected to the original URL.
   - If deemed malicious, the user is redirected to a warning page.

**URL Rewriting**:

- URLs are rewritten with a unique identifier for tracking.
- Helps in blocking future threats and protecting against zero-day attacks.

## Email Anti-Malware Pipeline Diagram

The following diagram visualizes the process as mail flows through EOP and Microsoft Defender for Office 365's anti-malware pipeline:

![Email Anti-Malware Pipeline Diagram](https://learn.microsoft.com/en-us/training/wwl/examine-microsoft-defender-office-365/media/mail-flow-through-safe-attachments-safe-links-29d54ac0.png)

# Manage Spoof Intelligence in Microsoft 365

## Understanding Spoof Intelligence

Spoof intelligence is a feature within Microsoft Defender for Office 365 that helps organizations differentiate between legitimate and illegitimate email spoofing.

### Legitimate Spoofing Scenarios

#### Internal Domain Spoofing:

- Third-party bulk mail for company polls.
- External company sending marketing emails on your behalf.
- Assistants sending emails for others.
- Internal applications generating email notifications.

#### External Domain Spoofing:

- Mailing list relays.
- External company sending emails for another company (e.g., automated reports, SaaS notifications).

## Using Spoof Intelligence Insight

Spoof intelligence insight allows organizations to identify and manage unauthenticated emails that fail SPF, DKIM, or DMARC checks.

### Accessing Spoof Intelligence Insight

- **URL**: [Microsoft Defender Portal](https://security.microsoft.com)
- **Navigation Path**: Email & Collaboration > Policies & Rules > Threat policies > Tenant Allow/Block Lists

### Roles Required:

- **Modify Policy**: Organization Management or Security Administrator.
- **Read-Only Access**: Global Reader or Security Reader.

### Enabling/Disabling Spoof Intelligence

- **Default State**: Enabled by default.
- **Policy Configuration**: Accessible in anti-phishing policies in EOP and Microsoft Defender for Office 365.

### Viewing Spoof Intelligence Data

- **Insight Mode**: Displays detected spoofed messages in the last 7 days.
- **What If Mode**: Shows potential detections if spoof intelligence were deactivated.

### PowerShell Cmdlet for Extended Data

- **Cmdlet**: `Get-SpoofIntelligenceInsight`
- **Data Range**: Shows data for the last 30 days.

## Managing Spoofed Senders

### Reviewing Spoofed Messages

- **Spoofed User**: Domain of the spoofed sender.
- **Sending Infrastructure**: Domain from PTR record or IP/24 if no PTR exists.
- **Message Count**: Number of spoofed messages received in the last 7 days.
- **Last Seen**: Date of the last spoofed message received.
- **Spoof Type**: Internal (own domain) or External.
- **Action**: Allowed or Blocked by spoof intelligence.

### Filtering and Searching

- **Filter**: By Spoof Type or Action.
- **Search**: By domain or sending infrastructure.

### Detailed View and Actions

- **Override Verdict**: Allow or Block spoofing.
- **Reason for Detection**: Why the message was flagged.
- **Recommended Actions**: Steps to take against the message.
- **Domain Summary**: Information on the spoofed domain.
- **Sender Data**: Information on the sender.
- **Threat Explorer**: Link to detailed sender data.

### Handling Allowed Spoofed Senders

- **Spoofed Sender**: Specific domain/infrastructure pair allowed to spoof.
- **Restrictions**: Only allows spoofing for the specific sender, not for any domain or infrastructure.

## Examples of Spoof Intelligence Management

### Example 1: Legitimate Internal Spoofing

- **Scenario**: A third-party sender uses your domain to send emails.
- **Action**: Use spoof intelligence to allow this sender, preventing false positives.

### Example 2: Legitimate External Spoofing

- **Scenario**: An external SaaS company sends reports on behalf of another company.
- **Action**: Review in spoof intelligence insight and allow if verified as legitimate.

## Spoof Intelligence Best Practices

- Regularly monitor allowed spoofed senders for security.
- Actively manage the Tenant Allow/Block List to prevent unsafe messages.
- Utilize Threat Explorer for in-depth analysis of senders and messages.
- Train staff on identifying and reporting suspected spoofing attempts.

# Configure Outbound Spam Filtering Policies

## Overview

Outbound spam filtering is critical for maintaining the integrity and reputation of an organization's email system. Exchange Online Protection (EOP) checks outbound emails for spam and unusual activity. Outbound spam may indicate a compromised account.

## Outbound Spam Policies

EOP uses outbound spam policies to help prevent spam. Admins can view, edit, and configure the default policy and create custom policies for specific users, groups, or domains.

### Creating Outbound Spam Policies

Outbound spam policies can be configured in the Microsoft Defender portal or using PowerShell.

#### Microsoft Defender Portal

- **URL**: [Microsoft Defender Portal](https://security.microsoft.com)
- **Navigation**: Email & Collaboration > Policies & Rules > Threat policies > Anti-spam

#### Exchange Online PowerShell

- For organizations with Exchange Online mailboxes.
- Use the `*-HostedOutboundSpamFilterPolicy` and `*-HostedOutboundSpamFilterRule` cmdlets.

#### Standalone EOP PowerShell

- For organizations without Exchange Online mailboxes.
- Use the same cmdlets as Exchange Online PowerShell.

### Elements of an Outbound Spam Policy

- **Outbound Spam Filter Policy**: Specifies actions and notifications.
- **Outbound Spam Filter Rule**: Specifies priority and sender filters.

### Default Policy

- The default policy named "Default" applies to all senders and cannot be deleted.

## Creating Custom Outbound Spam Policies in the Microsoft Defender Portal

1. Navigate to Anti-spam policies.
2. Select "Create policy" and choose "Outbound".
3. Provide a unique name and description for the policy.
4. Specify internal senders the policy applies to (users, groups, domains).
5. Configure message limits and actions for exceeding limits.
6. Control automatic email forwarding to external senders.
7. Set up notifications for suspicious outbound email messages.
8. Review settings and select "Create".

## Configure Outbound Spam Policies in Exchange PowerShell

### Step 1: Create an Outbound Spam Filter Policy

```powershell
New-HostedOutboundSpamFilterPolicy -Name "PolicyName" -AdminDisplayName "Comments" <Additional Settings>
```

**Example:**

```powershell
New-HostedOutboundSpamFilterPolicy -Name "Contoso Executives" -RecipientLimitExternalPerHour 400 -RecipientLimitInternalPerHour 800 -RecipientLimitPerDay 800 -ActionWhenThresholdReached BlockUser
```

### Step 2: Create an Outbound Spam Filter Rule

```powershell
New-HostedOutboundSpamFilterRule -Name "RuleName" -HostedOutboundSpamFilterPolicy "PolicyName" <Sender filters> [<Sender filter exceptions>] [-Comments "OptionalComments"]
```

**Example:**

```powershell
New-HostedOutboundSpamFilterRule -Name "Contoso Executives" -HostedOutboundSpamFilterPolicy "Contoso Executives"
```

## Best Practices

- Regularly review and update outbound spam policies.
- Implement stricter settings for sensitive roles or groups within the organization.
- Monitor the effectiveness of policies and adjust as necessary.
- Train users on best practices to prevent account compromise.

