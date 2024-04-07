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
|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------:|:--------:|:-------:|:-------------------:|
| [AVD-1 - Enable Azure Private Link Service for FSLogix storage account](#avd-1---enable-azure-private-link-service-for-fslogix-storage-account) | Access & Security | Medium | Verified | No |
| [AVD-2 - Deploy a pair of Domain Controllers and DNS Servers in Azure Virtual Machines Across Availability Zones in the same region as AVD session hosts](#avd-2---deploy-domain-controllers-and-dns-servers-in-azure-virtual-machines-across-availability-zones) | Availability | High | Verified | No |
| [AVD-3 - Implement RDP Shortpath for Public or Managed Networks](#avd-3---implement-rdp-shortpath-for-public-or-managed-networks) | Networking | Medium | Verified | No |
| [AVD-4 - Implement a Multi-Region BCDR Plan](#avd-4---implement-a-multi-region-bcdr-plan) | Disaster Recovery | Medium | Verified | No |
| [AVD-5 - Capacity Planning for AVD Resources](#avd-5---capacity-planning-for-avd-resources) | Disaster Recovery | Low | Verified | No |
| [AVD-6 - Create only one FSLogix file share per Storage Account](#avd-6---create-only-one-fslogix-file-share-per-storage-account) | Storage | Medium | Verified | No |
| [AVD-7 - Create one FSLogix file share per host pool](#avd-7---create-one-fslogix-file-share-per-host-pool) | Storage | Medium | Verified | No |
| [AVD-8 - Enable Azure Backup for FSLogix Storage Account](#avd-8---enable-azure-backup-for-fslogix-storage-account) | Storage | Medium | Verified | No |
| [AVD-9 - Scaling plans should be created per region and not scaled across regions](#avd-9---scaling-plans-should-be-created-per-region-and-not-scaled-across-regions) | Disaster Recovery | Medium | Verified | No |
| [AVD-10 - Validate that the AVD session hosts can communicate with the AVD control plane and UDP ports are open if in use](#avd-10---validate-avd-session-host-connectivity-to-the-avd-control-plane-and-udp-ports-open-if-in-use) | Networking | Medium | Verified | No |
| [AVD-11 - Ensure Secondary Entra ID connect synchronization server](#avd-11---ensure-secondary-entra-id-connect-synchronization-server) | Access & Security | Low | Verified | No |
| [AVD-12 - Ensure DNS regions are replicated to avoid single point of failure](#avd-12---ensure-dns-regions-are-replicated-to-avoid-single-point-of-failure) | Networking | Medium | Verified | No |
| [AVD-13 - Create updated image version and replace session hosts rather than updating host directly](#avd-13---create-updated-image-version-and-replace-session-hosts-rather-than-updating-host-directly) | Governance | Low | Verified | No |
| [AVD-14 - Create a validation pool for testing of planned updates](#avd-14---pooled-create-a-validation-pool-for-testing-of-planned-updates) | Governance | Medium | Verified | No |
| [AVD-15 - Configure scheduled agent updates](#avd-15---pooled-configure-scheduled-agent-updates) | System Efficiency | Medium | Verified | No |
| [AVD-16 - Use Azure Site Recovery or Backups on VMs supporting personal desktops](#avd-16---use-azure-site-recovery-or-backups-on-vms-supporting-personal-desktops) | Disaster Recovery | Medium | Verified | No |
| [AVD-17 - Ensure a unique OU is used when deploying host pools with domain joined session hosts](#avd-17---ensure-a-unique-ou-is-used-when-deploying-host-pools-with-domain-joined-session-hosts) | Governance | Medium | Verified | No |
| [AVD-18 - Ensure the standard FSLogix configuration is deployed](#avd-18---ensure-the-standard-fslogix-configuration-is-deployed) | Storage | Medium | Verified | No |
| [AVD-19 - Ensure user permissions are set correctly on FSLogix SMB shares](#avd-19---ensure-user-permissions-are-set-correctly-on-fslogix-smb-shares) | Storage | High | Verified | No |
| [AVD-20 - Configure Diagnostic Settings on FSLogix storage and capture session hosts FSLogix events.](#avd-20---configure-diagnostic-settings-on-fslogix-storage-and-capture-session-hosts-fslogix-events) | Storage | Medium | Verified | No |
| [AVD-21 - Manually install FSLogix updates](#avd-21---manually-install-fslogix-updates) | Availability | High | Verified | No |
| [AVD-22 - Turn on Continuous Availability for ANF if using App Attach](#avd-22---turn-on-continuous-availability-for-anf-if-using-app-attach) | App Attach Storage | Medium | Verified | No |
| [AVD-23 - App attach should be placed in separate file share and Disaster recovery plan should include App attach storage](#avd-23---app-attach-should-be-placed-in-separate-file-share-and-disaster-recovery-plan-should-include-app-attach-storage) | Storage | Medium | Verified | No |
| [AVD-24 - Ensure virtual networks have route tables/route server configured for all regions](#avd-24---ensure-virtual-networks-have-route-tablesroute-server-configured-for-all-regions) | Networking | Medium | Verified | No |
| [AVD-25 - Ensure virtual networks isolation with separate IP space and NSGs for Prod and DR](#avd-25---ensure-virtual-networks-isolation-with-separate-ip-space-and-nsgs-for-prod-and-dr) | Networking | Medium | Verified | No |
| [AVD-26 - Ensure route tables accommodate failover](#avd-26---ensure-route-tables-accommodate-failover) | Disaster Recovery | Medium | Verified | No |
| [AVD-27 - Configure routes to directly connect from the subnet to the AVD control plane](#avd-27---configure-routes-to-allow-session-host-to-control-plane-traffic-to-go-directly-out-of-the-subnet) | Disaster Recovery | Medium | Verified | No |
| [AVD-28 - Ensure Resilient Deployment of Keyvault for AVD Host Pools](#avd-28---provision-secondary-key-vault-for-disaster-recovery) | Disaster Recovery | High | Verified | No |
| [AVD-29 - Configure AVD insights Workbook](#avd-29---configure-avd-insights-workbook) | Monitoring | High | Verified | No |
| [AVD-30 - Ensure separate log analytics workspaces for Prod and DR](#avd-30---ensure-separate-log-analytics-workspaces-for-prod-and-dr) | Disaster Recovery | Low | Verified | No |
| [AVD-31 - Organize AVD resources using the AVD Scale unit model described by the AVD Landing Zone Methodology](#avd-31---organize-avd-resources-using-the-avd-scale-unit-model-described-by-the-avd-landing-zone-methodology) | Governance | Low | Verified | No |
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

Definitions of states can be found [here]({{< ref "../../../_index.md#definitions-of-terms-used-in-aprl">}})

{{< /alert >}}

## Recommendations Details

### AVD-1 - Enable Azure Private Link Service for FSLogix storage account

**Category: Access & Security**

**Impact: Medium**

**Guidance**

Azure Private Link Service enables you to access Azure Storage Account and Azure hosted customer/partner services over a Private Endpoint in your virtual network. An Azure Private Endpoint is a network interface that connects you privately and securely to a service powered by Azure Private Link. The private endpoint uses a private IP address from your VNet, effectively bringing the service into your VNet. All traffic to the service can be routed through the private endpoint, so no gateways, NAT devices, ExpressRoute or VPN connections, or public IP addresses are needed. Traffic between your virtual network and the service traverses over the Microsoft backbone network, eliminating exposure from the public Internet. You can connect to an instance of an Azure resource, giving you the highest level of granularity in access control.

**Resources**

- [Learn More](https://learn.microsoft.com/azure/storage/files/storage-files-networking-endpoints?tabs=azure-portal)
- [Private link](https://learn.microsoft.com/azure/storage/common/storage-private-endpoints)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-1/avd-1.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-2 - Deploy a pair of Domain Controllers and DNS Servers in Azure Virtual Machines Across Availability Zones in the same region as AVD session hosts

**Category: Availability**

**Impact: High**

**Guidance**

When using an AD DS identity solution with AVD, ensure each region with session hosts has multiple domain controllers and DNS servers on Azure virtual machines distributed across availability zones. This improves the environment’s reliability by removing a dependency on on-premises services and also mitigates dependency on ER/VPN/Inter-Azure dependencies while improving performance, because there is a shorter path for user authentication.

This recommendation is not relevant when you are utilizing Microsoft Entra as the identity provider.

**Resources**

- [Learn More](https://learn.microsoft.com/azure/architecture/example-scenario/identity/adds-extend-domain#reliability)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-2/avd-2.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-3 - Implement RDP Shortpath for Public or Managed Networks

**Category: Networking**

**Impact: Medium**

**Guidance**

It is recommended to enable RDP Shortpath for AVD. RDP Shortpath is a feature of Azure Virtual Desktop that establishes a direct UDP-based transport between a supported Windows Remote Desktop client and session host. By default, Remote Desktop Protocol (RDP) tries to establish connection using UDP and uses a TCP-based reverse connect transport as a fallback connection mechanism. TCP-based reverse connect transport provides the best compatibility with various networking configurations and has a high success rate for establishing RDP connections. UDP-based transport offers better connection reliability and more consistent latency.

**Resources**

- [Learn More](https://learn.microsoft.com/azure/virtual-desktop/rdp-shortpath?tabs=managed-networks)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-3/avd-3.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-4 - Implement a Multi-Region BCDR Plan

**Category: Disaster Recovery**

**Impact: Medium**

**Guidance**

It is recommended to adopt a multi-region deployment (active-active or active-passive) for AVD. Each region should contain at least identity, name resolution, AVD management resources, and session hosts in case of a primary region outage.

**Resources**

- [Multi-region BCDR](https://learn.microsoft.com/azure/architecture/example-scenario/wvd/azure-virtual-desktop-multi-region-bcdr)
- [Learn More](https://learn.microsoft.com/azure/well-architected/azure-virtual-desktop/business-continuity#active-active-scenarios)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-4/avd-4.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-5 - Capacity Planning for AVD Resources

**Category: Disaster Recovery**

**Impact: Low**

**Guidance**

Monitor and plan for subscription limits and API throttling limits. Closely monitor your Azure Virtual Desktop deployments, and keep track of resource usage within your subscription. By proactively monitoring capacity, you can identify potential challenges early on, and you can take suitable actions to avoid reaching limits.
Consider scaling across multiple subscriptions if further scaling is required, or work with Azure support to adjust limits based on your business requirements.
To handle a large number of users, consider scaling horizontally by creating multiple host pools.

**Resources**

- [Capacity Planning](https://learn.microsoft.com/azure/well-architected/azure-virtual-desktop/business-continuity#capacity-planning)
- [Learn More](https://learn.microsoft.com/azure/architecture/example-scenario/wvd/windows-virtual-desktop#azure-virtual-desktop-limitations)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-5/avd-5.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-6 - Create only one FSLogix file share per Storage Account

### AVD-7 - Create a dedicated FSLogix file share and setup per host pool

### AVD-8 - Enable Azure Backup for FSLogix Storage Account

**Category: Storage**

**Impact: Medium**

**Guidance**

It is recommended to enable backup on the FSLogix Storage Account. Ensuring the user profiles are resilient will allow user data and experience to be consistent through outages.

**Resources**

- [FSLogix](https://learn.microsoft.com/fslogix/overview-what-is-fslogix)
- [Backup Storage Account](https://learn.microsoft.com/azure/backup/blob-backup-configure-manage?tabs=operational-backup)

**Resource Graph Query**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-8/avd-8.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-9 - Scaling plans should be created per region and not scaled across regions

**Category: Disaster Recovery**

**Impact: Medium**

**Guidance:**
Each region has its own scaling plans assigned to host pools within that region. However, these plans can become inaccessible if there's a regional failure. To mitigate this risk, it's advisable to create a secondary scaling plan in another region.

**Resources:**

- [Learn More](https://learn.microsoft.com/azure/virtual-desktop/autoscale-scaling-plan?tabs=portal)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-9/avd-9.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-10 - Validate that the AVD session hosts can communicate with the AVD control plane and UDP ports are open if in use

**Category: Networking**

**Impact: Medium**

**Guidance:**
Ensure that AVD session hosts can effectively communicate with the AVD control plane and that UDP ports are open if UDP is utilized. Validate the connectivity of VMs to the AVD Control Plane and confirm the accessibility of UDP TURN ports. Whitelist global URLs and ensure that UDP/TURN ports are open and accessible to facilitate smooth user connections. Proper connectivity validation guarantees optimal performance and user experience within the AVD environment.

**Resources:**

- [Learn More](https://learn.microsoft.com/azure/virtual-desktop/troubleshoot-rdp-shortpath)
- [Learn More](https://learn.microsoft.com/azure/virtual-desktop/check-access-validate-required-fqdn-endpoint)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-10/avd-10.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-11 - Ensure Secondary Entra ID connect synchronization server

**Category: Access & Security**

**Impact: Low**

**Guidance:**
Hybrid - Entra ID Connect best to run in Azure but can be hosted on-prem. Secondary or more VMs should be setup in staging mode in event of failover.
Set up secondary server in staging mode for Entra Connect for syncing to Entra in case of primary server outage.

**Resources:**

- [Learn More](https://learn.microsoft.com/entra/identity/hybrid/connect/how-to-connect-install-multiple-domains)

**Resource Graph Query/Scripts**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-11/avd-11.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-12 - Ensure DNS regions are replicated to avoid single point of failure

**Category: Networking**

**Impact: Medium**

**Guidance:**
Active Directory Domain Services (AD DS) integrated DNS/other should target Secondary/Tertiary customer DNS across multi-region zones. If using custom DNS, ensure there are redundant DNS servers to avoid a single point of failure.

**Resources:**

- [Learn More](https://learn.microsoft.com/azure/architecture/example-scenario/azure-virtual-desktop/azure-virtual-desktop-multi-region-bcdr)

**Resource Graph Query/Scripts:**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-12/avd-12.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-13 - Create updated image version and replace session hosts rather than updating host directly

**Category: Governance**

**Impact: Low**

**Guidance:**
Establish a systematic process for handling image updates within your Azure Virtual Desktop environment. Instead of directly updating individual session hosts, create a new version of the updated image. This process involves creating and configuring a golden image with the necessary updates and configurations. Once the new image is prepared, replace existing session hosts with instances using the updated image. This approach ensures consistency across all session hosts and minimizes the risk of configuration drift. Additionally, it enables quick rollback to a previous image version in case of any issues with the update. Implementing this process helps streamline maintenance activities and ensures that all session hosts are up-to-date with the latest configurations and updates.
has context menu

**Resources:**

- [Learn More](https://learn.microsoft.com/training/modules/create-manage-session-host-image/)

**Resource Graph Query/Scripts:**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-13/avd-13.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-14 - Create a validation pool for testing of planned 

**Category: Governance**

**Impact: Medium**

**Guidance:**
At least one Validation Pool to have early warning if a planned update to AVD causes an issue. support to adjust limits based on your business requirements. To handle a large number of users, consider scaling horizontally by creating multiple host pools.
Also check that the host pool has been used regularly to test planned updates.
Host pools are a collection of one or more identical virtual machines within Azure Virtual Desktop environment. We highly recommend you create a validation host pool where service updates are applied first. Validation host pools let you monitor service updates before the service applies them to your standard or non-validation environment. Without a validation host pool, you may not discover changes that introduce errors, which could result in downtime for users in your standard environment.
To ensure your apps work with the latest updates, the validation host pool should be as similar to host pools in your non-validation environment as possible. Users should connect as frequently to the validation host pool as they do to the standard host pool. If you have automated testing on your host pool, you should include automated testing on the validation host pool.

**Resources:**

- [Learn More](https://learn.microsoft.com/azure/virtual-desktop/configure-validation-environment?tabs=azure-portal)

**Resource Graph Query/Scripts:**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-14/avd-14.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-15 - Configure scheduled agent updates

**Category: System Efficiency**

**Impact: Medium**

**Guidance:**
Ensure schedules have been created to provide maintenance windows for AVD agent updates.
The Scheduled Agent Updates feature lets you create up to two maintenance windows for the Azure Virtual Desktop agent, side-by-side stack, and Geneva Monitoring agent to get updated so that updates don't happen during peak business hours.

**Resources:**

- [Learn More](https://learn.microsoft.com/azure/virtual-desktop/scheduled-agent-updates)

**Resource Graph Query/Scripts:**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-15/avd-15.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-16 - Use Azure Site Recovery or Backups on VMs supporting personal desktops

**Category: Disaster Recovery**

**Impact: Medium**

**Guidance:**
Leverage Azure Site Recovery (ASR) or implement Azure Backup for personal host pools for seamless failover and failback capabilities, enabling the replication of VMs supporting personal desktops to a secondary Azure region. In the event of a disaster or unexpected outage, this ensures the recovery of these VMs from a known-state.

**Resources:**

- [Learn More](https://learn.microsoft.com/azure/virtual-desktop/scheduled-agent-updates)

**Resource Graph Query/Scripts:**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-16/avd-16.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-17 - Ensure a unique OU is used when deploying host pools with domain joined session hosts

**Category: Governance**

**Impact: Medium**

**Guidance:**
Hybrid VMs should be in a unique OU. When using AD-joined, session hosts will benefit from using a unique OU to target specific AVD configurations per hostpool. Examples include Fslogix, time out limits, session controls, and much more. It’s also important to segment Prod and DR organization units to ensure resources are configured per environment.

**Resources:**

- [Learn More](https://learn.microsoft.com/windows-server/identity/ad-ds/deploy/virtual-dc/adds-on-azure-vm#configure-the-vms-and-install-active-directory-domain-services)

**Resource Graph Query/Scripts:**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-17/avd-17.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-18 - Ensure the standard FSLogix configuration is deployed

**Category: Storage**

**Impact: High**

**Guidance:**
Ensure all session hosts have the standard FSLogix configuration deployed. Regularly validate settings for consistency and alignment with best practices.

**Resources:**

- [Learn More](https://learn.microsoft.com/fslogix/reference-configuration-settings?tabs=profiles)

**Resource Graph Query/Scripts:**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-18/avd-18.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-19 - Ensure user permissions are set correctly on FSLogix SMB shares

**Category: Storage**

**Impact: High**

**Guidance:**
Verify user permissions are correctly set on SMB shares so that users have appropriate access to only their own profile and not other user profiles, while administrators have full access at the root volume. Also ensure secondary storage path permissions are set in case of a DR event.

**Resources:**

- [Learn More](https://learn.microsoft.com/fslogix/how-to-configure-storage-permissions)

**Resource Graph Query/Scripts:**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-19/avd-19.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-20 - Configure Diagnostic Settings on FSLogix storage and capture session hosts FSLogix events

**Category: Storage**

**Impact: Medium**

**Guidance:**
Configure diagnostic settings on FSLogix storage resources and regularly its metrics and FSLogix logs for errors. Events can be reviewed by looking locally inside the Session Host, but it is recommended to configure AVD insights workbook to consolidate this information to a Log Analytics workspace.

**Resources:**

- [Learn More](https://learn.microsoft.com/fslogix/troubleshooting-events-logs-diagnostics)
- [Learn More](https://learn.microsoft.com/azure/storage/files/storage-files-monitoring)

**Resource Graph Query/Scripts:**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-20/avd-20.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-21 - Manually install FSLogix updates

**Category: Governance**

**Impact: Low**

**Guidance:**
Ensure a process is in place to regularly check for FSLogix agent upgrades and maintain FSLogix up to date. We recommend customers upgrade to the latest version of FSLogix as quickly as their deployment process can allow. FSLogix will provide hotfix releases which address current and potential bugs that impact customer deployments. Additionally, it is the first requirement when opening any support case.

**Resources:**

- [Learn More](https://learn.microsoft.com/fslogix/how-to-install-fslogix)

**Resource Graph Query/Scripts:**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-21/avd-21.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-22 - Turn on Continuous Availability for ANF if using App Attach

**Category: Availability**

**Impact: Medium**

**Guidance**

Turn on Continuous Availability if using Azure Netapp Files.

Verify the number of users connecting to each file share to make sure the SMB path can handle the number of file connections. Currently, Azure Files supports up to 10k handles per root directory.

**Resources**

- [Learn More](https://learn.microsoft.com/azure/virtual-desktop/app-attach-overview?pivots=msix-app-attach)

**Resource Graph Query/Scripts:**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-22/avd-22.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-23 - App attach should be placed in separate file share and Disaster recovery plan should include App attach storage

**Category: Storage**

**Impact: Medium**

**Guidance**

App Attach packages should be on a separate share from profiles. And App Attach files should be backed up.

Best practice is to separate App Attach VHD files in a separate file share away from user profiles, both for performance and scalability purposes. Requirements can vary greatly depending on how many packaged applications are stored in an image, and you need to test your applications to understand your requirements.

Your file share should be in the same Azure region as your session hosts.

**Resources**

- [Learn More](https://learn.microsoft.com/azure/virtual-desktop/app-attach-overview?pivots=msix-app-attach)

**Resource Graph Query/Scripts:**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-23/avd-23.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-24 - Ensure virtual networks have route tables/route server configured for all regions

**Category: Networking**

**Impact: Medium**

**Guidance**

For high availability connections back to on-premises datacenters should consider backup paths across the regions that have been utilized. Ensure redundancy in routing by having a secondary route table in the secondary region.

**Resources**

- [Learn More](https://learn.microsoft.com/azure/expressroute/designing-for-disaster-recovery-with-expressroute-privatepeering#need-for-redundant-connectivity-solution)

**Resource Graph Query/Scripts:**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-24/avd-24.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-25 - Ensure virtual networks isolation with separate IP space and NSGs for Prod and DR

**Category: Networking**

**Impact: Medium**

**Guidance**

NSG and ASG per AVD persona and IP space per Prod/DR regions.

It's important your organization plans for IP addressing in Azure. Planning ensures the IP address space doesn't overlap across on-premises locations and Azure regions. Overlapping IP address spaces across on-premises and Azure regions create major contention challenges.

**Resources**

- [Learn More](https://learn.microsoft.com/azure/cloud-adoption-framework/ready/azure-best-practices/plan-for-ip-addressing)

**Resource Graph Query/Scripts:**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-25/avd-25.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-26 - Ensure route tables accommodate failover

**Category: Disaster Recovery**

**Impact: Medium**

**Guidance**

Ensure Route Tables that force tunnel traffic to FW/NVA have failover considerations evaluated and won't fail or trigger next-gen FW protections.

AVD workload teams should collaborate with centralized teams that manage the shared infrastructure, like networking, to ensure that both Production and DR workloads have the appropriate route tables in place for failover of routing to perform as expected.

**Resources**

- [Learn More](https://learn.microsoft.com/azure/cloud-adoption-framework/ready/landing-zone/design-area/management-business-continuity-disaster-recovery)

**Resource Graph Query/Scripts:**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-26/avd-26.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-27 - Configure routes to directly connect from the subnet to the AVD control plane

### AVD-28 - Ensure Resilient Deployment of Keyvault for AVD Host Pools

**Category: Disaster Recovery**

**Impact: High**

**Guidance:**
To ensure continuous availability and disaster recovery readiness, it is recommended to provision a secondary Key Vault in a secondary region. In the event of a primary region failure, this secondary Key Vault will ensure that critical secrets are accessible for use in deployments in the secondary region.

**Resources:**

- [Learn More](https://learn.microsoft.com/azure/key-vault/general/disaster-recovery-guidance)

**Resource Graph Query/Scripts:**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-28/avd-28.kql" >}} {{< /code >}}

{{< /collapse >}}

### AVD-29 - Configure AVD insights Workbook

**Category: Monitoring**

**Impact: High**

**Guidance**

AVD Insights is an Azure Workbook template provided by the AVD product team. It is highly recommended in order to monitor and troubleshoot AVD workloads across metrics, logs, events, and more. Both Production and DR workloads should be enabled with AVD Insights.

**Resources**

- [Learn More](https://learn.microsoft.com/azure/virtual-desktop/insights?tabs=monitor)

**Resource Graph Query/Scripts:**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-29/avd-29.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-30 - Ensure separate log analytics workspaces for Prod and DR

**Category: Disaster Recovery**

**Impact: Low**

**Guidance**

Having separate Log Analytics ensures that your DR environment is fully operational for visibility of the metrics, performance, and other auditing tools your workload teams will rely on in the event of an incident.

**Resources**

- [Learn More](https://learn.microsoft.com/azure/virtual-desktop/diagnostics-log-analytics)

**Resource Graph Query/Scripts:**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-30/avd-30.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>

### AVD-31 - Organize AVD resources using the AVD Scale unit model described by the AVD Landing Zone Methodology

**Category: Governance**

**Impact: Low**

**Guidance**

Follow AVD Landing Zone best practices using multiple resource groups based on resource type and associated shared resources for AVD workloads.

**Resources**

- [Learn More](https://learn.microsoft.com/azure/cloud-adoption-framework/scenarios/azure-virtual-desktop/enterprise-scale-landing-zone)

**Resource Graph Query/Scripts:**

{{< collapse title="Show/Hide Query/Script" >}}

{{< code lang="sql" file="code/avd-31/avd-31.kql" >}} {{< /code >}}

{{< /collapse >}}

<br><br>
