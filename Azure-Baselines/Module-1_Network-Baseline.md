# Create a Networking baseline

Network connectivity is possible between:

- Resources that are located in Azure
- Between on-premises and Azure-hosted resources
- To and from the internet and Azure.

## Azure networking security recommendations

### Restrict RDP and SSH access from the internet - Level 1

Reach Azure VMs by using Remote Desktop Protocol (RDP) and the Secure Shell (SSH) protocol.

- To manage them remotely
- The protocols are standard in datacenter computing.
- Security risk-  attackers can use brute-force techniques to gain access to Azure VMs
  - Will use your VM as a launching pad.
- Recommended that you disable direct RDP and SSH access from the internet for Azure VMs.
- other options that you can use to access these VMs for remote management:
  - Point-to-site VPN
  - Site-to-site VPN
  - Azure ExpressRoute
  - Azure Bastion Host

1. Sign in to the Azure portal. Search for and select Virtual machines.
2. In the left menu under Settings, select Networking.
3. In the Networking pane, verify that the Inbound port rules tab doesn't have a rule for RDP, for example: port=3389, protocol = TCP, Source = Any or Internet.
4. Verify that the Inbound port rules tab doesn't have a rule for SSH, for example: port=22, protocol = TCP, Source = Any or Internet.

### Restrict SQL Server access from the internet - Level 1.

If a firewall is turned on but isn't correctly configured, attempts to connect to SQL Server might be blocked.

- Access an instance of SQL Server through a firewall: configure the firewall on the computer that is running SQL Server.
- Ensure that no SQL Server databases allow ingress from the internet.
  - Less vulnerable.

1. Sign in to the Azure portal. Search for and select SQL servers.
2. In the menu pane under Security, select Networking.
3. In the Networking pane, on the Public access tab, ensure that a firewall rule exists. Ensure that no rule has a Start IP of 0.0.0.0 and End IP of 0.0.0.0 or another combination that allows access to wider public IP ranges.
4. If you change any settings, select Save.

### Enable Network Watcher - Level 1

Azure Network Watcher feature that gives you information about IP ingress and egress traffic

Written in JSON

- Outbound and inbound flows on a per-rule basis.
- The network interface (NIC) the flow applies to.
- 5-tuple information about the flow: source and destination IP addresses, source and destination ports, and the protocol that was used.
- Whether the traffic was allowed or denied.
- In version 2, throughput information like bytes and packets.

1. Sign in to the Azure portal. Search for and select Network Watcher.
2. Select Network Watcher for your subscription and location.
3. If no NSG flow logs exist for your subscription, create an NSG flow log.

### Set NSG flow log retention period to more than 90 days - Level 2

Network Watcher is automatically enabled in your virtual network's region when it is created or updated.

- Resources arent effected and there is no charge

1. Sign in to the Azure portal. Search for and select Network Watcher.
2. In the left menu under Logs, select NSG flow logs.
3. Select an NSG flow log.
4. Ensure that Retention (days) is greater than 90 days.
5. If you change any settings, in the menu bar, select Save.