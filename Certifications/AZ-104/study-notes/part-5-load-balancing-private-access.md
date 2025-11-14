# AZ-104 Study Notes - Part 5: Load Balancing & Private Access

## Source
John Savill AZ-104 Administrator Associate Study Cram v2 - Part 5 of 8

---

## Topics Covered

### ExpressRoute Advanced Features

- [ ] 💰 **ExpressRoute Pricing**
  - Based on circuit bandwidth
  - Local vs Standard vs Premium SKUs
  - Metered vs Unlimited data plans

- [ ] 🌟 **ExpressRoute Premium**
  - Connect to regions outside geopolitical boundary
  - Use Microsoft 365 services over ExpressRoute
  - Larger route table (more routes advertised)
  - Required for very large, complex networks
  - Higher cost than Standard

- [ ] 📡 **Microsoft Peering**
  - Access Azure PaaS services directly (not through VNet)
  - Examples: Storage accounts, databases
  - Uses BGP to advertise routes
  - **Route Filter**: Specify which services to advertise to on-premises
  - Traffic goes through ExpressRoute circuit to PaaS public endpoints
  - Alternative to Private Endpoints for some scenarios

- [ ] 🔄 **ExpressRoute Backup Options**
  - Second ExpressRoute circuit in different location (natural disaster protection)
  - Site-to-Site VPN as backup path
  - Can coexist VPN + ExpressRoute

### Azure Virtual WAN

- [ ] 🌐 **Virtual WAN Overview**
  - Managed hub-and-spoke networking solution
  - "Someone else does it" - Microsoft manages connectivity
  - Regional construct with hubs
  - Simplifies complex network topologies

- [ ] 📦 **Virtual WAN SKUs**
  - **Basic**:
    - Site-to-Site VPN only
    - Lower cost
    - Limited functionality
  - **Standard**:
    - ExpressRoute support
    - Point-to-Site VPN
    - Site-to-Site VPN
    - VNet-to-VNet transitive connectivity (automatic!)
    - Hub-to-Hub connectivity
    - Secured Azure Virtual WAN (Azure Firewall integration)
    - Custom NVA deployment support

- [ ] ✅ **Virtual WAN Benefits**
  - Automatic transitive routing (unlike manual VNet peering)
  - Centralized management
  - Built-in security options
  - Standard is typical choice for most scenarios
  - Consumption-based pricing (pay for what you use)

### Service Endpoints vs Private Endpoints

#### Service Endpoints

- [ ] 🔌 **Service Endpoint Basics**
  - Enable subnet to access PaaS services securely
  - Service still has public endpoint
  - Makes subnet a "known entity" to the service
  - More direct communication path
  - Private IP shows in service logs

- [ ] 🎯 **Service Endpoint Configuration**
  - Enable on subnet for specific service type (Storage, SQL, etc.)
  - Service firewall allows specific VNet/subnet
  - Only applies to resources in that specific subnet
  - Other subnets must have own service endpoints

- [ ] 📍 **Service Endpoint Use Cases**
  - Storage accounts: Allow only specific subnets
  - SQL databases: Restrict access to VNet resources
  - Key Vault: Limit access to specific networks
  - Still uses public endpoint (secured via firewall)

- [ ] ⚠️ **Service Endpoint Limitations**
  - Only works for resources in the configured subnet
  - Cannot use from on-premises (public endpoint still)
  - Cannot use from peered VNets without additional configuration
  - Service still has public endpoint (just firewalled)

#### Private Endpoints

- [ ] 🔐 **Private Endpoint Basics**
  - Creates private IP address in your subnet
  - Connects to specific instance of PaaS service
  - **Public endpoint can be completely disabled**
  - Uses Azure Private Link technology
  - Traffic stays on Microsoft backbone

- [ ] 🌟 **Private Endpoint Advantages**
  - Works from anywhere private IP is routable (on-prem, peered VNets, other subnets)
  - No public endpoint required
  - Eliminates data exfiltration risk
  - Better security than service endpoints
  - Just an IP address in your VNet

