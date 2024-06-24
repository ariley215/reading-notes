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
  - Less vulnerable 