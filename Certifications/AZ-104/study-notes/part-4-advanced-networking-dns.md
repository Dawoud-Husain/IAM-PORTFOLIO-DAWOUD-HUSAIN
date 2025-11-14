# AZ-104 Study Notes - Part 4: Advanced Networking & DNS

## Source
John Savill AZ-104 Administrator Associate Study Cram v2 - Part 4 of 8

---

## Topics Covered

### VNet Peering Advanced Concepts

- [ ] ❌ **Peering Non-Transitivity**
  - VNet peering is NOT transitive
  - If Hub peers to Spoke1 and Spoke2, Spoke1 cannot talk to Spoke2
  - Must create explicit peering between spokes for direct communication
  - **Workaround**: Route through Azure Firewall or NVA with User-Defined Routes (UDRs)

- [ ] 🔀 **Spoke-to-Spoke Communication**
  - Option 1: Create direct peering between spokes
  - Option 2: Use Azure Firewall/NVA in hub as next hop
  - Requires User-Defined Routes (UDRs) telling spokes to route via firewall
  - Firewall/NVA forwards traffic between spokes

### Azure Virtual Network Manager

- [ ] 🎯 **Azure Virtual Network Manager Overview**
  - Centrally manage virtual networks at scale
  - Create network groups (static or dynamic)
  - Define connectivity configurations
  - Apply security admin rules
  - Visual topology view

- [ ] 👥 **Network Groups**
  - **Static**: Manually add VNets to group
  - **Dynamic**: Rule-based membership (criteria-driven)
  - VNet can belong to multiple groups
  - Easier than manually managing individual peerings

- [ ] 🔗 **Connectivity Configurations**
  - **Hub-and-Spoke**: Central hub with gateway, spokes radiate outward
  - **Mesh**: Any-to-any connectivity within region
  - Mesh uses clever technology (not traditional peerings)
  - Regional any-to-any connectivity in mesh

- [ ] 🛡️ **Security Admin Rules**
  - Apply BEFORE network security groups (NSGs)
  - **Allow**: Traffic flows to NSG for evaluation
  - **Always Allow**: Bypass NSG completely (even if NSG denies)
  - **Deny**: Block traffic immediately (never reaches NSG)
  - Use case: Ensure critical services (domain controllers, patching) always accessible
  - Prevents local admins from accidentally blocking critical traffic

- [ ] 📊 **Rule Processing Order**
  - 1. Security Admin Rules (Virtual Network Manager)
  - 2. Network Security Groups (local subnet/NIC level)
  - Funnel model: Traffic flows through Security Admin Rules first

### Network Security Groups (NSGs)

- [ ] 🔒 **NSG Fundamentals**
  - Set of security rules (firewall rules)
  - Apply to subnet or network interface
  - Must be in same region as VNet
  - Inbound and outbound rules

- [ ] 📋 **NSG Rule Attributes**
  - **Priority**: 1-65500 (lower number = higher priority, 1 is highest)
  - **Name**: Descriptive rule name
  - **Source**: IP, service tag, or application security group
  - **Destination**: IP, service tag, or application security group
  - **Ports**: Port range or specific ports
  - **Protocol**: TCP, UDP, ICMP, Any
  - **Action**: Allow or Deny

- [ ] 🏷️ **Service Tags**
  - Represent IP address ranges for Azure services
  - Azure manages and updates automatically
  - **Examples**: Internet, VirtualNetwork, AzureLoadBalancer, Storage, SQL, AppService
  - Regional variants: Storage.EastUS, AppService.WestEurope
  - Eliminates need to track changing Azure IP ranges
  - Available for both source and destination (depends on direction)

- [ ] 🏢 **Application Security Groups (ASGs)**
  - Tags applied to network interfaces
  - Logical grouping of VMs by role (web servers, database servers, etc.)
  - Rules reference ASG instead of IP addresses
  - **Example**: "WebServers can talk to DatabaseServers on port 1433"
  - Must be in same region as NSG
  - More flexible than IP-based rules

- [ ] 📊 **Default NSG Rules**
  - **Outbound**: VirtualNetwork-to-VirtualNetwork allowed, Internet allowed, all else denied
  - **Inbound**: VirtualNetwork-to-VirtualNetwork allowed, AzureLoadBalancer allowed, all else denied
  - Default rules have lowest priority (65000-65500)
  - Cannot delete default rules, can override with higher priority

- [ ] 🔗 **NSG Association**
  - Associate to subnet (applies to all resources in subnet)
  - Associate to network interface (applies to specific resource)
  - Can have both subnet-level and NIC-level NSGs

- [ ] 🔍 **Effective Security Rules**
  - View actual rules applying to a NIC
  - Shows combined subnet + NIC rules
  - Portal: VM → Networking → Effective security rules

### User-Defined Routes (UDRs)

