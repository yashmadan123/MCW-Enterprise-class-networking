![Microsoft Cloud Workshops](https://github.com/Microsoft/MCW-Template-Cloud-Workshop/raw/main/Media/ms-cloud-workshop.png "Microsoft Cloud Workshops")

<div class="MCWHeader1">
Enterprise-class networking in Azure
</div>

<div class="MCWHeader2">
Hands-on lab step-by-step
</div>

<div class="MCWHeader3">
December 2022
</div>

Information in this document, including URL and other Internet Web site references, is subject to change without notice. Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place or event is intended or should be inferred. Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

The names of manufacturers, products, or URLs are provided for informational purposes only and Microsoft makes no representations and warranties, either expressed, implied, or statutory, regarding these manufacturers or the use of the products with any Microsoft technologies. The inclusion of a manufacturer or product does not imply endorsement of Microsoft of the manufacturer or product. Links may be provided to third party sites. Such sites are not under the control of Microsoft and Microsoft is not responsible for the contents of any linked site or any link contained in a linked site, or any changes or updates to such sites. Microsoft is not responsible for webcasting or any other form of transmission received from any linked site. Microsoft is providing these links to you only as a convenience, and the inclusion of any link does not imply endorsement of Microsoft of the site or the products contained therein.

© 2022 Microsoft Corporation. All rights reserved.

Microsoft and the trademarks listed at <https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/Usage/General.aspx> are trademarks of the Microsoft group of companies. All other trademarks are property of their respective owners

**Contents**

<!-- TOC -->

- [Enterprise-class networking in Azure hands-on lab step-by-step](#enterprise-class-networking-in-azure-hands-on-lab-step-by-step)
  - [Abstract and learning objectives](#abstract-and-learning-objectives)
  - [Overview](#overview)
  - [Solution architecture](#solution-architecture)
  - [Requirements](#requirements)
  - [Help references](#help-references)
  - [Exercise 1: Create a Virtual Network and provision subnets](#exercise-1-create-a-virtual-network-and-provision-subnets)
    - [Task 1: Create a Virtual Network](#task-1-create-a-virtual-network)
    - [Task 2: Configure subnets](#task-2-configure-subnets)
  - [Exercise 2: Virtual Network Peering](#exercise-2-virtual-network-peering)
    - [Task 1: Configure VNet peering WGVNet1 to WGVNet2 and Vice Versa](#task-1-configure-vnet-peering-wgvnet1-to-wgvnet2-and-vice-versa)
  - [Exercise 3: Configure Network Security Groups and Application Security Groups](#exercise-3-configure-network-security-groups-and-application-security-groups)
    - [Task 1: Create application security groups](#task-1-create-application-security-groups)
    - [Task 2: Configure application security groups](#task-2-configure-application-security-groups)
    - [Task 3: Create network security group](#task-3-create-network-security-group)
  - [Exercise 4: Create route tables with required routes](#exercise-4-create-route-tables-with-required-routes)
    - [Task 1: Create route tables](#task-1-create-route-tables)
    - [Task 2: Add routes to each route table](#task-2-add-routes-to-each-route-table)
  - [Exercise 5: Configure n-tier application and validate functionality](#exercise-5-configure-n-tier-application-and-validate-functionality)
    - [Task 1: Create a load balancer to distribute load between the web servers](#task-1-create-a-load-balancer-to-distribute-load-between-the-web-servers)
    - [Task 2: Configure the load balancer](#task-2-configure-the-load-balancer)
  - [Exercise 6: Provision and configure Azure firewall solution](#exercise-6-provision-and-configure-azure-firewall-solution)
    - [Task 1: Provision the Azure firewall](#task-1-provision-the-azure-firewall)
    - [Task 2: Create Firewall Rules](#task-2-create-firewall-rules)
    - [Task 3: Associate route tables to subnets](#task-3-associate-route-tables-to-subnets)
  - [Exercise 7: Configure Site-to-Site connectivity](#exercise-7-configure-site-to-site-connectivity)
    - [Task 1: Create OnPrem Virtual Network](#task-1-create-onprem-virtual-network)
    - [Task 2: Configure gateway subnets for on premise Virtual Network](#task-2-configure-gateway-subnets-for-on-premise-virtual-network)
    - [Task 3: Create the first gateway](#task-3-create-the-first-gateway)
    - [Task 4: Create the second gateway](#task-4-create-the-second-gateway)
    - [Task 5: Connect the gateways](#task-5-connect-the-gateways)  
  - [Exercise 8: Validate connectivity from 'on-premises' to Azure](#exercise-8-validate-connectivity-from-on-premises-to-azure)
    - [Task 1: Create a virtual machine to validate connectivity](#task-1-create-a-virtual-machine-to-validate-connectivity)
    - [Task 2: Configure routing for simulated 'on-premises' to Azure traffic](#task-2-configure-routing-for-simulated-on-premises-to-azure-traffic)
  - [Exercise 9: Create a Network Monitoring Solution](#exercise-9-create-a-network-monitoring-solution)
    - [Task 1: Create a Log Analytics Workspace](#task-1-create-a-log-analytics-workspace)
    - [Task 2: Configure Network Watcher](#task-2-configure-network-watcher)
  - [Exercise 10: Using Network Watcher to Test and Validate Connectivity](#exercise-10-using-network-watcher-to-test-and-validate-connectivity)
    - [Task 1: Configuring the Storage Account for the NSG Flow Logs](#task-1-configuring-the-storage-account-for-the-nsg-flow-logs)
    - [Task 2: Configuring Diagnostic Logs](#task-2-configuring-diagnostic-logs)
    - [Task 3: Reviewing Network Traffic](#task-3-reviewing-network-traffic)
    - [Task 4: Network Connection Troubleshooting](#task-4-network-connection-troubleshooting)
  - [After the hands-on lab](#after-the-hands-on-lab)

<!-- /TOC -->

# Enterprise-class networking in Azure hands-on lab step-by-step

## Abstract and learning objectives

In this hands-on lab, you will setup and configure virtual networks in a secure hub-and-spoke design. You will also learn how to secure virtual networks by implementing Azure Firewall, network security groups and application security groups, as well as configure route tables on the subnets in your virtual network. Additionally, you will set up access to the virtual network via a jump box and provision a site-to-site VPN connection from another virtual network, providing emulation of hybrid connectivity from an on-premises environment.

At the end of this hands-on lab, you will be better able to configure Azure networking components.

## Overview

You have been asked by Woodgrove Financial Services to provision a proof of concept deployment that will be used by the Woodgrove team to gain familiarity with a complex Virtual Networking deployment, including all of the components that enable the solution. Specifically, the Woodgrove team will be learning:

- How to bypass system routing to accomplish custom routing scenarios.

- How to capitalize on load balancers to distribute load and ensure service availability.

- How to implement Azure Firewall to control hybrid and cross-virtual network traffic flow based on policies.

- How to implement a combination of Network Security Groups (NSGs) and Application Security Groups (ASGs) to control traffic flow within virtual networks.

- How to monitor network traffic for proper route configuration and trouble shooting.

The result of this proof of concept will be an environment resembling this diagram:

## Solution architecture

![This image represents an entire overview of an environment for the result of this proof of concept. On the left is the OnPremVNetRG resource group, in the middle is the WGVNetRG1 resource group, and on the right is the WGVNetRG2 resource group. In the lower right is the MonitoringRG resource group.](images/hol-architecture.png "Solution Architecture")

## Requirements

You must have a working Azure subscription to carry out this hands-on lab step-by-step.

## Help references

|    |            |
|----------|:-------------:|
| **Description** | **Links** |
| IP Addressing and Subnetting for New Users                | <http://www.cisco.com/c/en/us/support/docs/ip/routing-information-protocol-rip/13788-3.html>  |
| ExpressRoute documentation                                | <https://learn.microsoft.com/en-us/azure/expressroute/>                                       |
| ExpressRoute Routing requirements                         | <https://learn.microsoft.com/en-us/azure/expressroute/expressroute-routing/>                  |
| ExpressRoute NAT requirements                             | <https://learn.microsoft.com/en-us/azure/expressroute/expressroute-nat>                       |
| ExpressRoute workflows                                    | <https://learn.microsoft.com/en-us/azure/expressroute/expressroute-workflows>                 |
| ExpressRoute Global Reach                                 | <https://learn.microsoft.com/azure/expressroute/expressroute-global-reach>                    |
| Site-to-Site VPN documentation                            | <https://learn.microsoft.com/en-us/azure/vpn-gateway/>                                        |
| Virtual Network documentation                             | <https://learn.microsoft.com/en-us/azure/virtual-network/>                                    |
| Plan Virtual Networks                                     | <https://learn.microsoft.com/en-us/azure/virtual-network/virtual-network-vnet-plan-design-arm>|
| Virtual Network Traffic Routing                           | <https://learn.microsoft.com/en-us/azure/virtual-network/virtual-networks-udr-overview>       |
| Load Balancer                                             | <https://learn.microsoft.com/en-us/azure/load-balancer/load-balancer-overview>                |
| Microsoft Azure Virtual Datacenter: A Network Perspective | <https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/resources/networking-vdc>   |
| Deploy highly available network virtual appliances        | <https://learn.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha>           |
| Azure Firewall Documentation                              | <https://learn.microsoft.com/azure/firewall/>                                                 |
| Virtual Network Service Endpoints                         | <https://learn.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview>|
| Azure Bastion                                             | <https://learn.microsoft.com/azure/bastion/bastion-overview>                                  |

## Exercise 1: Create a Virtual Network and provision subnets

Duration: 15 minutes

### Task 1: Create a Virtual Network

1. Navigate to the Azure portal. Expand the navigation on the left, then select **+ Create a resource**. In the **Search the Marketplace** box, search for **Virtual network**. Select **Virtual network**, then select **Create**.

2. On the **Create virtual network** blade, on the **Basic** tab, enter the following information:

    - Subscription: **Select your subscription**.

    - Resource group: Select **Create new**, and enter the name **WGVNetRG1**.

    - Name: **WGVNet1**

    - Location: **South Central US**

3. Select **Next: IP Addresses**

    ![In this screenshot, the Basics tab of the 'Create virtual network' blade is depicted with the Resource group, Name, Region, fields and the 'Next: IP Addresses' button highlighted.](images/hol-ex1-task1-create-virtual-network-basics.png "Create virtual network: Basics")

4. On the **Create virtual network - IP Addresses** tab, enter the following information. Then, select **Next: Security**.

    - IPv4 Address space: **10.7.0.0/20**

    - Select **+ Add subnet** then enter the following information and select **Add**.

      - Subnet name: **GatewaySubnet**

      - Subnet address range: **10.7.0.0/27**

5. On the **Create virtual network Security** tab, select **Enable** for **BastionHost**.

6. Enter the following information, then select **Review + Create**.

    - Bastion name: **WGBastion**

    - AzureBastionSubnet address space: **10.7.5.0/24**

    - Public IP address: **Create new**

    - Public IP address name: **BastionPublicIP**

    ![In this screenshot, the 'Security' tab of the Azure portal's 'Create virtual network' blade is depicted with BastionHost, Bastion name, AzureBastionSubnet address space, Public IP address, and 'Review + create' button highlighted.](images/hol-ex1-task1-create-virtual-network-security.png "Create virtual network: Security")

7. Select **Review + Create**.

8. Review the configuration and select **Create**.

    ![In this screenshot, the 'Review + create' tab of the Azure portal's 'Create virtual network' blade is depicted' with the 'Create' button highlighted.](images/hol-ex1-task1-create-virtual-network-review.png "Create virtual network: Review + create")

9. Monitor the deployment status by selecting **Notifications** at the top of the portal. When the deployment is complete, select **Go to Resource**.

### Task 2: Configure subnets

1. Go to the WGVNetRG1 Group, and select **WGVNet1** virtual network resource. Once you are on the WGVNet1 virtual network blade, select **Subnets** under **Settings** from the navigation on the left.

    ![In the Virtual Network blade, in the Settings section of the navigation, Subnets is highlighted.](images/hol-ex1-task2-subnets-navigation.png "Virtual Network blade")

2. In the **Subnets** blade select **+Subnet**.

    ![In the Subnets blade for WGVNet1, the add Subnet button is highlighted.](images/hol-ex1-task2-add-subnet-button.png "Subnets blade")

3. On the **Add subnet** blade, enter the following information:

    - Name: **Management**

    - Address range: **10.7.2.0/25**

    - Network security group: **None**

    - Route table: **None**

    - Service Endpoints: **Leave as Default**.

4. When your dialog looks like the following screenshot, select **Save** to create the subnet.

    ![In this screenshot, the 'Add subnet' blade of the Azure portal is depicted with the settings from the previous step as well as the Save button highlighted.](images/hol-ex1-task2-add-subnet-blade-management.png "Add Subnet blade - Management subnet")

5. Repeat Step 3, enter the following information for the Azure Firewall which we will use to control traffic flow in and out of the Network.

    - Name: **AzureFirewallSubnet** (This name is fixed and cannot be changed.)

    - Address range: **10.7.1.0/24**

    - NAT gateway: **None**

    - Network security group: **None**

    - Route table: **None**

    - Service Endpoints: **Leave as Default**

    ![In this screenshot, the 'Add subnet' blade of the Azure portal is depicted with the settings from this step as well as the Save button highlighted.](images/hol-ex1-task2-add-subnet-blade-azure-firewall.png "Add Subnet blade - Azure Firewall subnet")

## Exercise 2: Virtual Network Peering

Duration: 20 Minutes

### Task 1: Configure VNet peering WGVNet1 to WGVNet2 and Vice Versa

1. Select the resource group **WGVNetRG1**, and select the configuration blade for **WGVNet1**. Select **Peerings** under **Settings** on the left.

2. Select **+ Add**.

    ![In this screenshot, the Peerings blade of the WGVNet1 Virtual Network resources is depicted. With the '+ Add' button selected.](images/hol-ex2-task1-add-peerings-button.png "Virtual network blade")

3. Set the following configuration for the new peering. Select **Add** to create the peering.

    **This virtual network**

    - Peering link name: **VNETPeering_WGVNet1-WGVNet2**

    - Traffic to remote virtual network: **Allow (default)**

    - Traffic forwarded from remote virtual network: **Allow (default)**

    - Virtual network gateway or Route Server: **None (default)**

    **Remote virtual network**

    - Peering link name: **VNETPeering_WGVNet2-WGVNet1**

    - Virtual Network: **WGVNet2**

    - Traffic to remote virtual network: **Allow (default)**

    - Traffic forwarded from remote virtual network: **Allow (default)**

    - Virtual network gateway or Route Server: **None (default)**

    ![In this screenshot, the 'Add peering' blade of the Azure portal is depicted with the required settings specified above highlighted for this virtual network.](images/hol-ex2-task1-add-peering-blade-1.png "WGVNet1 add peering blade - this virtual network")

    ![In this screenshot, the 'Add peering' blade of the Azure portal is depicted with the required settings specified above highlighted for this virtual network.](images/hol-ex2-task1-add-peering-blade-2.png "WGVNet1 add peering blade - this virtual network")

## Exercise 3: Configure Network Security Groups and Application Security Groups

Duration: 20 minutes

In this exercise, you will restrict traffic between tiers of an n-tier application by using network security groups and application security groups.

### Task 1: Create application security groups

1. In the Azure portal, select **+ Create a resource**. In the **Search the Marketplace** box, search for and select **Application security group**. Next, on the **Application security group** blade, select **Create**.

2. On the **Create an application security group** blade, on the **Basics** tab, enter the following information, and select **Review + create**:

    - Subscription: **Select your subscription**.

    - Resource group: **WGVNetRG2**

    - Name: **WebTier**

    - Region: **South Central US** (This must match the location in which you created the **WGVNet2** virtual network.)

    ![In this screenshot, the 'Create an application security group' blade of the Azure portal is depicted with the above specified settings highlighted.](images/hol-ex3-task1-create-application-security-group-web-tier.png "Create Web Tier ASG")

3. On the **Create an application security group** blade, on the **Review + Create** tab, ensure the validation passes, and select **Create**.

4. Repeat the previous two steps to create an application security group named **DataTier** with the following settings.

    - Subscription: **Select your subscription**.

    - Resource group: **WGVNetRG2**

    - Name: **DataTier**

    - Region: **South Central US** (This must match the location in which you created the **WGVNet2** virtual network.)

    ![In this screenshot, the 'Create an application security group' blade of the Azure portal is depicted with the above specified settings highlighted.](images/hol-ex3-task1-create-application-security-group-data-tier.png "Create Data Tier ASG")

### Task 2: Configure application security groups

1. In the Azure portal, navigate to the **Virtual machines** blade and select **WGWEB1**.

2. On the **WGWEB1** blade, select **Networking** under **Settings** on the left.

3. On the **WGWEB1 - Networking** blade, select **Application security groups** and then select **Configure the application security groups**.

    ![In this screenshot, the networking blade for WGWEB1 is displayed. The Networking navigation option, 'Application security groups' section heading, and 'Configure the application security groups' link are highlighted.](images/hol-ex3-task2-virtual-machine-configure-application-security-groups.png "Networking blade - application security groups section")

4. On the **Configure the application security groups** blade, in the **Application security groups** drop-down list, select **WebTier**, then **Save**.

    ![In this screenshot, the 'Configure the application security groups' blade for WGWEB1 is depicted with the WebTier app security group selected in the 'Application security groups' dropdown and the Save button highlighted.](images/hol-ex3-task2-configure-web-tier-asg.png "Configure Web Tier ASG")

5. Repeat steps 1-4, but this time for **WGWEB2** in order to assign to its network interface the **WebTier** application security group.

6. Repeat steps 1-4, but this time for **WGSQL1** in order to assign to its network interface the **DataTier** application security group.

### Task 3: Create network security group

This task will create a network security group with the following rules:

- Allow SQL traffic (port 1433) from the WebTier application security group to the DataTier application security group.
- Allow HTTP web traffic (port 80) from anywhere to the WebTier application security group.
- Allow RDP traffic (port 3389) from the Azure Bastion range to anywhere.
- Deny all other traffic from the virtual machines to the data tier.
- Deny all other traffic from the virtual machines to the web tier.

1. In the Azure portal, select **+ Create a resource**. In the **Search the Marketplace** box, search for **Network security group** and press Enter. On the **Network security group** blade, select **Create**.

2. On the **Create network security group** blade, enter the following information, and select **Review + Create** then **Create**:

    - Subscription: **Select your subscription**.

    - Resource group: **WGVNetRG2**

    - Name: **WGAppNSG1**

    - Region: **South Central US** (This must match the location in which you created the **WGVNet2** virtual network.)

    ![In this screenshot, the 'Create network security group' blade of the Azure portal is depicted with the required settings listed above highlighted.](images/hol-ex3-task3-create-network-security-group.png "Create WGApp NSG")

3. In the Azure Portal, navigate to **All Services**, type **Network security groups** the search box and select **Network security groups**.

4. On the **Network security groups** blade, select **WGAppNSG1**.

5. On the **WGAppNSG1** blade, select **Inbound security rules** under **Settings** on the left and select **Add**.

    ![In this screen, the 'Inbound security rules' blade for the WGAppNSG1 network security group is displayed. 'Inbound security rules' in the left navigation and the Add button are highlighted.](images/hol-ex3-task3-add-inbound-security-rule.png)

6. On the **Add inbound security rule** blade, enter the following information, and select **Add**:

    - Source: **Application security group**

    - Source application security group: **WebTier**

    - Source port ranges: **\***

    - Destination: **Application security group**

    - Destination application security group: **DataTier**

    - Destination port ranges: **1433**

    - Protocol: **TCP**

    - Action: **Allow**

    - Priority: **100**

    - Name: **AllowDataTierInboundTCP1433**

    ![In this screenshot, the 'Add inbound security rule' blade of the Azure portal is depicted with the above required settings highlighted.](images/hol-ex3-task3-add-inbound-security-rule-data-tier-inbound.png "Configure WGAppNSG1 AllowDataTierInboundTCP1433 rule")

7. On the **WGAppNSG1 - Inbound security rules** blade, select **Add**.

8. On the **Add inbound security rule** blade, enter the following information, and select **Add**:

    - Source: **Any**

    - Source port ranges: **\***

    - Destination: **Application security group**

    - Destination application security group: **WebTier**

    - Destination port ranges: **80**

    - Protocol: **TCP**

    - Action: **Allow**

    - Priority: **150**

    - Name: **AllowAnyWebTierInboundTCP80**

    ![In this screenshot, the 'Add inbound security rule' blade of the Azure portal is depicted with the above required settings highlighted.](images/hol-ex3-task3-add-inbound-security-rule-web-tier-inbound.png "Configure WGAppNSG1 AllowAnyWebTierInboundTCP80 rule")

9. On the **WGAppNSG1 - Inbound security rules** blade, select **Add**.

10. On the **Add inbound security rule** blade, enter the following information, and select **Add**:

    - Source: **IP Addresses**

    - Source IP addresses/CIDR ranges: **10.7.2.0/25, 10.7.5.0/24** (This IP address range represents the Bastion subnet on WGVNet1.)

    - Source port ranges: **\***

    - Destination: **Any**

    - Destination port ranges: **3389**

    - Protocol: **Any**

    - Action: **Allow**

    - Priority: **200**

    - Name: **AllowMgmtInboundAny3389**

    ![In this screenshot, the 'Add inbound security rule' blade of the Azure portal is depicted with the above required settings highlighted.](images/hol-ex3-task3-add-inbound-security-rule-mgmt-inbound.png "Configure WGAppNSG1 AllowMgmtInboundAny3389 rule")

11. On the **WGAppNSG1 - Inbound security rules** blade, select **Add**.

12. On the **Add inbound security rule** blade, enter the following information, and select **Add**:

    - Source: **Service Tag**

    - Source service tag: **VirtualNetwork**

    - Source port ranges: **\***

    - Destination: **Application security group**

    - Destination application security group: **DataTier**

    - Destination port ranges: **\***

    - Protocol: **Any**

    - Action: **Deny**

    - Priority: **1000**

    - Name: **DenyVNetDataTierInbound**

    ![In this screenshot, the 'Add inbound security rule' blade of the Azure portal is depicted with the above required settings highlighted.](images/hol-ex3-task3-add-inbound-security-rule-deny-data-tier-inbound.png "Configure WGAppNSG1 DenyVNetDataTierInbound rule")

13. On the **WGAppNSG1 - Inbound security rules** blade, select **Add**.

14. On the **Add inbound security rule** blade, enter the following information, and select **Add**:

    - Source: **Service Tag**

    - Source service tag: **VirtualNetwork**

    - Source port ranges: **\***

    - Destination: **Application security group**

    - Destination application security group: **WebTier**

    - Destination port ranges: **\***

    - Protocol: **Any**

    - Action: **Deny**

    - Priority: **1050**

    - Name: **DenyVNetWebTierInbound**

    ![In this screenshot, the 'Add inbound security rule' blade of the Azure portal is depicted with the above required settings selected.](images/hol-ex3-task3-add-inbound-security-rule-deny-web-tier-inbound.png "Configure WGAppNSG1 DenyVNetWebTierInbound rule")

15. On the **WGAppNSG1 - Inbound security rules** blade, select **Subnets** under **Settings** and then select **+ Associate**.

    ![In this screenshot, Subnets blade of WGAppNSG1 is depicted. The 'Subnets' navigation and the '+ Associate' button are highlighted.](images/hol-ex3-task3-associate-subnet-nsg.png "WGAppNSG1 Subnets")

16. On the **Associate subnet** blade, select **WGVNet2** on the **Virtual network** drop down and **AppSubnet** on the **Subnet** dropdown.

17. Select **OK** at the bottom of the **Associate subnet** blade.

## Exercise 4: Create route tables with required routes

Duration: 15 minutes

Route Tables are containers for User Defined Routes (UDRs). The route table is created and associated with a subnet. UDRs allow you to direct traffic in ways other than normal system routes would. In this case, UDRs will direct outbound traffic via the Azure firewall.

### Task 1: Create route tables

1. On the main portal menu, select **+ Create a Resource**. Type **route** into the search box, and select **Route table** then select **Create**.

2. On the **Create a Route table** blade enter the following information:

    - Subscription: **Select your subscription**.

    - Resource group: Select **WGVNetRG1** from the drop down.

    - Region: **South Central US**

    - Name: **MgmtRT**

    - Propagate gateway routes: **Yes**

3. When the dialog looks like the following screenshot, select **Review + Create** then **Create**.

    ![In this screenshot, the 'Create Route table' blade of the Azure portal is depicted with the required settings listed in the previous step highlighted.](images/hol-ex4-task1-create-route-table-basics.png "Create route table")

4. Repeat steps 1 and 2 to create the **AppRT** route table:

    - Subscription: **Select your subscription**.

    - Resource group: Select **WGVNetRG2** from the drop down.

    - Region: **South Central US**

    - Name: **AppRT**

    - Propagate gateway routes: **Yes**

5. Once route tables are created, your **Route tables** blade should look like the following screenshot:

    ![In this screenshot, the 'Route tables' blade of the Azure portal is depicted with the two route tables created in this task listed.](images/hol-ex4-task1-route-tables-blade.png "Route table link")

### Task 2: Add routes to each route table

1. Select the **AppRT** route table, and select **Routes** under **Settings** on the left.

    ![In this screenshot, the AppRT route table blade in the Azure portal is depicted with Routes in the Settings section of the navigation on the left highlighted.](images/hol-ex4-task2-routes-navigation.png "Route table blade ")

2. On the **Routes** blade, select **+ Add**. Enter the following information, and select **Add**:

    - Route name: **AppToInternet**

    - Address prefix destination: **IP Addresses**

    - Address prefix: **0.0.0.0/0**

    - Next hop type: **Virtual appliance**

    - Next hop address: **10.7.1.4** (This is the private IP of Azure Firewall.)

    ![In this screenshot, the 'Add route' blade of the AppRT route table in the Azure portal is depicted with the required settings listed above highlighted.](images/hol-ex4-task2-add-route-blade-app-to-internet.png "Add route configuration")

3. Repeat this procedure to add the **AppToMgmt** route using the following information:

    - Route name: **AppToMgmt**

    - Address prefix destination: **IP Addresses**

    - Address prefix: **10.7.0.8/29**

    - Next hop type: **Virtual appliance**

    - Next hop address: **10.7.1.4** (This is the private IP of Azure Firewall.)

    ![In this screenshot, the 'Add route' blade of the AppRT route table in the Azure portal is depicted with the required settings listed above highlighted.](images/hol-ex4-task2-add-route-blade-app-to-mgmt.png "Edit route")

4. Upon completion, your routes in the **AppRT** route table should look like the following screenshot:

    ![In this screenshot, the Routes blade of the AppRT route table is depicted with the two newly creates routes listed.](images/hol-ex4-task2-apprt-route-table.png "Route table ")

5. In the Azure Portal, go to All Services and type **route** in the search box and select **Route tables**.

6. Select **MgmtRT**, and select **Routes** under **Settings** on the left.

    ![In this screenshot, the 'Routes' blade of the Azure portal is depicted with the MgmtRT route table selected. The Routes option under the Settings section of the left navigation is highlighted.](images/hol-ex4-task2-mgmtrt-in-route-tables.png "MgmtRT")

7. On the **Routes** blade, select **+Add**. Enter the following information, and select **Add**:

    - Route name: **MgmtToOnPremises**

    - Address prefix destination: **IP Addresses**

    - Address prefix: **192.168.0.0/16**

    - Next hop type: **Virtual network gateway**

    - Next hop address: **Leave blank**.

    ![In this screenshot, the 'Add route' blade of the MgmtRT route table in the Azure portal is depicted with the required settings listed above highlighted.](images/hol-ex4-task2-add-route-blade-mgmt-to-onpremises.png "Add route")

8. Add the **MgmtToApp** route using the following information:

    - Route name: **MgmtToApp**

    - Address prefix destination: **IP Addresses**

    - Address prefix: **10.7.2.0/25**

    - Next hop type: **Virtual appliance**

    - Next hop address: **10.7.1.4** (This is the private IP of Azure Firewall.)

    ![In this screenshot, the 'Add route' blade of the MgmtRT route table in the Azure portal is depicted with the required settings listed above highlighted.](images/hol-ex4-task2-add-route-blade-mgmt-to-app.png "Add route")

9. Upon completion, your routes in the **MgmtRT** route table should look like the following screenshot:

    ![In this screenshot, the Routes blade of the MgmtRT route table is depicted with the two newly creates routes listed.](images/hol-ex4-task2-mgmtrt-route-table.png "Route table")

    >**Note:** The route tables and routes you have just created are not associated with any subnets yet, so they are not impacting any traffic flow yet. This will be accomplished later in the lab.

## Exercise 5: Configure n-tier application and validate functionality

Duration: 20 minutes

In this exercise, you will create and configure a load balancer to distribute the load between the web servers.

### Task 1: Create a load balancer to distribute the load between the web servers

1. In the Azure portal, from the home page navigation, select **Load balancers**, then select **+ Create**.

    ![In this screenshot, the navigation from the Azure portal home page is expanded. The hamburger menu (three stacked lines) and 'Load balancers' option are highlighted.](images/hol-ex5-task1-load-balancer-navigation.png)

2. On the **Create load balancer** blade, on the **Basics** tab, enter the following values:

    - Subscription: **Select your subscription**.

    - Resource group: **WGVNetRG2**

    - Name: **WGWEBLB**

    - Region: **South Central US**

    - SKU: **Standard**

    - Type: **Internal**

    - Tier: **Regional**

    Ensure your **Create load balancer** dialog looks like the following, and select **Next: Frontend IP configuration** then select **Create**.

    ![In this screenshot, the 'Create load balancer' blade is depicted with the required settings listed above and the 'Next: Frontend IP configuration' button highlighted.](images/hol-ex5-task1-create-load-balancer-wgweblb.png "Create load balancer")

3. On the **Frontend IP configuration** tile, select **+ Add a frontend IP configuration** and enter the following values:

    - Name: **WGWEBLBIP**

    - Virtual network: **WGVNet2**

    - Subnet: **AppSubnet (10.8.0.0/25)**

    - Assignment: **Static**

    - IP address: **10.8.0.100**

    - Availability zone: **1**

    Ensure your **Create load balancer - Frontend IP configuration** dialog looks like the following, and select **Add**.

    ![In this screenshot, the 'Frontend IP configuration' blade is depicted with the required settings listed above as well as the Add button highlighted.](images/hol-ex5-task1-frontend-ip-config.png "Frontend IP configuration")

4. Select **Review + create**, and then select **Create**.

    >**Note**: **Backend pools** can now be configured within the **Create Load balancer** wizard.  For this exercise, we will complete this in the next task to show where to find it on the resource.

### Task 2: Configure the load balancer

1. Open the **WGWEBLB** load balancer in the Azure portal.

2. Select **Backend pools**, and select **+Add** at the beginning.

    ![In this screenshot, the Azure portal blade for the WGWEBLB load balancer is depicted with Backend pools' selected on the left and the '+ Add' button selected.](images/hol-ex5-task2-backend-pools-add-button.png "Load balancer blade")

3. Enter **LBBE** for the pool name. Select **NIC** for **Backend Pool Configuration**. Under **IP Configurations**, select **+ Add**.

    ![In this screenshot, the 'Add backend pool' blade is depicted with the Name and 'Associated to' fields filled in as listed above.](images/hol-ex5-task2-add-backend-pool.png "Add backend pool blade")

4. Under **Virtual machine**, select **+ Add** and choose the **WGWEB1** and **WGWEB2** virtual machines and select **Add**.

    ![In this screenshot, the 'Add IP configurations to backend pool' blade is depicted with WGWEB1 and WGWEB2 options highlighted and checked. The Add button is also highlighted.](images/hol-ex5-task2-add-vms-to-backend-pool.png)

    >**Note**: If you do not see WGWEB1 in the Virtual Machine selection list, the public IP address was not created as a Standard SKU.  Locate **webip** and in the **Overview** tile, select the **Upgrade to Standard SKU** banner to change the SKU.  You will need to change the IP to **Static** in the **Configuration** and temporarily **Disassociate** it from **WGWEB1NetworkInterface**. Once upgraded, **Associate** webip with the **Network Interface** for WGWEB1.

    ![In this screenshot, the upgrade to standard sku will take you through the process of upgrading the public IP address.](images/hol-ex5-task2-upgrade-ip-address-sku.png "Upgrade Public IP from Basic to Standard")

5. Select **Save** at the bottom of the **Add backend pool** blade to add the backend pool.

6. Wait to proceed until the Backend pool configuration is finished updating. When the backends are added, they should look like the following image.

    ![In this screenshot, the 'WGWEBLB - Backend pools' blade of the Azure portal is depicted. The two virtual machines in the backend pool show a status of running, indicating that the backend pool configuration is complete.](images/hol-ex5-task2-backend-pools.png "Backend pool blade")

7. Next, under **Settings** on the WGWEBLB Load Balancer blade, select **Health Probes**. Select **+ Add**, and use the following information to create a health probe.

    - Name: **HTTP**

    - Protocol: **HTTP**

    ![In this screenshot, the 'WGWEBLB' load balancer blade of the Azure portal is depicted with 'Health probes' under 'Settings' in the left navigation highlighted.](images/hol-ex5-task2-health-probes-add-button.png "Settings section, Add health probe blade")

    ![In this screenshot, the 'Add health probe' blade is depicted with the required settings listed above selected along with the Add button selected.](images/hol-ex5-task2-add-health-probe-blade-http.png "Add health probe blade")

8. Select **Add**.

9. After the Health probe has been added, select **Load balancing rules** from the left navigation. Select **+ Add** and complete the configuration as shown below followed by selecting **Add**.

    - Name: **HTTP**

    - Frontend IP address: Select the load balancer IP entry with **10.8.0.100**.

    - Backend pool: **LBBE**

    - Port: **80**

    - Backend port: **80**

    - Health probe: **HTTP**

    ![In this screenshot, the 'Add load balancing rule' blade of the Azure portal is depicted with the required settings listed above and the Add button highlighted.](images/hol-ex5-task2-add-load-balancing-rule-http.png "Add load balancing rule")

10. Navigate to WGWEB1 in the Azure portal. Connect to WGWEB1 via Bastion. Within WGWEB1, open Microsoft Edge from the Start menu and navigate to <http://10.8.0.100>. Ensure that you successfully connect to either one of the two Web servers.

    ![In this screenshot, the web page that appears when you navigate to the load balancer IP address appears indicating that your successfully connected to the WEB1 web server.](images/hol-ex5-task2-cloudshop-demo-on-wgweb1.png "Server response for the CloudShop demo on WGWEB1")

    ![In this screenshot, the web page that appears when you navigate to the load balancer IP address appears indicating that your successfully connected to the WEB2 web server.](images/hol-ex5-task2-cloudshop-demo-on-wgweb2.png "Server response for the CloudShop demo on WGWEB1")

11. Using the portal, disassociate the public IP from the NIC of **WGWEB1** VM. Do this by navigating to the VM and selecting **Networking** under **Settings** on the left. Select the **NIC Public IP** then choose **Dissociate**. Select **Yes** when prompted.

    ![In this screenshot, the WGWEB1 - Networking blade of the Azure portal is depicted with the NIC Public IP selected.](images/hol-ex5-task2-wgweb1-dissociate-ip-address.png "Virtual machine networking blade")

12. Next, return to the **WGWEB1 - Networking** blade and select the **Network Interface**.

13. Select **IP configurations** under **Settings** on the left.

    ![In this screenshot, the network interface page for the web server on the Azure portal is depicted with 'IP configuration' in the left navigation highlighted.](images/hol-ex5-task2-ip-configurations.png "Network interface blade")

14. Next, select **ipconfig1** shown above.

15. Select and make sure that the **Public IP address settings** is shown as **Dissociate**, and select **Save** if necessary. This should remove the public IP address from the network interface of the VM.

    ![In this screenshot, the 'ipconfig1' blade of the web server NIC is depicted with the 'Public IP address' set to 'Disassociate' and the Save button selected.](images/hol-ex5-task2-ipconfig1-dissociate.png "IP configuration blade")

## Exercise 6: Provision and configure Azure firewall solution

Duration: 15 minutes

In this exercise, you will provision and configure an Azure firewall in your network.

### Task 1: Provision the Azure firewall

1. In the Azure portal, select **+ Create a resource**. In the **Search the Marketplace** text box, type **Azure Firewall**, in the list of results, select **Firewall**, and on the **Firewall** blade, select **Create**.

2. On the **Create a firewall** blade, on the **Basics** tab, enter the following information:

    - Subscription: select your subscription.

    - Resource group: **WGVNetRG1**

    - Name: **azureFirewall**

    - Region: **South Central US**

    - Firewall SKU: **Standard**

    - Firewall management: **Use Firewall rules (classic) to manage this Firewall**

    - Choose a Virtual network: **Use existing**

    - Virtual network: **WGVNet1**

    - Public IP address: **(Add new) azureFirewall-ip**

        ![In this screenshot, the 'Create a firewall' blade is depicted with the required settings listed above selected.](images/hol-ex6-task1-create-a-firewall-wgvnetrg1.png "Create a firewall blade")

3. Select **Review + create** and then select **Create** to provision the Azure Firewall.

### Task 2: Create Firewall Rules

Within 1-2 minutes, the resource group **WGVNetRG1** will have the firewall created. Next, we will create firewall rules to allow the inbound and outbound traffic.

1. On the main Azure menu, select **Resource groups**.

2. Select the **WGVNetRG1** resource group. This resource group contains the azure firewall and its public IP address resources.

3. Navigate to the **azureFirewall-ip** blade and note the value of its public IP address. You will need it later in this task.

4. Navigate to the **azureFirewall** blade, and, on the **Overview** page, select **Rules (classic)** under **Settings** on the left.

    ![In this screenshot, the Azure Firewall page is depicted with Rules (classic) selected on the left.](images/hol-ex6-task2-azure-firewall-rules-navigation.png "Azure Firewall overview page")

5. Select **+ Add NAT Rule collection** and enter the following information to create an inbound NAT Rule (a collection is a list of rules that share the same priority and action) then select **Add**:

    - Name: **NATRuleCollection1**

    - Priority: **250**

    - Rules name: **IncomingHTTP**

    - Protocol: **TCP**

    - Source type: **IP Address**

    - Source: **\***

    - Destination Address: Type the public IP address assigned to the firewall you identified earlier in this task.

    - Destination ports: **80** (to allow HTTP traffic)

    - Translated Address: **10.8.0.100** (Private IP of the Azure Load Balancer you deployed earlier in this lab.)

    - Translated Port: **80**

6. Back on the **azureFirewall - Rules (classic)** page, select the newly created NAT rule collection. Add another rule for HTTPS, as illustrated in the following screenshot (alternatively you could create a single rule for both HTTP and HTTPS). The rules should look like the image below.

    - Rules name: **IncomingHTTPS**

    - Protocol: **TCP**

    - Source type: **IP Address**

    - Source: **\***

    - Destination Address: Type the public IP address assigned to the firewall you identified earlier in this task.

    - Destination ports: **443**

    - Translated Address: **10.8.0.100**

    - Translated Port: **443**

    ![In this screenshot, the 'Edit NAT rule collection' page is depicted with the required settings listed above selected.](images/hol-ex6-task2-edit-nat-rule-collection.png "Azure Firewall NAT Rules for HTTP and HTTPS")

7. Select **Add** and wait until the update completes.

8. Back on the Azure Firewall **Rules (classic)** page, select **Network rule collection**. Then Select **+ Add Network Rule collection** and enter the following information to create a Network Rule for inbound traffic. This rule allows HTTP connectivity from any directly connected network targeting the frontend IP address of the load balancer.

    - Name: **NetworkRuleCollectionAllow1**

    - Priority: **100**

    - Action: **Allow**

    - Rules name (IP Addresses): **IncomingWeb**

    - Protocol: **TCP**

    - Source: **\***

    - Destination Address: **10.8.0.100**

    - Destination ports: **80,443**

    ![In this screenshot, the azureFirewall Rules (classic) blade is depicted. The 'Network rule collection' tab and the 'Add network rule collection' link are highlighted.](images/hol-ex6-task2-network-rule-collection.png)

9. Create another rule for Remote Desktop sessions from the Management subnet on WGVNet1. The IP Addresses rules should look like the image below.

    - Rules name (IP Addresses): **IncomingMgmtRDP**

    - Protocol: **TCP**

    - Source: **10.7.2.0/25**

    - Destination Address: **10.8.0.0/25**

    - Destination ports: **3389**

    ![In this screenshot, the 'IP Addresses' section of the 'Add network rule collection' blade of the Azure portal is depicted with the required settings listed above selected.](images/hol-ex6-task2-add-network-rule-mgmt-subnet.png "Azure Firewall IP Addresses section")

10. Select **Add** and wait until the update completes.

### Task 3: Associate route tables to subnets

1. In the Azure portal, navigate to the blade of the **WGVNetRG2** resource group.

2. Select **AppRT**, followed by **Subnets** and then select **+ Associate**.

    ![In this screenshot, the AppRT - Subnets blade is depicted with Subnets selected on the left and the '+ Associate' button selected.](images/hol-ex6-task3-route-table-associate-button.png "AppRT Route table blade")

3. On the **Associate subnet** blade, select **WGVNet2** on the **Virtual network** drop down. Select **AppSubnet** on the **Subnet** dropdown.

    ![In this screenshot, the 'Associate subnet' blade is depicted with the 'WGVNet2' virtual network and 'AppSubnet' subnet selected along with the 'OK' button.](images/hol-ex6-task3-associate-subnet-blade-wgvnet2.png "Associate subnet section for AppRT")

4. Select **OK** at the bottom of the **Associate subnet** blade.

5. Navigate to the blade of the **WGVNetRG1** resource group, and select **MgmtRT**, then **Subnets**.

6. Select **+ Associate**.

7. On the **Associate subnet** blade, select **WGVNet1** on the **Virtual network** drop down. Select **Management** on the **Subnet** dropdown.

    ![In this screenshot, the 'Associate subnet' blade is depicted with the 'WGVNet1' virtual network and 'Management' subnet selected along with the 'OK' button.](images/hol-ex6-task3-associate-subnet-blade-wgvnet1.png "Associate subnet blade for MgmtRT")

8. Select **OK** at the bottom of the **Associate subnet** blade.

## Exercise 7: Configure Site-to-Site connectivity

Duration: 60 minutes

In this exercise, we will simulate an on-premises connection to the internal web application. To do this, we will first set up another Virtual Network in a separate Azure region followed by the Site-to-Site connection of the 2 Virtual Networks Finally, we will set up a virtual machine in the new Virtual Network to simulate on-premises connectivity to the internal load-balancer.

### Task 1: Create OnPrem Virtual Network

1. In the Azure portal, select **+ Create a resource**, then in the **Search the Marketplace** box, search for and select **Virtual network**. Select **Create**.

2. On the **Create virtual network** blade, enter the following information:

    - Subscription: **Select your subscription**.

    - Resource group: Select **Create new**, and enter the name **OnPremVNetRG**.

    - Name: **OnPremVNet**

    - Region: **East US** (Make sure this is **NOT** the same location you have specified in the previous exercises.)

3. Leave the other options with their default values.

4. Upon completion, it should look like the following screenshot. Validate the information is correct, and select **Next: IP Addresses**.

    ![In this screenshot, the Basics tab of the 'Create virtual network' blade in the Azure portal is depicted with the required settings selected.](images/hol-ex7-task1-create-virtual-network-onpremvnet.png "Create virtual network")

5. On the **IP addresses** tab of the **Create virtual network blade**, enter the following information.

    - Address space: **192.168.0.0/16**

    - Select **+ Add subnet** then enter the following information in the blade that appears on the right and select **Add**.

      - Subnet name: **default**

      - Subnet address range: **192.168.0.0/24**

6. Select **Review + create** then **Create**.

### Task 2: Configure gateway subnets for on premise Virtual Network

1. Select the **OnPremVNetRG** Resource Group and then open the **OnPremVNet** blade and select **Subnets**.

2. Next, select **+ Gateway subnet**.

    ![In this screenshot, the 'OnPremVNet - Subnets' blade is depicted with the Subnets selected on the left and the '+ Gateway subnet' selected.](images/hol-ex7-task2-onpremvnet-add-gateway-subnet.png "Virtual network blade")

3. Specify the following configuration for the subnet, and select **Save**:

    - Subnet address range: **192.168.1.0/27**

    - Route table: **None** (We will add this later.)

    ![In this screenshot, the 'Add subnet' blade of the Azure portal is depicted with the required settings listed above selected along with the Save button.](images/hol-ex7-task2-add-gateway-subnet.png "Add subnet")

4. Next, select **+ Subnet** and add the **OnPremManagementSubnet** subnet to the **OnPremVNet**, as shown below in the screenshot:

    - Name: **OnPremManagementSubnet**

    - Address range: **192.168.2.0/27**

    - Leave the rest of the values as their defaults. Select **Save**.

    ![In this screenshot, the 'Add subnet' blade of the Azure portal is depicted with the required settings listed above selected along with the Save button.](images/hol-ex7-task2-add-onprem-mgmt-subnet.png "Add subnet")

### Task 3: Create the first gateway

1. Using the Azure Management portal, select **+ Create a resource**, type **Virtual network gateway** in the **Search the Marketplace** text box, in the list of results, select **Virtual network gateway**, and then select **Create**.

2. On the **Create virtual network gateway** blade,  enter the following information and select **Review + create**:

    - Subscription: **Select your subscription**.

    - Name: **OnPremWGGateway**

    - Region: **East US** (This must match the location in which you created the **OnPremVNet** virtual network.)

    - Gateway type: **VPN**

    - VPN type: **Route-based**

    - SKU: **VpnGw1**

    - Virtual network: **OnPremVNet**

    - Public IP address: **Create new**

    - Public IP address name: **onpremgatewayIP1**

    - Enable active-active mode: **Enabled**

    - Second Public IP address name: **onpremgatewayIP2**

    - Configure BGP: **Disabled**

    ![In this screenshot, the 'Create virtual network gateway' blade of the Azure portal is depicted with the above Project details and Instance details highlighted.](images/hol-ex7-task3-create-onpremwggateway-1.png "Create virtual network gateway Instance details")

    ![In this screenshot, the 'Create virtual network gateway' blade of the Azure portal is depicted with the Public IP settings.](images/hol-ex7-task3-create-onpremwggateway-2.png "Create virtual network gateway Public IP addresses")

3. Validate your settings and select **Review + Create** then **Create**.

    >**Note:** The gateway will take 30-45 minutes to provision. Rather than waiting, continue to the next task.

### Task 4: Create the second gateway

1. Using the Azure Management portal, select **+ Create a resource**, type **Virtual Network gateway** in the **Search the Marketplace** text box, in the list of results, select **Virtual network gateway**, and then select **Create**.

2. On the **Create virtual network gateway** blade,  enter the following information and select **Review + create**:

    - Subscription: **Select your subscription**.

    - Name: **WGVNet1Gateway**

    - Region: **South Central US** (This must match the location in which you created the **WGVNet1** virtual network.)

    - Gateway type: **VPN**

    - VPN type: **Route-based**

    - SKU: **VpnGw1**

    - Virtual network: **WGVNet1**

    - Resource group: **WGVNetRG1**

    - Public IP address: **Create new**

    - Public IP address name: **vnet1gatewayIP1**

    - Enable active-active mode: **Enabled**

    - Second Public IP address name: **vnet1gatewayIP2**

    - Configure BGP: **Disabled**

    ![In this screenshot, the 'Create virtual network gateway' blade of the Azure portal is depicted with the Instance details settings selected.](images/hol-ex7-task4-create-wgvnet1gateway-1.png "Create virtual network gateway - Instance details")

    ![In this screenshot, the 'Create virtual network gateway' blade of the Azure portal is depicted with the above public IP settings selected.](images/hol-ex7-task4-create-wgvnet1gateway-2.png "Create virtual network gateway - Public IP addresses")

3. Validate your settings and select **Review + Create** then **Create**.

    >**Note:** The gateway will take 30-45 minutes to provision. You will need to wait until both gateways are provisioned before proceeding to the next section.

4. The Azure portal will display a notification when the deployments have completed.

### Task 5: Connect the gateways

1. In the Azure portal, select **+ Create a resource**, in the **Search the Marketplace** text box, type in **Connection**, and press **Enter**.

2. On the **Connection** blade, select **Create**.

3. On the **Basics** blade, leave the **Connection type** set to **VNet-to-VNet**. Select the existing **WGVNetRG1** resource group. Enter the following information and select **Next: Settings**:
    - Establish bidirectional connectivity - checked
    - First connection name - **WGVNet1-to-OnPremWGGateway**
    - Second connection name - **WGGateway-to-WGVNet1**
    - Region - **South Central US**

    ![In this screenshot, the Basics step of the 'Create connection' blade of the Azure portal is depicted with the required settings listed above selected.](images/hol-ex7-task5-create-connection-vnet-to-vnet.png "Basics")

4. On the Settings step, select **WGVNet1Gateway** as the first virtual network gateway and **OnPremWGGateway** as the second virtual network gateway. Ensure **Establish bidirectional connectivity** and **IKEv2** is selected. Enter a shared key, such as **A1B2C3D4**. Select **Review + create**.

    ![In this screenshot, the Settings step of the 'Create connection' blade of the Azure portal is depicted with the required settings listed above selected including the two virtual network gateway resources created earlier.](images/hol-ex7-task5-create-connection-vnet-to-vnet-settings.png "select virtual network gateway")

5. Select **Create** on the **Summary** page to create the connection.

6. In the Azure portal, in the 'Search resources, services, and docs' search box, type **connections** in the search text box. Select **Connections**.

    ![In this screenshot, the 'Search resources, services, and docs' search box of the Azure portal is depicted with Connections searched for and selected.](images/hol-ex7-task5-search-for-connections.png "Azure Portal")

7. Watch the progress of the connection status, and use the **Refresh** icon until the status changes for both connections from **Unknown** to **Connected**. This may take 5-10 minutes or more. You might need to refresh the page to see the change in status.

    ![In this screenshot, the Connections blade of the Azure portal is depicted with the two connections created earlier listed with their respective statuses showing as Connected.](images/hol-ex7-task5-connections-blade.png "Connections blade")

## Exercise 8: Validate connectivity from 'on-premises' to Azure

Duration: 30 minutes

In this exercise, you will validate connectivity from your simulated on-premises environment to Azure.

### Task 1: Create a virtual machine to validate connectivity

1. Create a new virtual machine in the OnPremVnet virtual network. In the Azure portal, select **+ Create a resource** and select **Virtual machine**.

2. On the **Create a virtual machine** blade, on the **Basics** tab, enter the following information, and select **Next : Disks >**:

    - Subscription: **Select your subscription*.

    - Resource group: Select **Create new** and enter **OnPremVMRG**.

    - Virtual machine name: **OnPremVM**

    - Region: **(US) East US** (This must match the region in which you created the OnPremVNet virtual network.)

    - Availability options: **No infrastructure redundancy required**

    - Image: **Windows Server 2019 Datacenter - Gen2**

    - Size: **Standard DS1 v2**

    - User name: **demouser**

    - Password/Confirm password: **demo\@pass123**

    - Public inbound ports: **Allow selected ports**

    - Select inbound ports: **RDP**

3. On the **Create a virtual machine** blade, on the **Disks** tab, set the following configuration and select **Next : Networking >**:

    - OS disk type: **Premium SSD**

4. On the **Create a virtual machine** blade, on the **Networking** tab, set the following configuration and select **Next : Management >**:

    - Virtual network: **OnPremVNet**

    - Subnet: **OnPremManagementSubnet (192.168.2.0/29)**

    - Public IP: **(new)OnPremVM-ip**

    - NIC network security group: **Basic**

    - Public inbound ports: **Allow selected ports**

    - Select inbound ports: **RDP**

    - Accelerated networking: **Unchecked**

    - Load balancing options: **None**

5. On the **Create a virtual machine** blade, on the **Management** tab, set the following configuration and select **Review + create**:

    - System assigned managed identity: **Unchecked**

    - Login with Azure AD: **Unchecked**

    - Enable auto-shutdown: **Unchecked**

    - Enable backup: **Unchecked**

    - Enable OS guest diagnostics: **Unchecked**

6. On the **Create a virtual machine** blade, on the **Review + Create** tab, ensure the validation passes, and select **Create**. The virtual machine will take about 5 minutes to provision.

### Task 2: Configure routing for simulated 'on-premises' to Azure traffic

When packets arrive from the simulated 'on-premises' Virtual Network (OnPremVNet) to the 'Azure-side' (WGVNet1), they arrive at the gateway WGVNet1Gateway. This gateway is in a gateway subnet (10.7.0.0/27). For packets to be directed to the Azure firewall, we need another route table and route to be associated with the gateway subnet on the 'Azure-side'.

1. On the Azure portal select **All services** at the left navigation. Enter **Route** in the search box, and select **Route tables**.

2. On the **Route tables** blade, select **+ Add**.

3. On the **Create route table** blade, enter the following information:

    - Subscription: **Select your subscription**.

    - Resource group: Select the drop-down menu, and select **WGVNetRG1**.

    - Region: **South Central US** (This must match the location in which you created the **WGVNet1** virtual network.)

    - Name: **WGAzureVNetGWRT**

    - Propagate gateway routes: **Yes**

    ![In this screenshot, the 'Create route table' blade of the Azure portal is depicted with the required settings listed above selected.](images/hol-ex8-task2-create-route-table-wgazurevnetgwrt.png "Create route table")

4. Select **Review + create** then **Create**.

5. Select **Go to resource** to go to the **WGAzureVNetGWRT** route table.

6. Select **Routes** under **Settings** on the left.

7. On the **Routes** blade, select the **+ Add** button. Enter the following information, and select **Add**:

    - Route name: **OnPremToAppSubnet**

    - Address prefix destination: **IP Addresses**

    - Address prefix: **10.8.0.0/25**

    - Next hop type: **Virtual appliance**

    - Next hop address: **10.7.1.4**

    ![In this screenshot, the 'Add route' blade of the 'WGAzureVNetGWRT' route table is depicted with the required settings listed above selected along with the OK button.](images/hol-ex8-task2-onpremtoappsubnet-to-virtual-appliance.png "Add route")

8. On the **WGAzureVNetGWRT - Routes** blade, select **Subnets** under **Settings** on the left.

9. On the **Subnets** blade, select **+ Associate**.

10. On the **Associate subnet** blade, select **WGVNet1** under the **Virtual Network** drop down and select **GatewaySubnet** under the **Subnet** drop down.

    ![In this screenshot, the 'Associate subnet' blade of the 'WGAzureVNetGWRT' route table is depicted with the required WGVNet1 virtual network and GatewaySubnet subnet selected along with the OK button.](images/hol-ex8-task2-associate-gatewaysubnet.png "Associate subnet")

    >**Note:** At this point, you have configured your enterprise network. You should be able to test your Enterprise Class Network from one region to another. Your testing can include the following scenarios:

    - On the 'on-premises' virtual machine (OnPremVM), attempt to initiate a Remote Desktop session to any virtual machine on the AppSubnet (10.8.0.0/25). Note that this should fail since it is blocked by Azure Firewall.

    - In the Azure portal, navigate to and browse to the web application deployed to the WGVnet2 via the private IP address of the Azure Load Balancer(10.8.0.100). Note that this traffic is routed (and allowed) via Azure Firewall.

    - In the Azure portal, navigate to the WGWEB1 VM and initiate a Bastion connection session to the WGWEB1 virtual machine by selecting **Connect** and **Bastion**. This should be successful since it is allowed by Azure Firewall and Azure Bastion Host.

    - In the Azure portal, navigate to the WGWEB1 VM and initiate a Bastion connection session to the WGWEB2 virtual machine by selecting **Connect** and **Bastion**. This should be successful since it is allowed by Azure Firewall and Azure Bastion Host.

    - From within the WGWEB1 VM Bastion connection session, initiate a Remote Desktop session to the WGSQL1 via its private IP address (10.8.1.4). This should be successful since it is allowed by Azure Firewall.

## Exercise 9: Create a Network Monitoring Solution

Duration: 15 minutes

### Task 1: Create a Log Analytics Workspace

1. Connect to the Azure portal. Select **+ Create a resource**, and in the **Search the Marketplace** box, search for and select **Log Analytics Workspace**. Select **Create**.

2. On the **Create workspace** blade, enter the following information:

    - Subscription: **Select your subscription**.

    - Resource group: Select **Create new**, and enter the name **MonitoringRG**.

    - Name: **Enter a unique name in all lowercase**

    - Location: **East US**

3. Upon completion, it should look like the following screenshot. Validate the information is correct, and select **Review + create** then **Create**.

    ![In this screenshot, the 'Create Log Analytics workspace' blade is depicted with the required settings listed in the previous step and the 'Review + Create' button selected.](images/hol-ex9-task1-create-log-analytics-workspace.png "Create Log Analytics workspace")

### Task 2: Configure Network Watcher

1. Connect to the Azure portal. Select **All Services** on the left navigation, and in the Category list, select **Networking** followed by selecting **Network Watcher**.

    ![In this screenshot, the 'All services' blade of the Azure portal is depicted with the Networking category selected on the left and 'Network watcher' selected on the list.](images/hol-ex9-task2-services-network-watcher.png "All Services blade")

2. In the **Overview** blade, ensure that **NetworkWatcher_southcentralus** and **NetworkWatcher_eastus** is listed.

3. If they are not listed, add them to the list using the **+ Add** button.

   ![In this screenshot, the 'Overview' blade of the 'Network Watcher' service is depicted with the available regions listed and the '+ Add' button selected.](images/hol-ex9-task2-network-watcher.png "Network Watcher Overview blade")

## Exercise 10: Using Network Watcher to Test and Validate Connectivity

Duration: 60 minutes

In this exercise, you will collect the flow log and perform connectivity from your simulated on-premises environment to Azure. This will be accomplished by using the Network Watcher Service in the Azure Platform.

### Task 1: Configuring the Storage Account for the NSG Flow Logs

1. On the Azure portal select **+ Create a resource**. Select **Storage account**. Select **Create**.

2. On the **Create a storage account** blade. Enter the following information, and select **Review + Create** then select the **Create** button:

    - Subscription: **Your Subscription**

    - Resource Group: **MonitoringRG** (Use the existing resource group created earlier.)

    - Storage Account Name: **This must be Unique and alphanumeric, lowercase, and have no special characters.**

    - Location: **(US) South Central US**

    - Performance: **Standard**

    - Account Kind: **StorageV2 (general purpose v2)**

    - Replication: **Locally-redundant storage (LRS)**

    ![In this screenshot, the 'Create storage account' blade is depicted with the above required settings selected along with the 'Review + create' button.](images/hol-ex10-task1-create-storage-account-monitoring.png "Add storage account")

   >**Note:** Ensure the storage account is created before continuing.

3. Repeat steps 1 and 2, but select **East US** for the region and give it a different name.

4. On the Azure portal select **All services** at the left navigation. From the Categories menu select **Networking** then select **Network Watcher**.

5. From the **Network Watcher** blade under the **Logs** menu on the left, select **NSG flow logs**. Select **+ Create**.

    ![In this screenshot, the 'NSG Flow logs blade is depicted with the '+ Create' button selected.](images/hol-ex10-task1-nsg-flow-logs-create-button.png "Network Security Groups in Flow Log")

6. In the **Create a flow log** blade that appears, enter the following information then select **Next: Configuration**.

    - Subscription: **Select your subscription**

    - Network Security Group: **WGAppNSG1**

    - Storage Accounts: **The available storage account that you created earlier**.

    - Retention (days): **0**

    ![In this screenshot, the Basics tab of the 'Create a flow log' blade is depicted with the required settings listed above selected along with the 'Next: Configuration' button selected.](images/hol-ex10-task1-create-flowlog-wgappnsg1.png "Create a flow log Basics")

7. On the **Configuration** tab of the **Create a flow log** blade, enter the following information then select **Review + create** then **Create**.

    - Flow Logs Version: **Version 2**

    - Enable Traffic Analytics: **Checked**

    - Traffic Analytics processing interval: **Every 10 minutes**

    - Log Analytics Workspace: **The log analytics workspace you created earlier**

8. Repeat Steps 5 - 7 to create a flow log for the **OnPremVM-nsg** Network Security Group as well. When completed your **NSG flow logs** blade on **Network Watcher** should look like what's depicted in the below image.

     ![In this screenshot, the 'Network Watcher - NSG flow logs' blade is depicted with the two flow logs created earlier listed.](images/hol-ex10-task1-nsg-flow-logs-blade.png "Network Watcher Flow Log")

9. Navigate back to the **OnPremVM**. Connect to it by downloading and opening the RDP file.

10. Open the **Microsoft Edge** browser from the Start menu. Navigate to portal.azure.com.  Login to the **Azure** portal.

11. In the RDP session for the **OnPremVM**, navigate to the **Azure** portal, navigate to **Virtual machines** and select the **WGWEB1**. Connect to **WGWEB1** through **Bastion**.  In **WGWEB1**,  navigate to the load balancer's private ip address (**10.8.0.100**) and generate some traffic by refreshing the browser. Allow ten minutes to pass for traffic analytics to generate.

     ![In this screenshot, the RDP connections to OnPremVM and WGWEB1 are depicted with the load balancer connection open.](images/hol-ex10-task1-web-traffic-over-load-balancer.png "CloudShop Application")

### Task 2: Configuring Diagnostic Logs

1. On the Azure portal, select **All services** at the left navigation. From the Categories menu select **Networking**, then **Network Watcher**,

2. Select **Diagnostic Logs** from the **Logs Menu** within the blade.

3. Select **onpremvm*NNN*** then select **+Add diagnostic setting**.

     ![In this screenshot, the Diagnostics logs blade of Network Watch is depicted. The 'Diagnostic Logs' menu option and the network interface resource that starts with 'onpremvm' is highlighted.](images/hol-ex10-task2-diagnostic-logs-navigation.png "Network Watcher Diagnostic logs")

4. Enter **OnPremDiag** as the name then select the checkbox for **Archive to a storage account**. On the **Storage accounts** drop down, select the available storage account you created earlier.

5. Select the **Send to Log Analytics workspace** checkbox. Select the workspace created earlier in the dropdown. Select the **AllMetrics** checkbox and set the **Retention (days)** to **60**. Select the **Save** button to complete the settings.

     ![In this screenshot, the 'Diagnostic setting' blade of the Azure portal is depicted with the required settings listed above selected.](images/hol-ex10-task2-diagnostic-setting-blade.png "Diagnostic Settings")

6. Repeat Steps 2 - 5 for each network resource. Once completed your settings will look like the following screenshot.

     ![In this screenshot, the 'Network Watcher - Diagnostic logs' blade is depicted with all the network resources having a 'Diagnostic status' of 'Enabled'.](images/hol-ex10-task2-diagnostic-logs.png "Diagnostic Settings")

### Task 3: Reviewing Network Traffic

1. On the Azure portal select **All services** at the left navigation. From the Categories menu select **Networking** then select **Network Watcher**.

2. Select **Traffic Analytics** from the **Logs** menu in the blade. At this time, the diagnostic logs from the network resources have been ingested. Select **View map**.

     ![In this screenshot, the 'Network Watcher - Traffic Analytics' blade is depicted is depicted with the 'View map' button selected.](images/hol-ex10-task3-network-watcher-traffic-analytics.png "Your Network Environment")

3. Select the **green check mark** which identifies your network. Within the pop-up menu select **More Details** to propagate detailed information of the flow to and from your network.

     ![In this screenshot, the Traffic Analytics Geo Map View is depicted with a green check mark on the region where your network resides and monitor metrics displayed next to it.](images/hol-ex10-task3-network-watcher-traffic-analytics-map.png "Your Network Environment")

>**Note:** You can select the **See More** link to query the connections detail for more information.

### Task 4: Network Connection Troubleshooting

1. On the Azure portal select **All services** at the left navigation. From the Categories menu select **Networking** then select **Network Watcher**.

2. Select **Connection Troubleshoot** from the **Network Diagnostic tools** menu.

3. To troubleshoot a connection or to validate the route enter the following information and select **Check**:

    - Subscription: **Your Subscription**

    - Resource Group: **OnPremVMRG**

    - Source Type: **Virtual Machine**

    - Virtual Machine: **OnPremVM**

    - Destination: **Select a virtual machine**

    - Resource Group: **WGVNetRG2**

    - Virtual Machine: **WGWEB1**

    - Protocol: **TCP**

    - Destination Port: **80**

     ![In this screenshot, the 'Network Watcher - Connection troubleshoot' blade is depicted with the required settings listed above selected.](images/hol-ex10-task4-network-watcher-connection-troubleshoot-settings.png "Connection Troubleshoot")

4. Once the check is complete the Connection troubleshoot feature will display a grid view of the Name, IP address, Status, and Next hop as seen in the following screenshot.

     ![In this screenshot, the results of the connection troubleshoot operation performed in the previous step is depicted with a grid view showing the Name, IP address, Status, and Next hop IP address.](images/hol-ex10-task4-network-watcher-connection-troubleshoot.png "Connection Troubleshoot")

## After the hands-on lab

Duration: 10 minutes

After you have successfully completed the Enterprise-class networking in Azure hands-on lab step-by-step, you will want to delete the Resource Groups created. This will free up your subscription from future charges.

The resource groups that were called out in this lab include:

- MonitoringRG
- OnPremVMRG
- OnPremVNetRG
- WGVNetRG1
- WGVNetRG2

You should follow all steps provided *after* attending the Hands-on lab.