- [ ] 🔧 **Private Endpoint DNS Requirements**
  - Requires DNS configuration
  - Create private DNS zone (e.g., privatelink.blob.core.windows.net)
  - DNS record points service name to private IP
  - TLS certificate validation still works (proper FQDN)
  - DNS must resolve consistently everywhere (on-prem + Azure)

- [ ] 📊 **Service Endpoint vs Private Endpoint**
  - **Service Endpoint**: Subnet-level, public endpoint remains, subnet firewall rules
  - **Private Endpoint**: IP address in VNet, can disable public, works from anywhere with routing

#### Private Link Service

- [ ] 🔗 **Private Link Service**
  - Expose your own services via Private Link
  - Requires Standard Load Balancer
  - Consumers create private endpoints to your service
  - NAT (Network Address Translation) between networks
  - No VNet peering required

- [ ] 🎯 **Private Link Service Use Cases**
  - Overlapping IP address spaces (can't peer VNets)
  - Don't want to peer VNets (security isolation)
  - Provide service to customers without peering
  - Only expose specific service (frontend IP), not entire VNet

- [ ] ⚙️ **Private Link Service Configuration**
  - Deploy Standard Load Balancer (internal)
  - Add Private Link Service to load balancer
  - Consumers create private endpoints pointing to your service
  - Standard LB frontend IP becomes accessible via private endpoint

### Azure Bastion

- [ ] 🖥️ **Azure Bastion Overview**
  - Managed jump box service
  - RDP/SSH access to VMs without public IPs
  - Access from internet via Azure portal or CLI
  - Integrates with Entra ID
  - Supports conditional access policies

- [ ] 🔐 **Azure Bastion Security**
  - No public IP needed on VMs
  - RDP/SSH over SSL (port 443)
  - Entra authentication integration
  - Conditional access: MFA, device compliance, etc.
  - No need for NSG rules to allow RDP/SSH from internet

- [ ] 📦 **Azure Bastion SKUs**
  - **Basic/Developer**:
    - VMs in same VNet only
    - Portal-based access
    - Lower cost
  - **Basic**:
    - VMs in peered VNets
    - Portal-based access
  - **Standard**:
    - VMs in peered VNets
    - Cross-platform: RDP to Linux, SSH to Windows
    - Azure CLI connectivity (not just portal)
    - Better scaling
    - Shareable links
    - Disable copy/paste for security
    - Session recording

- [ ] 🏗️ **Azure Bastion Deployment**
  - Deploys into dedicated subnet: "AzureBastionSubnet"
  - Subnet size: /26 (64 IPs)
  - Must be named exactly "AzureBastionSubnet"
  - One Bastion per VNet (can access peered VNets)

### Azure Load Balancer

- [ ] ⚖️ **Load Balancer Overview**
  - Layer 4 load balancing (TCP/UDP)
  - Regional service (within single region)
  - High availability for backend services
  - Internal or external (public-facing)

- [ ] 🏗️ **Load Balancer Components**
  - **Frontend IP**: IP address clients connect to (public or private)
  - **Backend Pools**: Resources to distribute traffic to
  - **Health Probes**: Check backend health (HTTP, HTTPS, TCP)
  - **Load Balancing Rules**: Define traffic distribution
  - **Inbound NAT Rules**: Forward specific ports to specific backends
  - **Outbound Rules**: Define outbound internet access (Standard only)

- [ ] 📦 **Load Balancer SKUs - FREE (Basic)**
  - **DEPRECATED**: Retiring September 30, 2025
  - Maximum 300 backends (in availability sets)
  - No SLA
  - NICs only in backend (no IP addresses)
  - No availability zone support
  - **DO NOT USE** - migrating to Standard required

- [ ] ⭐ **Load Balancer SKUs - Standard**
  - **Recommended**: Use for all deployments
  - Maximum 1,000 backends
  - SLA provided
  - Availability zone support (zone-redundant or zonal)
  - Backend pool: NICs OR IP addresses
  - Locked down by default (need outbound rules for internet)
  - Must use Standard Public IP (if external)
  - Paid resource

- [ ] 🎯 **Load Balancing Rules - Tuples**
  - **5-tuple**: Destination IP + Source IP + Destination Port + Source Port + Protocol
    - Most specific, same client always to same backend
  - **3-tuple**: Destination IP + Source IP + Protocol
    - Remove ports from equation
  - **2-tuple**: Destination IP + Source IP
    - Remove protocol, most flexible
  - Choose based on session persistence requirements

- [ ] 🔍 **Health Probes**
  - **HTTP/HTTPS**: Check specific URL path (Standard adds HTTPS)
  - **TCP**: Check if port is open
  - Determines backend availability
  - Unhealthy backends removed from rotation

- [ ] 🌐 **Floating IP**
  - Backend sees frontend IP (not its own IP)
  - Avoids IP rewriting
  - Useful for SQL Always On, multi-IP scenarios
  - Backend must be configured to accept frontend IP

- [ ] 🔒 **Backend Pool - Standard Features**
  - NICs from VMs/VMSS in same VNet
  - IP addresses (useful for containers/AKS pods)
  - All backends must be in same VNet as load balancer
  - AKS pods don't have NICs, only IPs (requires Standard)

- [ ] 📤 **Outbound Rules (Standard Only)**
  - Define how backend resources access internet
  - Required if no public IP on backend
  - Uses SNAT (Source Network Address Translation)
  - Frontend public IP used for outbound connections

### Application Gateway

- [ ] 🌐 **Application Gateway Overview**
  - Layer 7 load balancing (HTTP/HTTPS/HTTP2/WebSocket)
  - Regional service
  - Web application focused
  - Advanced routing capabilities
  - SSL/TLS termination

- [ ] 🏗️ **Application Gateway Versions**
  - **V1**: Fixed capacity, older
  - **V2**: Auto-scaling, zone redundancy/zonal, recommended
    - Auto-scale based on load
    - Zone redundant or zonal deployment
    - Better performance

- [ ] 💰 **Application Gateway SKUs**
  - **Standard**: Basic functionality
  - **WAF (Web Application Firewall)**: Standard + OWASP protection
    - Protects against common vulnerabilities
    - SQL injection, XSS, etc.
    - OWASP Core Rule Set
    - Paid add-on

- [ ] 🔧 **Application Gateway Deployment**
  - Deploys into dedicated subnet
  - Recommended size: /24 (for growth)
  - V1: Maximum 32 instances (can use /26)
  - V2: Auto-scales, use larger subnet

- [ ] 🌍 **Frontend IP Configuration**
  - **Previously**: Always required public IP (could use NSG to block)
  - **Now (Preview)**: Public IP optional
  - Can have private IP only
  - Must have at least one (public OR private)
  - Dual-stack: IPv4 and IPv6

- [ ] 🎯 **Application Gateway Components**
  - **Frontend IP**: Public and/or private IP
  - **Listeners**: Listen on specific port for requests
  - **Rules**: Route traffic based on conditions
  - **Backend Pools**: Target resources
  - **HTTP Settings**: Backend communication configuration
  - **Health Probes**: Check backend availability

- [ ] 👂 **Listeners**
  - Listen on specific port on frontend IP
  - **Basic Listener**: All traffic on port to one rule
  - **Multi-Site Listener**: Multiple listeners on same port
    - Route based on FQDN (Fully Qualified Domain Name)
    - Server Name Indication (SNI) support
    - One IP, multiple websites (e.g., all on 443)
    - Different certificates per site

- [ ] 🔀 **Routing Rules**
  - **Basic**: Route to specific backend pool
  - **Path-based**: Route based on URL path
    - Example: /images/* → Image servers, /videos/* → Video servers
  - **URL Rewrite**: Modify URL before sending to backend
  - **Header Rewrite**: Modify request/response headers

- [ ] 🎯 **Backend Pools - Flexibility**
  - Virtual Machines
  - Virtual Machine Scale Sets
  - IP Addresses
  - Fully Qualified Domain Names (FQDNs)
  - App Services
  - **Can point to**: Resources in Azure, on-premises (via VPN/ExpressRoute), public IPs
  - Much more flexible than Azure Load Balancer

- [ ] ⚙️ **HTTP Settings**
  - Backend port and protocol
  - Cookie-based session affinity
  - Connection draining
  - Request timeout
  - Custom health probe override
  - Backend encryption (backend HTTPS)

- [ ] 🔒 **SSL/TLS Offloading**
  - Terminate SSL at gateway
  - Certificate management at gateway
  - Reduced load on backend servers
  - Can re-encrypt to backend (end-to-end SSL)

- [ ] 🔍 **Health Probes**
  - Check backend availability
  - HTTP/HTTPS checks
  - Custom paths and intervals
  - Automatic removal of unhealthy backends

- [ ] 🌐 **Web Application Firewall (WAF)**
  - Protect against OWASP Top 10 vulnerabilities
  - SQL injection, XSS, etc.
  - Detection or Prevention mode
  - Custom rules
  - Managed rule sets (updated by Microsoft)
  - Geo-filtering
  - Bot protection

---

## Key Takeaways

1. **Virtual WAN Standard** - Automatic transitive routing (unlike manual VNet peering)
2. **Microsoft Peering** - Access PaaS services over ExpressRoute without VNet
3. **Service Endpoints** - Subnet-level, public endpoint remains, firewall rules
4. **Private Endpoints** - Private IP in VNet, public can be disabled, works everywhere
5. **Private Link Service** - Expose your services via Private Link (Standard LB required)
6. **Azure Bastion** - Managed jump box, no public IPs on VMs, /26 subnet
7. **Load Balancer SKUs** - Basic retiring Sept 2025, use Standard (1000 backends, zones, IP addresses)
8. **Tuples** - 5-tuple (most specific), 3-tuple (medium), 2-tuple (most flexible)
9. **Application Gateway** - Layer 7, V2 with auto-scale, WAF for OWASP protection
10. **Backend Pool Flexibility** - App Gateway: VMs, IPs, FQDNs, App Services, on-prem
11. **Multi-Site Listener** - Multiple websites on same IP/port (SNI support)
12. **Floating IP** - Backend sees frontend IP (not its own)

---

## Exam Tips

- **Virtual WAN**: Standard includes transitive connectivity (spokes can talk); Basic is VPN only
- **Route Filter**: Used with Microsoft Peering to specify which services to advertise
- **Service Endpoint vs Private Endpoint**:
  - Service Endpoint = subnet-level, public endpoint remains
  - Private Endpoint = private IP, public can be disabled, works from anywhere
- **Private Link Service**: Requires Standard Load Balancer (not Basic/Free)
- **Azure Bastion Subnet**: Must be named "AzureBastionSubnet" exactly, /26 size
- **Bastion Standard**: Cross-platform (RDP to Linux, SSH to Windows), CLI access
- **Load Balancer Basic**: RETIRING Sept 30, 2025 (same date as Basic Public IP)
- **Standard Load Balancer**: Locked down by default, need outbound rules
- **Backend Pool IP Addresses**: Standard only (useful for AKS pods)
- **Floating IP**: Backend must be configured to accept frontend IP
- **Application Gateway Subnet**: Recommended /24 for V2 auto-scaling
- **Multi-Site Listener**: Same port, different FQDNs, SNI support
- **WAF**: Protection against OWASP Top 10, detection or prevention mode
- **Backend Pool Flexibility**: App Gateway can point to on-prem resources (LB cannot)

---

## Portal Navigation

- **Virtual WAN**: Search "Virtual WANs" → Create → Choose SKU
- **Service Endpoints**: VNet → Subnets → Select subnet → Service endpoints
- **Private Endpoints**: Search "Private endpoints" → Create → Select resource
- **Private Link Service**: Standard LB → Private Link → Add
- **Azure Bastion**: Search "Bastions" → Create → Choose SKU → AzureBastionSubnet
- **Load Balancer**: Search "Load balancers" → Create → Standard SKU
- **Application Gateway**: Search "Application gateways" → Create → V2 SKU
- **WAF Policies**: Search "Web Application Firewall" → Create policy

---

## Configuration Comparison

### Service Endpoint vs Private Endpoint

| Feature | Service Endpoint | Private Endpoint |
|---------|-----------------|------------------|
| **Public Endpoint** | Still exists | Can be disabled |
| **Scope** | Subnet only | Anywhere with routing |
| **On-premises Access** | No (public endpoint) | Yes (with DNS) |
| **Peered VNet Access** | No (without config) | Yes |
| **IP Address** | No IP allocated | Private IP in subnet |
| **DNS Required** | No | Yes (private DNS zone) |
| **Cost** | Free | Charged per endpoint |
| **Security** | Subnet firewall rules | Complete isolation |

### Load Balancer SKU Comparison

| Feature | Basic/Free (Retiring) | Standard |
|---------|----------------------|----------|
| **Retirement** | Sept 30, 2025 | Current |
| **Backends** | 300 (availability sets) | 1,000 |
| **SLA** | No SLA | SLA provided |
| **Zones** | No support | Zone redundant/zonal |
| **Backend Types** | NICs only | NICs or IP addresses |
| **Outbound** | Implicit | Explicit rules required |
| **Public IP** | Basic | Standard |
| **Cost** | Free | Paid |

### Application Gateway vs Load Balancer

| Feature | Azure Load Balancer | Application Gateway |
|---------|---------------------|---------------------|
| **Layer** | Layer 4 (TCP/UDP) | Layer 7 (HTTP/HTTPS) |
| **Protocol** | Any TCP/UDP | HTTP/HTTPS/HTTP2/WebSocket |
| **Routing** | Simple distribution | Path-based, multi-site |
| **SSL Offload** | No | Yes |
| **WAF** | No | Yes (SKU option) |
| **Backend Types** | NICs/IPs in VNet | VMs/IPs/FQDNs/App Services/On-prem |
| **URL Rewrite** | No | Yes |
| **Session Affinity** | Tuple-based | Cookie-based |
| **Auto-scale** | No | Yes (V2) |

---

## PowerShell/CLI Commands

```powershell
# Virtual WAN
New-AzVirtualWan -ResourceGroupName RG -Name MyVWAN -Type Standard

# Service Endpoint
$subnet = Get-AzVirtualNetworkSubnetConfig -Name MySubnet -VirtualNetwork $vnet
$subnet.ServiceEndpoints.Add("Microsoft.Storage")

# Private Endpoint
New-AzPrivateEndpoint -ResourceGroupName RG -Name MyPE -Subnet $subnet

# Private Link Service
New-AzPrivateLinkService -ResourceGroupName RG -Name MyPLS

# Azure Bastion
New-AzBastion -ResourceGroupName RG -Name MyBastion -Sku Standard

# Load Balancer
New-AzLoadBalancer -ResourceGroupName RG -Name MyLB -Sku Standard

# Application Gateway
New-AzApplicationGateway -ResourceGroupName RG -Name MyAppGW
```

---

## Decision Trees

### Service Endpoint vs Private Endpoint?

```
Need to access PaaS service securely?
├─ Access only from specific subnets?
│   └─ Service Endpoint (simpler, free)
├─ Access from on-premises?
│   └─ Private Endpoint (requires DNS config)
├─ Need to disable public endpoint?
│   └─ Private Endpoint
└─ Access from multiple VNets/locations?
    └─ Private Endpoint
```

### Load Balancer vs Application Gateway?

```
What protocol?
├─ TCP/UDP (non-HTTP)
│   └─ Azure Load Balancer
└─ HTTP/HTTPS
    ├─ Need URL-based routing?
    │   └─ Application Gateway
    ├─ Need SSL offload?
    │   └─ Application Gateway
    ├─ Need WAF?
    │   └─ Application Gateway
    └─ Simple distribution?
        └─ Either (App GW for more features)
```

### Azure Bastion SKU Selection?

```
What features needed?
├─ VMs in same VNet only?
│   └─ Basic/Developer
├─ Need peered VNet access?
│   └─ Basic or Standard
├─ Need CLI access (not just portal)?
│   └─ Standard
├─ Cross-platform (RDP to Linux)?
│   └─ Standard
└─ Need shareable links or copy/paste control?
    └─ Standard
```