- [ ] 🗺️ **Effective Routes**
  - View all routes a NIC is receiving
  - Includes VNet routes, peering routes, gateway routes, UDRs
  - Portal: VM → Networking → Effective routes

- [ ] 🎯 **Next Hop**
  - Destination for traffic matching route
  - Options: Virtual appliance, VNet gateway, Internet, VNet, None (drop)
  - Used with Azure Firewall, NVA for centralized routing

### Azure Firewall

- [ ] 🔥 **Azure Firewall Overview**
  - First-party Microsoft network virtual appliance (NVA)
  - Fully managed firewall service
  - Inbound DNAT (Destination NAT)
  - Outbound SNAT (Source NAT)
  - Layer 4 (TCP/UDP) and Layer 7 (application) filtering

- [ ] 📦 **Azure Firewall SKUs**
  - **Basic**: Low performance, basic features
  - **Standard**: Up to 30 Gbps, category filtering, DNS proxy
  - **Premium**: Up to 100 Gbps, TLS termination, IDPS, URL filtering
  - Choose based on performance and feature requirements

- [ ] 🎯 **Azure Firewall Rule Types**
  - **Network Rules**: Layer 4 (TCP/UDP) filtering by IP, port, protocol
  - **Application Rules**: Layer 7 (HTTP/HTTPS) filtering by FQDN, URL
  - **NAT Rules**: Inbound DNAT for publishing services

- [ ] 🚀 **Azure Firewall Premium Features**
  - **TLS Termination**: Inbound and outbound SSL/TLS inspection
  - **IDPS**: Intrusion Detection and Prevention System (fully managed)
  - **URL Filtering**: Filter based on URL path (requires SSL termination)
  - **Category Filtering**: Block by category (malware, phishing, etc.)

- [ ] 🔧 **Azure Firewall Implementation**
  - Requires User-Defined Routes (UDRs) to send traffic to firewall
  - Typically deployed in hub VNet
  - Spokes route traffic via UDRs to firewall

### Azure DNS

#### Public DNS Zones

- [ ] 🌍 **Public DNS Zone**
  - Authoritative DNS zone for internet-accessible domains
  - Host A, AAAA, CNAME, MX, TXT, SRV, NS records
  - Azure manages name servers
  - Low cost, high availability

