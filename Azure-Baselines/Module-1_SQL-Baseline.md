# Create an Azure SQL Database baseline

Azure SQL DB provides an easy transition from an on-premises database to a cloud-based database that has built-in diagnostics, redundancy, security, and scalability.

## Enable auditing - Level 1

Auditing:

- Helps you maintain regulatory compliance, understand database activity, and gain insight into discrepancies and anomalies that might alert you to business concerns or suspected security violations.
- Enables and facilitates adherence to compliance standards
  - Doesn't guarantee compliance.

1. Sign in to the Azure portal. Search for and select SQL databases.
2. In the left menu under Security, select Auditing.
3. In the Auditing pane, enable Enable Azure SQL Auditing, and then select at least one audit log destination.
4. If you change any settings, in the menu bar, select Save.

## Enable SQL protections in Microsoft Defender for Cloud - Level 1

Identfies these harmful DB acess:

- Potential SQL injection.
- Access from an unusual location or datacenter.
- Access from an unfamiliar principal or from a potentially harmful application.
- Brute-force SQL credentials.

1. Sign in to the Azure portal. Search for and select Microsoft Defender for Cloud.
2. In the left menu under Management, select Environment settings.
3. In the Defender plans pane, under Microsoft Defender for, set Azure SQL Databases to On.
4. Return to Azure Home. Search for and select SQL databases.
5. For each database instance, in the left menu under Security, select Microsoft Defender for Cloud.
6. View security recommendations, alerts, and vulnerability assessment findings for your SQL Database instance.

## Configure audit retention for more than 90 days - Level 1

Need to be kept to meet legal and regulatory compliance requirements.

1. Sign in to the Azure portal. Search for and select SQL databases.
2. In the left menu under Security, select Auditing.
3. Select your Audit log destination, and then expand Advanced properties.
4. Ensure that Retention (Days) is greater than 90 days.
5. If you change any settings, in the menu bar, select Save.
