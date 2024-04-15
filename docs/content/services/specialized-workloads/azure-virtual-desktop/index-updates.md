+++
title = "Azure Virtual Desktop"
description = "Best practices and resiliency recommendations for Azure Virtual Desktop and associated resources and settings."
date = "1/9/24"
author = "yshafner"
msAuthor = "yonahshafner"
draft = false
+++

The presented resiliency recommendations in this guidance include Azure Virtual Desktop and associated resources and settings.

## Summary of Recommendations

{{< table style="table-striped" >}}
| Recommendation | Category | Impact | State | ARG Query Available |
|:---|:---:|:---:|:---:|:---:|
| [IT-2 - Replicate your Image Templates to a secondary region](https://azure.github.io/Azure-Proactive-Resiliency-Library/services/compute/image-templates/#it-2---replicate-your-image-templates-to-a-secondary-region) | Disaster Recovery | Low | Preview | Yes |
| [CG-1 - A minimum of three replicas should be kept for production image versions](https://azure.github.io/Azure-Proactive-Resiliency-Library/services/compute/compute-gallery/#cg-1---a-minimum-of-three-replicas-should-be-kept-for-production-image-versions) | Availability | Medium | Verified | Yes |
| [CG-2 - Zone redundant storage should be used for image versions](https://azure.github.io/Azure-Proactive-Resiliency-Library/services/compute/compute-gallery/#cg-2---zone-redundant-storage-should-be-used-for-image-versions) | Availability | Medium | Verified | Yes |
| [VM-2 - Deploy VMs across Availability Zones](https://azure.github.io/Azure-Proactive-Resiliency-Library/services/compute/virtual-machines/#vm-2---deploy-vms-across-availability-zones) | Availability | High | Verified | Yes |
| [VM-7 - Enable Backups on your VMs](https://azure.github.io/Azure-Proactive-Resiliency-Library/services/compute/virtual-machines/#vm-7---backup-vms-with-azure-backup-service) | Disaster Recovery | Medium | Verified | Yes |
| [VM-8 - Production VMs should be using SSD disks](https://azure.github.io/Azure-Proactive-Resiliency-Library/services/compute/virtual-machines/#vm-8---production-vms-should-be-using-ssd-disks) | System Efficiency | High | Verified | Yes |
| [VM-21 - Configure diagnostic settings for all Azure Virtual Machines](https://azure.github.io/Azure-Proactive-Resiliency-Library/services/compute/virtual-machines/#vm-21---configure-diagnostic-settings-for-all-azure-virtual-machines) | Monitoring | Low | Preview | Yes |
| [ERC-1 - Connect your on-premises network to critical workloads in Azure through two or more ExpressRoute circuits in different peering locations](https://azure.github.io/Azure-Proactive-Resiliency-Library/services/networking/expressroute-circuits/#erc-1---connect-your-on-premises-network-to-critical-workloads-in-azure-through-two-or-more-expressroute-circuits-in-different-peering-locations) | Availability | High | Verified | No |
| [ERC-2 - Ensure the two physical links of your ExpressRoute circuit are connected to two distinct edge devices in your network](https://azure.github.io/Azure-Proactive-Resiliency-Library/services/networking/expressroute-circuits/#erc-2---ensure-the-two-physical-links-of-your-expressroute-circuit-are-connected-to-two-distinct-edge-devices-in-your-network) | Availability | High | Verified | No |
| [VPNG-1 - Choose a Zone-redundant gateway](https://azure.github.io/Azure-Proactive-Resiliency-Library/services/networking/vpn-gateway/#vpng-1---choose-a-zone-redundant-gateway) | Availability | High | Verified | Yes |
| [NSG-4 - Configure NSG Flow Logs](https://azure.github.io/Azure-Proactive-Resiliency-Library/services/networking/network-security-group/#nsg-4---configure-nsg-flow-logs) | Monitoring | Medium | Preview | Yes |
| [ST-1 - Ensure that Storage Account configuration is at least Zone redundant](https://azure.github.io/Azure-Proactive-Resiliency-Library/services/storage/storage-account/#st-1---ensure-that-storage-account-configuration-is-at-least-zone-redundant) | Storage | High | Verified | Yes |
| [KV-3 - Enable Azure Private Link Service for Key vault](https://azure.github.io/Azure-Proactive-Resiliency-Library/services/security/key-vault/#kv-3---enable-azure-private-link-service-for-key-vault) | Networking | High | Verified | Yes |
| [ALA-1 - Configure Service Health Alerts](https://azure.github.io/Azure-Proactive-Resiliency-Library/services/monitoring/service-health-alerts/#ala-1---configure-service-health-alerts) | Monitoring | High | Verified | Yes |
| [ALA-1 - Configure Service Health Alerts](https://azure.github.io/Azure-Proactive-Resiliency-Library/services/monitoring/service-health-alerts/) | Monitoring | Medium | Verified | No |
| [WADS-3 - Ensure that all fault-points and fault-modes are understood and operationalized](https://azure.github.io/Azure-Proactive-Resiliency-Library/well-architected/2-design/#wads-3---ensure-that-all-fault-points-and-fault-modes-are-understood-and-operationalized) | Availability | High | Verified | No |
| [WADS-7 - Design a BCDR strategy that will help to meet the business requirements](https://azure.github.io/Azure-Proactive-Resiliency-Library/well-architected/2-design/#wads-7---design-a-bcdr-strategy-that-will-help-to-meet-the-business-requirements) | Disaster Recovery | High | Verified | No |

{{< /table >}}

{{< alert style="info" >}}