- [ ] 🔗 **Alias Records**
  - Point to Azure resources directly
  - **Prevents dangling DNS**: If resource deleted, record becomes empty set
  - Prevents DNS hijacking (attacker can't claim deleted resource name)
  - Supported for: A, AAAA, CNAME records (not MX)
  - Use instead of standard CNAME for Azure resources

- [ ] ⚠️ **Dangling DNS Problem**
  - Standard record points to deleted resource
  - Attacker creates resource with same name
  - Your DNS now points to attacker's resource
  - **Solution**: Use alias records

- [ ] 📊 **Supported Public Record Types**
  - A (IPv4), AAAA (IPv6), CNAME, MX, TXT, SRV, NS, PTR, SOA
  - Alias support: A, AAAA, CNAME only

#### Private DNS Zones

- [ ] 🔒 **Private DNS Zone**
  - DNS resolution within virtual networks
  - Not accessible from internet
  - Global resource (link to VNets in any region/subscription)
  - Requires appropriate permissions to link

- [ ] 🔄 **VNet Linking Types**
  - **Auto-Registration**: VNet resources automatically register DNS records
    - VNet can auto-register to only ONE private DNS zone
    - Private DNS zone can have up to 100 VNets for auto-registration
  - **Resolution**: VNet can query DNS zone
    - VNet can link to up to 1000 private DNS zones for resolution
    - Private DNS zone can be used by 1000 VNets for resolution

- [ ] 🏷️ **Automatic Registration**
  - Resources in linked VNet automatically create DNS records
  - Records updated when resource IP changes
  - Records deleted when resource deleted
  - Eliminates manual DNS management

- [ ] 🌐 **Default Internal DNS**
  - Default zone: internal.cloudapp.net
  - Cannot manually add records to default zone
  - Auto-generated for VNet resources

- [ ] 🔍 **Azure DNS IP Address**
  - Azure DNS accessible at: **168.63.129.16**
  - Only accessible from within VNet
  - Resources automatically configured to use this

#### Azure Private DNS Resolver

- [ ] 🔌 **Private DNS Resolver**
  - Enables on-premises to resolve Azure private DNS zones
  - Enables Azure resources to resolve on-premises DNS
  - **Inbound endpoint**: On-premises queries Azure private zones
  - **Outbound endpoint**: Azure queries on-premises DNS servers
  - Hybrid DNS solution

- [ ] 🏢 **Use Cases**
  - On-premises servers need to resolve Azure private link endpoints
  - Azure VMs need to resolve corporate DNS zones
  - Bidirectional DNS resolution between Azure and on-premises

- [ ] 🔧 **Custom DNS Servers**
  - Can configure custom DNS servers at VNet level
  - Configured via DHCP to resources
  - Ensure custom DNS can still resolve 168.63.129.16 if needed
  - May need forwarding rules for Azure DNS zones

#### Split-Brain DNS

- [ ] 🧠 **Split-Brain DNS Concept**
  - Same domain name, different records internally vs externally
  - **Public zone**: External resolution (internet-facing IPs)
  - **Private zone**: Internal resolution (private IPs)
  - Common for hybrid environments

### Hybrid Connectivity

#### VPN Gateway

- [ ] 🔐 **VPN Gateway Overview**
  - Encrypted connectivity over internet
  - Connects Azure VNet to on-premises network
  - Requires Gateway subnet (minimum /29, recommended /27)
  - Gateway subnet name must be "GatewaySubnet"

- [ ] 📋 **VPN Gateway Types**
  - **Policy-Based (Static Routing)**:
    - Legacy, not recommended
    - Only 1 tunnel
    - Basic SKU only
    - Very restrictive
    - Only if on-premises device requires it
  - **Route-Based (Dynamic Routing)**:
    - Recommended for all scenarios
    - N number of tunnels
    - Supports Point-to-Site VPN
    - Active-passive or active-active modes
    - Modern, flexible

- [ ] 🏢 **Site-to-Site VPN**
  - Connect on-premises network to Azure VNet
  - Private IP space to private IP space
  - Goes over internet (encrypted)
  - On-premises requires VPN server/gateway
  - Azure side uses VPN Gateway

- [ ] 👤 **Point-to-Site VPN**
  - Individual computer connects to Azure VNet
  - Remote worker connectivity
  - Only available with route-based VPN
  - Multiple protocols: IKEv2, OpenVPN, SSTP

- [ ] 🔄 **VPN Gateway High Availability**
  - **Active-Passive**: Default, one gateway active
  - **Active-Active**: Both gateways active, two tunnels
  - Can have redundant on-premises gateways
  - Multiple configurations possible for resiliency

- [ ] ⚡ **VPN Gateway Characteristics**
  - Connectivity over public internet
  - IPsec/IKE VPN tunnel
  - Encrypted traffic
  - Variable latency (internet-dependent)
  - Lower cost than ExpressRoute

#### ExpressRoute

- [ ] 🚀 **ExpressRoute Overview**
  - Private connectivity to Azure (not over internet)
  - Connects to Microsoft backbone network
  - More reliable, lower latency, higher bandwidth than VPN
  - Higher cost than VPN

- [ ] 🌐 **Microsoft Backbone**
  - Microsoft's global private network
  - All Azure regions connected to it
  - Resilient, redundant regional network gateways

- [ ] 🏢 **Peering Points (Meet-Me Points)**
  - Neutral facilities where networks interconnect
  - Carrier hotels, points of presence (PoPs)
  - Microsoft extends network to these locations
  - Customer extends network to same location
  - Cross-connect between customer and Microsoft

- [ ] 🔌 **ExpressRoute Circuit**
  - Logical connection at peering point
  - Represents customer's private connection to Microsoft
  - Different circuits for different locations
  - Can have multiple circuits

- [ ] 🔒 **ExpressRoute Private Peering**
  - Connect on-premises private IP space to Azure VNet private IP space
  - VNet uses ExpressRoute Gateway
  - Gateway connects to ExpressRoute circuit(s)
  - Known routes shared via BGP

- [ ] 🌍 **ExpressRoute Global Reach**
  - Connect on-premises locations to each other via Microsoft backbone
  - Uses ExpressRoute circuits for inter-site connectivity
  - Alternative to your own WAN links
  - Traffic routes over Microsoft's private network
  - **Different from private peering**: Global Reach = location-to-location, Private Peering = location-to-Azure

- [ ] 🔀 **ExpressRoute Gateway Multi-Circuit**
  - ExpressRoute Gateway can connect to multiple circuits
  - Different weightings for different circuits
  - BGP path prepending for route preference
  - Failover between circuits if one fails

- [ ] 🔧 **ExpressRoute Connectivity Options**
  - **Carrier/Service Provider**: Provider handles connectivity to peering point
  - **Direct Connectivity**: Your own connection to peering point (large enterprises)
  - **MPLS**: Multi-Protocol Label Switching provider

- [ ] ⚖️ **VPN + ExpressRoute Coexistence**
  - Can have both VPN Gateway and ExpressRoute Gateway in same VNet
  - Requires /27 or larger Gateway subnet
  - VPN as backup to ExpressRoute
  - Different use cases for each

---

## Key Takeaways

1. **VNet Peering** - NOT transitive; spokes can't talk to each other without UDRs or direct peering
2. **Azure Virtual Network Manager** - Security Admin Rules apply BEFORE NSGs (Always Allow bypasses NSG)
3. **NSG Rule Priority** - Lower number = higher priority (1 is best, 65500 is worst)
4. **Service Tags** - Azure-managed IP ranges, eliminates manual tracking
5. **Application Security Groups** - Tag NICs by role, rules reference tags not IPs
6. **Azure Firewall SKUs** - Basic/Standard/Premium (Premium has TLS termination, IDPS, URL filtering)
7. **Alias Records** - Prevent dangling DNS and DNS hijacking
8. **Auto-Registration** - VNet can auto-register to only ONE private DNS zone
9. **Azure DNS IP** - 168.63.129.16 (only accessible from within VNet)
10. **VPN Gateway** - Use route-based (not policy-based); minimum /29 subnet, recommend /27
11. **ExpressRoute Global Reach** - Location-to-location via Microsoft backbone
12. **Private Peering vs Global Reach** - Private Peering = on-prem to Azure; Global Reach = on-prem to on-prem

---

## Exam Tips

- **Peering Transitivity**: Common exam trap - remember spokes can't talk to spokes without explicit config
- **Security Admin Rules**: "Always Allow" bypasses NSG (critical concept)
- **NSG Priority**: Remember lower number = higher priority (opposite of some systems)
- **Service Tag Availability**: Not all service tags available for both source/destination (depends on direction)
- **ASG Region**: Must be in same region as NSG
- **Alias Record Types**: Only A, AAAA, CNAME (NOT MX)
- **Auto-Registration Limits**: 1 zone per VNet for registration, 100 VNets per zone
- **Resolution Limits**: 1000 zones per VNet, 1000 VNets per zone
- **Gateway Subnet**: Must be named "GatewaySubnet" exactly
- **Policy-Based VPN**: Only 1 tunnel, legacy, basic SKU (avoid unless required)
- **Route-Based VPN**: N tunnels, supports Point-to-Site
- **ExpressRoute Global Reach**: Different from Private Peering (location-to-location vs location-to-Azure)
- **Coexistence**: VPN + ExpressRoute requires /27 Gateway subnet (not /29)

---

## Portal Navigation

- **Virtual Network Manager**: Search "Network Manager" → Create → Define groups and configurations
- **NSG**: Search "Network security groups" → Create → Add inbound/outbound rules
- **Application Security Groups**: Search "Application security groups" → Create
- **Azure Firewall**: Search "Firewalls" → Create → Choose SKU
- **Public DNS Zone**: Search "DNS zones" → Create → Add record sets
- **Private DNS Zone**: Search "Private DNS zones" → Create → Virtual network links
- **VPN Gateway**: Virtual Network → Gateway subnet → Create VPN Gateway
- **ExpressRoute**: Search "ExpressRoute circuits" → Create
- **Effective Routes**: VM → Networking → Network interface → Effective routes
- **Effective Security Rules**: VM → Networking → Effective security rules

---

## Key IP Addresses & Ranges

- **Azure DNS**: 168.63.129.16 (accessible only from VNet)
- **Gateway Subnet**: Minimum /29, recommended /27
- **VPN Coexistence Subnet**: Minimum /27 (for VPN + ExpressRoute)

---

## PowerShell/CLI Commands

```powershell
# NSG
New-AzNetworkSecurityGroup
New-AzNetworkSecurityRuleConfig
Get-AzEffectiveNetworkSecurityGroup

# Routes
Get-AzEffectiveRouteTable

# DNS
New-AzDnsZone (public)
New-AzPrivateDnsZone
New-AzPrivateDnsVirtualNetworkLink

# VPN Gateway
New-AzVirtualNetworkGateway -GatewayType Vpn
New-AzLocalNetworkGateway

# ExpressRoute
New-AzExpressRouteCircuit
New-AzVirtualNetworkGateway -GatewayType ExpressRoute
```

---

## Decision Trees

### NSG Rule Source/Destination Selection

```
Need to allow Azure service?
├─ Yes → Use Service Tag (e.g., Storage, SQL)
└─ No → Is it based on VM role?
    ├─ Yes → Use Application Security Group
    └─ No → Use IP address or CIDR
```

### Hybrid Connectivity Selection

```
Need private connectivity to Azure?
├─ Predictable latency required?
│   ├─ Yes → ExpressRoute
│   └─ No → VPN Gateway acceptable
├─ Budget constraint?
│   ├─ Tight budget → VPN Gateway
│   └─ Higher budget → ExpressRoute
└─ Bandwidth requirement?
    ├─ High (>1 Gbps) → ExpressRoute
    └─ Low (<1 Gbps) → VPN Gateway

Connect multiple on-prem sites?
└─ Via Azure → ExpressRoute Global Reach
```
