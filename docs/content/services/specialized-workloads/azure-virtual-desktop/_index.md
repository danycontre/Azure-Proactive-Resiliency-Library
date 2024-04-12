---
title: Azure Virtual Desktop
geekdocCollapseSection: true
geekdocHidden: false
---

## Dependent Azure Resource Recommendations

| Recommendation                                                                                                                                                                                                                                                                    |  Provider Namespace   |     Resource Type      |
| :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :-------------------: | :--------------------: |
| [Monitor Service Health and Resource Health for AVD](../../../Azure-Proactive-Resiliency-Library-v2/azure-resources/DesktopVirtualization/hostPools/#monitor-service-health-and-resource-health-for-avd) | DesktopVirtualization | hostPools |
| [Configure AVD insights Workbook](../../../Azure-Proactive-Resiliency-Library-v2/azure-resources/DesktopVirtualization/hostPools/#pooled-configure-scheduled-agent-updates) | DesktopVirtualization | hostPools |
| [Ensure separate log analytics workspaces for Prod and DR](../../../Azure-Proactive-Resiliency-Library-v2/azure-resources/DesktopVirtualization/hostPools/#pooled-create-a-validation-pool-for-testing-of-planned-updates) | DesktopVirtualization | hostPools |
| [Organize AVD resources using the AVD Scale unit model described by the AVD Landing Zone Methodology](../../../Azure-Proactive-Resiliency-Library-v2/azure-resources/DesktopVirtualization/hostPools/#monitor-service-health-and-resource-health-of-avd) | DesktopVirtualization | hostPools |
| [Capacity Planning for AVD Resources](../../../Azure-Proactive-Resiliency-Library-v2/azure-resources/DesktopVirtualization/hostPools/#scaling-plans-should-be-created-per-region-and-not-scaled-across-regions) | DesktopVirtualization | hostPools |
| [Deploy a pair of Domain Controllers and DNS Servers in Azure Virtual Machines Across Availability Zones in the same region as AVD session hosts](../../../Azure-Proactive-Resiliency-Library-v2/azure-resources/VirtualMachineImages/imageTemplates/) | VirtualMachineImages | imageTemplates |
| [Ensure a unique OU is used when deploying host pools with domain joined session hosts](../../../Azure-Proactive-Resiliency-Library-v2/azure-resources/VirtualMachineImages/imageTemplates/#replicate-your-image-templates-to-a-secondary-regions) | Compute | galleries |
| [Ensure DNS regions are replicated to avoid single point of failure](../../../Azure-Proactive-Resiliency-Library-v2/azure-resources/Compute/virtualMachines/#deploy-vms-across-availability-zones) | Compute | virtualMachines |
| [Implement a Multi-Region BCDR Plan](../../../Azure-Proactive-Resiliency-Library-v2/azure-resources/Compute/virtualMachines/#backup-vms-with-azure-backup-services) | Compute | virtualMachines |
| [Use Azure Site Recovery or Backups on VMs supporting personal desktops](../../../Azure-Proactive-Resiliency-Library-v2/azure-resources/Compute/virtualMachines/#production-vms-should-be-using-ssd-disks) | Compute | virtualMachines |
| [Create only one FSLogix file share per Storage Account](../../../Azure-Proactive-Resiliency-Library-v2/azure-resources/Compute/virtualMachines/#configure-diagnostic-settings-for-all-azure-virtual-machines) | Compute | virtualMachines |
| [Create a dedicated FSLogix file share and setup per host pool](../../../Azure-Proactive-Resiliency-Library-v2/azure-resources/Network/expressRouteCircuits/#connect-on-prem-networks-to-azure-critical-workloads-via-multiple-expressroutes) | Network | expressRouteCircuits |
| [Enable Azure Backup for FSLogix Storage Account](../../..https://azure.github.io/Azure-Proactive-Resiliency-Library-v2/azure-resources/Network/expressRouteCircuits/#ensure-expressroutes-physical-links-connect-to-distinct-network-edge-devices) | Network | expressRouteCircuits |
| [Enable Azure Private Link Service for FSLogix storage account](../../../Azure-Proactive-Resiliency-Library-v2/azure-resources/Network/virtualNetworkGateways/#choose-a-zone-redundant-gateway) | Network | virtualNetworkGateways |
| [Scaling plans should be created per region and not scaled across regions](../../../Azure-Proactive-Resiliency-Library-v2/azure-resources/Network/networkSecurityGroups/#configure-nsg-flow-logs) | Network | networkSecurityGroups  |
| [Implement RDP Shortpath for Public or Managed Networks](../../../Azure-Proactive-Resiliency-Library-v2/azure-resources/Storage/storageAccounts/#ensure-that-storage-accounts-are-zone-or-region-redundant) | Storage | storageAccounts |
| [Validate that the AVD session hosts can communicate with the AVD control plane and that UDP ports are allowed if RDPshortpath is in use](../../../Azure-Proactive-Resiliency-Library-v2/azure-resources/Storage/storageAccounts/#ensure-that-storage-accounts-are-zone-or-region-redundant) | Storage | storageAccounts |
| [Ensure Secondary Entra ID connect synchronization server](../../../Azure-Proactive-Resiliency-Library-v2/azure-resources/Storage/storageAccounts/#ensure-that-storage-accounts-are-zone-or-region-redundant) | Storage | storageAccounts |
| [Ensure virtual networks have route tables/route server configured for all regions](../../../Azure-Proactive-Resiliency-Library-v2/azure-resources/Storage/storageAccounts/#ensure-that-storage-accounts-are-zone-or-region-redundant) | Storage | storageAccounts |
| [Ensure virtual networks isolation with separate IP space and NSGs for Prod and DR](../../../Azure-Proactive-Resiliency-Library-v2/azure-resources/Storage/storageAccounts/#ensure-that-storage-accounts-are-zone-or-region-redundant) | Storage | storageAccounts |
| [Ensure route tables accommodate failover](../../../Azure-Proactive-Resiliency-Library-v2/azure-resources/Storage/storageAccounts/#ensure-that-storage-accounts-are-zone-or-region-redundant) | Storage | storageAccounts |
| [Configure static routes to allow session hosts traffic to AVD control plane to go directly out of the subnet to internet](../../../Azure-Proactive-Resiliency-Library-v2/azure-resources/Storage/storageAccounts/#ensure-that-storage-accounts-are-zone-or-region-redundant) | Storage | storageAccounts |
| [Create updated image version and replace session hosts rather than updating host directly](../../../Azure-Proactive-Resiliency-Library-v2/azure-resources/Storage/storageAccounts/#ensure-that-storage-accounts-are-zone-or-region-redundant) |        Storage        |    storageAccounts     |
| [Create a validation pool for testing of planned updates](../../../Azure-Proactive-Resiliency-Library-v2/azure-resources/Storage/storageAccounts/#ensure-that-storage-accounts-are-zone-or-region-redundant) | Storage | storageAccounts |
| [Configure scheduled agent updates](../../../Azure-Proactive-Resiliency-Library-v2/azure-resources/Storage/storageAccounts/#ensure-that-storage-accounts-are-zone-or-region-redundant) | Storage | storageAccounts |
| [Ensure the standard FSLogix configuration is deployed](../../../Azure-Proactive-Resiliency-Library-v2/azure-resources/Storage/storageAccounts/#ensure-that-storage-accounts-are-zone-or-region-redundant) | Storage | storageAccounts |
| [Ensure user permissions are set correctly on FSLogix SMB shares](../../../Azure-Proactive-Resiliency-Library-v2/azure-resources/Storage/storageAccounts/#ensure-that-storage-accounts-are-zone-or-region-redundant) | Storage | storageAccounts |
| [Configure Diagnostic Settings on FSLogix storage and capture session hosts FSLogix events](../../../Azure-Proactive-Resiliency-Library-v2/azure-resources/Storage/storageAccounts/#ensure-that-storage-accounts-are-zone-or-region-redundant) | Storage | storageAccounts |
| [Manually install FSLogix updates](../../../Azure-Proactive-Resiliency-Library-v2/azure-resources/Storage/storageAccounts/#ensure-that-storage-accounts-are-zone-or-region-redundant) | Storage | storageAccounts |
| [Turn on Continuous Availability for ANF if using App Attach](../../../Azure-Proactive-Resiliency-Library-v2/azure-resources/Storage/storageAccounts/#ensure-that-storage-accounts-are-zone-or-region-redundant) | Storage | storageAccounts |
| [App attach should be placed in separate file share and Disaster recovery plan should include App attach storage](../../../Azure-Proactive-Resiliency-Library-v2/azure-resources/Storage/storageAccounts/#ensure-that-storage-accounts-are-zone-or-region-redundant) | Storage | storageAccounts |
| [Ensure Resilient Deployment of Keyvaults for AVD Host Pools](../../../Azure-Proactive-Resiliency-Library-v2/azure-resources/Storage/storageAccounts/#ensure-that-storage-accounts-are-zone-or-region-redundant) | KeyVault | KeyVault |

<br>

## Dependent Well-Architected Framework - Reliability Recommendations

| Recommendation                                                                                                                                                                                                                      | Reliability Stage |
| :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :---------------: |
| [Ensure that all fault-points and fault-modes are understood and operationalized](../../../Azure-Proactive-Resiliency-Library-v2/azure-waf/design/#ensure-that-all-fault-points-and-fault-modes-are-understood-and-operationalized) |      Design       |
| [Design a BCDR strategy that will help to meet the business requirements](../../../Azure-Proactive-Resiliency-Library-v2/azure-waf/design/#design-a-bcdr-strategy-that-will-help-to-meet-the-business-requirements)                 |      Design       |

<br>

## General Workload Guidance

{{< azure-specialized-workloads-recommendationlist name="azure-specialized-workloads-recommendationlist" >}}
