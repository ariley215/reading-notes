# Deploy secure infrastructures by using a landing zone

## Introduction to Azure Landing Zones

- **Definition**: An Azure landing zone is a set of guidelines and resources that provide a scalable and secure environment for deploying applications and services in Azure.

- **Purpose**: Facilitates application migration, modernization, and innovation at scale.

- **Components**: Consists of platform landing zones and application landing zones.

## Platform Landing Zones vs. Application Landing Zones

- **Platform Landing Zone**:
  - Subscription providing shared services (identity, connectivity, management).
  - Managed by central teams.
  - Improves operational efficiency.

- **Application Landing Zone**:
  - Subscription dedicated to hosting applications.
  - Pre-provisioned through code.
  - Managed by management groups for policy controls.

## Management Approaches for Application Landing Zones

| Management Approach        | Description |
| -------------------------- | ----------- |
| Central Team Management    | Central IT operates the zone, applying controls to both platform and application zones. |
| Application Team Management| Platform team delegates the zone to an application team, with governance by management group policies. |
| Shared Management          | Central IT manages underlying service; application teams manage the applications on top. |

## Azure Landing Zone Accelerators

- **Purpose**: Provide infrastructure-as-code implementations for correct Azure landing zone deployment.

- **Platform Landing Zone Accelerator**:
  - Azure landing zone portal accelerator.
  - Deploys conceptual architecture and applies configurations to components like management groups and policies.
  - Best for environments managed with the Azure portal.

## Building Scalable, Modular Azure Landing Zones

- **Cloud Adoption Framework**: Includes Secure methodology and provides a starting point for cloud adoption.

- **Azure Landing Zone**:
  - Offers automated implementation of architectures and operating environments.
  - Integrates security best practices.
  - Accelerates cloud adoption with security and governance.

- **Reference Architecture**: Use as a target end-state for mature and scaled-out design considerations.

## Customization and Code

- Azure landing zones can be customized to meet unique business requirements.
- Provide IT and security teams with a repeatable, predictable method for deployment.
- Support security, management, governance, platform automation, and DevOps.

![Azure Landing Zones](https://learn.microsoft.com/en-us/training/wwl-azure/governance-security/media/azure-landing-zone-architecture-example-ce4b31c9.png)
## Zero Trust Principles

- Adapt Azure landing zones based on Azure Security Benchmark (ASB) best practices and Zero Trust principles.
- Implement an end-to-end strategy across identities, endpoints, network, data, applications, and infrastructure.

## Azure Security Benchmark Recommendations

- Follow the high-impact security recommendations of the ASB.
- Integrate ASB recommendations into architectural strategy.
- ASB policy is assigned by default to ensure ASB compliance monitoring.

## Additional Resources

- [Azure Landing Zones Documentation](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/ready/landing-zone/)
- [Azure Security Benchmark](https://docs.microsoft.com/en-us/security/benchmark/azure/)
- [Cloud Adoption Framework](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/)
- [Zero Trust](https://docs.microsoft.com/en-us/security/zero-trust/)

## Diagram Reference

- Example of an Azure landing zone hierarchy for multiple tenants (not included in markdown format).

---

**Note**: Ensure to review all Azure landing zone documentation and customize the architecture to meet the specific needs of your organization. Use Azure landing zones to implement security and governance best practices, whether incrementally or all at once.