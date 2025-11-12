# Domain 3: Security Architecture (18%)

## Domain Weight: 18% of exam

---

## Professor Messer's Video Structure

This study guide follows Professor Messer's CompTIA SY0-701 Security+ Training Course structure.
**Total Videos in Domain 3: 18 videos**

### Section 3.1 - Compare and contrast security implications of different architecture models (4 videos)
- [ ] Network Infrastructure Concepts
- [ ] Cloud Infrastructures
- [ ] Infrastructure Considerations
- [ ] Other Infrastructure Concepts

### Section 3.2 - Given a scenario, apply security principles to secure enterprise infrastructure (6 videos)
- [ ] Secure Infrastructures
- [ ] Firewall Types
- [ ] Intrusion Prevention
- [ ] Network Appliances
- [ ] Port Security
- [ ] Secure Communication

### Section 3.3 - Compare and contrast concepts and strategies to protect data (3 videos)
- [ ] States of Data
- [ ] Data Types and Classifications
- [ ] Protecting Data

### Section 3.4 - Explain the importance of resilience and recovery in security architecture (5 videos)
- [ ] Resiliency
- [ ] Backups
- [ ] Power Resiliency
- [ ] Capacity Planning
- [ ] Recovery Testing

---

## Topics Covered

### 3.1 Compare and contrast security implications of different architecture models

#### Video 1: Network Infrastructure Concepts
- [ ] **Watched Professor Messer's "Network Infrastructure Concepts" video**

##### Network Segmentation
- [ ] **Physical segmentation**
  - Separate physical networks
  - Air-gapped systems
  - Complete isolation
  - Highest security level

- [ ] **Logical segmentation (VLANs)**
  - Virtual Local Area Networks
  - Switch-based segmentation
  - Broadcast domain separation
  - Layer 2 isolation

- [ ] **Screened subnet (DMZ)**
  - Demilitarized Zone
  - Public-facing services
  - Additional security layer
  - Separated from internal network

- [ ] **Intranet**
  - Internal private network
  - Company resources
  - Internal users only

- [ ] **Extranet**
  - Extended network access
  - Partners and vendors
  - Controlled external access
  - Limited resources

##### Software-Defined Networking (SDN)
- [ ] **SDN architecture**
  - Centralized control plane
  - Distributed data plane
  - API-driven management
  - Network programmability

- [ ] **Application layer**
  - Business applications
  - Network services
  - Northbound APIs

- [ ] **Control layer**
  - Network controller
  - Policy decisions
  - Traffic management

- [ ] **Infrastructure layer**
  - Physical switches and routers
  - Data forwarding
  - Southbound APIs

- [ ] **Benefits of SDN**
  - Centralized management
  - Automated provisioning
  - Network agility
  - Cost reduction

#### Video 2: Cloud Infrastructures
- [ ] **Watched Professor Messer's "Cloud Infrastructures" video**

##### Cloud Service Models
- [ ] **Infrastructure as a Service (IaaS)**
  - Virtual machines
  - Storage and networking
  - Customer manages OS and applications
  - Examples: AWS EC2, Azure VMs, Google Compute Engine

- [ ] **Platform as a Service (PaaS)**
  - Development platform
  - Runtime environment
  - No OS management required
  - Examples: Heroku, Google App Engine, Azure App Service

- [ ] **Software as a Service (SaaS)**
  - Complete applications
  - Web-based delivery
  - No infrastructure management
  - Examples: Office 365, Salesforce, Gmail

##### Cloud Deployment Models
- [ ] **Public cloud**
  - Shared infrastructure
  - Internet-accessible
  - Pay-per-use model
  - Examples: AWS, Azure, GCP

- [ ] **Private cloud**
  - Dedicated infrastructure
  - On-premises or hosted
  - Complete control
  - Enhanced security

- [ ] **Hybrid cloud**
  - Mix of public and private
  - Data portability
  - Flexible deployment
  - Best of both worlds

- [ ] **Community cloud**
  - Shared by specific group
  - Common interests/requirements
  - Cost-sharing model
  - Compliance-focused

##### Cloud Concepts
- [ ] **Infrastructure as Code (IaC)**
  - Automated deployment
  - Version-controlled infrastructure
  - Consistent configurations
  - Tools: Terraform, CloudFormation, Ansible

- [ ] **Serverless architecture**
  - Function as a Service (FaaS)
  - No server management
  - Event-driven execution
  - Pay per execution
  - Examples: AWS Lambda, Azure Functions

- [ ] **Microservices**
  - Decomposed applications
  - Independent services
  - API communication
  - Scalable and maintainable

- [ ] **API integrations**
  - RESTful APIs
  - Service communication
  - Third-party integrations
  - Authentication and authorization

- [ ] **Containers**
  - Lightweight virtualization
  - Application packaging
  - Portable deployment
  - Docker, Kubernetes

#### Video 3: Infrastructure Considerations
- [ ] **Watched Professor Messer's "Infrastructure Considerations" video**

##### Availability and Resilience
- [ ] **High availability**
  - Minimize downtime
  - Redundant systems
  - Failover capabilities
  - SLA targets (99.9%, 99.99%, 99.999%)

- [ ] **Fault tolerance**
  - Continue operating during failures
  - Redundant components
  - No single point of failure
  - Graceful degradation

- [ ] **Scalability**
  - Handle increased load
  - Vertical scaling (scale up)
  - Horizontal scaling (scale out)
  - Elastic resources

- [ ] **Responsiveness**
  - Performance optimization
  - Low latency
  - Fast response times
  - User experience

##### Cost Considerations
- [ ] **CapEx vs. OpEx**
  - Capital Expenditure (CapEx): upfront costs
  - Operational Expenditure (OpEx): ongoing costs
  - Cloud shifts to OpEx model
  - Budget planning

- [ ] **Cloud cost optimization**
  - Right-sizing resources
  - Reserved instances
  - Auto-scaling
  - Cost monitoring tools

##### Security Considerations
- [ ] **Attack surface**
  - Exposed systems and services
  - Entry points for attackers
  - Minimize exposure
  - Continuous assessment

- [ ] **Ease of deployment**
  - Automation tools
  - Quick provisioning
  - Consistent environments
  - DevOps integration

- [ ] **Risk transference**
  - Shared responsibility model
  - Cloud provider security
  - Insurance and contracts
  - Third-party risk

#### Video 4: Other Infrastructure Concepts
- [ ] **Watched Professor Messer's "Other Infrastructure Concepts" video**

##### Virtualization
- [ ] **Virtual machines (VMs)**
  - Hypervisor technology
  - Multiple OS instances
  - Resource sharing
  - Isolation between VMs

- [ ] **Type 1 hypervisor**
  - Bare metal
  - Direct hardware access
  - Better performance
  - Examples: VMware ESXi, Hyper-V, KVM

- [ ] **Type 2 hypervisor**
  - Runs on host OS
  - Lower performance
  - Easier setup
  - Examples: VMware Workstation, VirtualBox

- [ ] **VM sprawl**
  - Uncontrolled VM growth
  - Management challenges
  - Security risks
  - Resource waste

- [ ] **VM escape**
  - Breaking out of VM
  - Attacking hypervisor
  - Accessing host system
  - Critical vulnerability

##### Containerization
- [ ] **Containers vs. VMs**
  - Lighter weight than VMs
  - Share host OS kernel
  - Faster startup
  - More portable

- [ ] **Docker**
  - Container platform
  - Docker images
  - Docker Hub registry
  - Container orchestration

- [ ] **Kubernetes**
  - Container orchestration
  - Automated deployment
  - Scaling and management
  - Load balancing

##### IoT and Embedded Systems
- [ ] **Internet of Things (IoT)**
  - Connected devices
  - Sensors and actuators
  - Smart home devices
  - Industrial IoT (IIoT)

- [ ] **IoT security challenges**
  - Weak authentication
  - Unpatched vulnerabilities
  - Limited resources
  - Privacy concerns

- [ ] **Embedded systems**
  - Dedicated function
  - Firmware-based
  - RTOS (Real-Time OS)
  - Examples: routers, printers, industrial controls

- [ ] **SCADA/ICS**
  - Supervisory Control and Data Acquisition
  - Industrial Control Systems
  - Critical infrastructure
  - Air-gapped networks (traditionally)

##### Data Center Infrastructure
- [ ] **Data center design**
  - Hot aisle / cold aisle
  - Cooling systems
  - Power distribution
  - Physical security

- [ ] **On-premises data centers**
  - Complete control
  - Significant investment
  - Maintenance overhead
  - Legacy systems

- [ ] **Colocation**
  - Shared data center space
  - Customer equipment
  - Provider facilities
  - Shared security responsibility

**Notes:**
[Add your study notes here as you learn]

---

### 3.2 Given a scenario, apply security principles to secure enterprise infrastructure

#### Video 1: Secure Infrastructures
- [ ] **Watched Professor Messer's "Secure Infrastructures" video**

##### Security Zones
- [ ] **Trusted zone**
  - Internal network
  - Corporate resources
  - Authenticated users
  - Highest trust level

- [ ] **Untrusted zone**
  - Internet
  - External networks
  - No authentication
  - Lowest trust level

- [ ] **DMZ (Screened subnet)**
  - Between trusted and untrusted
  - Public-facing services
  - Additional protection layer
  - Web servers, email servers

- [ ] **Guest network**
  - Isolated from internal network
  - Limited access
  - Internet only
  - Visitor access

##### Network Connectivity
- [ ] **East-West traffic**
  - Lateral movement
  - Server-to-server communication
  - Within data center
  - Micro-segmentation important

- [ ] **North-South traffic**
  - Entering/leaving network
  - Client-to-server
  - Internet traffic
  - Traditional perimeter security

##### Attack Surface Reduction
- [ ] **Minimize exposure**
  - Reduce open ports
  - Disable unnecessary services
  - Remove unused applications
  - Principle of least functionality

- [ ] **Connectivity considerations**
  - Need-to-know access
  - Secure communication channels
  - Network segmentation
  - Zero Trust approach

#### Video 2: Firewall Types
- [ ] **Watched Professor Messer's "Firewall Types" video**

##### Firewall Categories
- [ ] **Network-based firewalls**
  - Protects entire network
  - Hardware appliances
  - High throughput
  - Perimeter defense

- [ ] **Host-based firewalls**
  - Software on individual systems
  - OS-level protection
  - Granular control
  - Examples: Windows Firewall, iptables

##### Firewall Technologies
- [ ] **Stateless firewalls**
  - Packet filtering
  - ACL-based rules
  - No connection tracking
  - Fast but limited

- [ ] **Stateful firewalls**
  - Connection state tracking
  - Dynamic rules
  - Better security
  - Most common type

- [ ] **Next-Generation Firewall (NGFW)**
  - Deep packet inspection
  - Application awareness
  - IPS integration
  - URL filtering
  - Advanced threat protection

- [ ] **Unified Threat Management (UTM)**
  - All-in-one security
  - Firewall, IPS, antivirus
  - Web filtering
  - VPN
  - Small/medium business focus

- [ ] **Web Application Firewall (WAF)**
  - Protects web applications
  - HTTP/HTTPS traffic inspection
  - OWASP Top 10 protection
  - SQL injection and XSS prevention
  - Examples: ModSecurity, Cloudflare WAF

#### Video 3: Intrusion Prevention
- [ ] **Watched Professor Messer's "Intrusion Prevention" video**

##### IDS vs. IPS
- [ ] **Intrusion Detection System (IDS)**
  - Passive monitoring
  - Alert on suspicious activity
  - No blocking
  - Signature and anomaly detection

- [ ] **Intrusion Prevention System (IPS)**
  - Active blocking
  - Inline deployment
  - Prevents attacks
  - Automated response

##### Detection Methods
- [ ] **Signature-based detection**
  - Known attack patterns
  - Low false positives
  - Requires updates
  - Can't detect zero-days

- [ ] **Anomaly-based detection**
  - Baseline behavior
  - Detects deviations
  - Finds unknown attacks
  - Higher false positives

- [ ] **Heuristic/behavior-based**
  - Suspicious patterns
  - Machine learning
  - Adaptive detection
  - Evolving threats

##### IPS Deployment
- [ ] **Inline mode**
  - In the traffic path
  - Can block attacks
  - Potential bottleneck
  - Active protection

- [ ] **Passive mode (TAP/SPAN)**
  - Monitor copy of traffic
  - No blocking capability
  - No impact on performance
  - Detection only

- [ ] **Fail-open vs. Fail-closed**
  - Fail-open: traffic continues if IPS fails
  - Fail-closed: traffic stops if IPS fails
  - Availability vs. security trade-off

#### Video 4: Network Appliances
- [ ] **Watched Professor Messer's "Network Appliances" video**

##### Security Appliances
- [ ] **Jump server (Jump box)**
  - Secure administrative access
  - Gateway to secure zone
  - Centralized access point
  - Auditing and logging

- [ ] **Proxy servers**
  - Intermediary for requests
  - Content filtering
  - Caching
  - Anonymity

- [ ] **Forward proxy**
  - Internal clients to Internet
  - User authentication
  - Content filtering
  - Bandwidth control

- [ ] **Reverse proxy**
  - Internet clients to internal servers
  - Load balancing
  - SSL offloading
  - Caching and compression

- [ ] **Load balancers**
  - Distribute traffic
  - Multiple servers
  - High availability
  - Health checks

- [ ] **Load balancing algorithms**
  - Round-robin
  - Least connections
  - Weighted distribution
  - Source IP hash
  - Session persistence

- [ ] **Layer 4 vs. Layer 7 load balancing**
  - Layer 4: Transport layer (IP/port)
  - Layer 7: Application layer (HTTP/content)
  - Layer 7 more intelligent
  - Layer 4 faster

##### Monitoring Devices
- [ ] **Sensors**
  - Collect security data
  - Network traffic analysis
  - Anomaly detection
  - Distributed placement

- [ ] **Collectors**
  - Aggregate sensor data
  - Centralized logging
  - SIEM integration
  - Data correlation

#### Video 5: Port Security
- [ ] **Watched Professor Messer's "Port Security" video**

##### Network Access Control
- [ ] **IEEE 802.1X**
  - Port-based authentication
  - Before network access
  - Supplicant, authenticator, authentication server
  - RADIUS/TACACS+

- [ ] **EAP (Extensible Authentication Protocol)**
  - Framework for authentication
  - Multiple methods
  - Flexible and extensible

- [ ] **EAP-TLS**
  - Certificate-based
  - Mutual authentication
  - Most secure
  - PKI required

- [ ] **EAP-TTLS**
  - Tunneled TLS
  - Server certificate only
  - Username/password in tunnel

- [ ] **PEAP**
  - Protected EAP
  - TLS tunnel
  - Microsoft implementation

##### Port Security Features
- [ ] **MAC filtering**
  - Allow/deny by MAC address
  - Switch port security
  - Easily spoofed
  - Basic protection

- [ ] **Persistent MAC learning**
  - Remember authorized MACs
  - Automatic learning
  - Sticky MAC addresses

- [ ] **Port security violations**
  - Shutdown: disable port
  - Restrict: drop frames, alert
  - Protect: drop frames silently

#### Video 6: Secure Communication
- [ ] **Watched Professor Messer's "Secure Communication" video**

##### VPN Technologies
- [ ] **Remote access VPN**
  - Individual users
  - Client software
  - Secure remote work
  - IPsec or SSL/TLS

- [ ] **Site-to-site VPN**
  - Connect networks
  - Always-on connection
  - No client software
  - IPsec common

- [ ] **IPsec VPN**
  - Network layer security
  - Authentication Header (AH)
  - Encapsulating Security Payload (ESP)
  - Transport mode vs. tunnel mode

- [ ] **SSL/TLS VPN**
  - Application layer
  - Web browser access
  - Easier deployment
  - Clientless options

- [ ] **Split tunnel vs. full tunnel**
  - Split: some traffic via VPN, some direct
  - Full: all traffic via VPN
  - Security vs. performance

##### Advanced Technologies
- [ ] **Software-Defined WAN (SD-WAN)**
  - WAN optimization
  - Multiple connection types
  - Intelligent routing
  - Centralized management
  - Application-aware

- [ ] **Secure Access Service Edge (SASE)**
  - Cloud-delivered security
  - Combines networking and security
  - WAN + security services
  - Zero Trust Network Access (ZTNA)

**Notes:**
[Add your study notes here as you learn]

---

### 3.3 Compare and contrast concepts and strategies to protect data

#### Video 1: States of Data
- [ ] **Watched Professor Messer's "States of Data" video**

##### Data States
- [ ] **Data at rest**
  - Stored on media
  - Hard drives, SSDs, databases
  - File servers, backups
  - Encryption: BitLocker, FileVault, database encryption

- [ ] **Data in transit (data in motion)**
  - Moving across network
  - Between systems
  - Over the Internet
  - Encryption: TLS/SSL, IPsec, VPN

- [ ] **Data in use (data in processing)**
  - In memory/CPU
  - Being processed
  - Most vulnerable state
  - Encryption: homomorphic encryption, trusted execution environments

##### Protection Methods by State
- [ ] **Protecting data at rest**
  - Full-disk encryption (FDE)
  - File-level encryption
  - Database encryption
  - Secure deletion
  - Physical security

- [ ] **Protecting data in transit**
  - TLS/SSL for web traffic
  - VPN for network traffic
  - Email encryption (S/MIME, PGP)
  - Secure file transfer (SFTP, FTPS)

- [ ] **Protecting data in use**
  - Process isolation
  - Secure enclaves
  - Application security
  - Memory encryption
  - Access controls

#### Video 2: Data Types and Classifications
- [ ] **Watched Professor Messer's "Data Types and Classifications" video**

##### Data Types
- [ ] **Regulated data**
  - Legal requirements
  - Industry-specific
  - Compliance mandates
  - Examples: HIPAA, PCI DSS, GDPR

- [ ] **Trade secrets**
  - Proprietary information
  - Competitive advantage
  - Business value
  - Non-disclosure agreements

- [ ] **Intellectual property (IP)**
  - Patents, trademarks
  - Copyrights
  - Trade secrets
  - Legal protection

- [ ] **Legal information**
  - Contracts
  - Legal documents
  - Attorney-client privilege
  - Discovery and litigation holds

- [ ] **Financial information**
  - Banking records
  - Credit card data
  - Accounting information
  - SOX compliance

- [ ] **Human-readable vs. non-human-readable**
  - Plain text vs. encrypted
  - Images vs. hashes
  - Processed data
  - Machine-only formats

##### Data Classification Levels
- [ ] **Public**
  - No confidentiality
  - General distribution
  - Marketing materials
  - Public websites

- [ ] **Private/Internal**
  - Internal use only
  - Not for public
  - Company policies
  - Internal communications

- [ ] **Sensitive**
  - Limited distribution
  - Potential damage if disclosed
  - Financial data
  - Personnel information

- [ ] **Confidential**
  - Highly sensitive
  - Restricted access
  - Significant damage potential
  - Trade secrets

- [ ] **Critical/Restricted**
  - Most sensitive
  - Very limited access
  - Severe damage if disclosed
  - Executive strategy, M&A

##### Government Classifications
- [ ] **Unclassified**
- [ ] **Sensitive but Unclassified (SBU)**
- [ ] **Confidential**
- [ ] **Secret**
- [ ] **Top Secret**

##### Data Sovereignty
- [ ] **Geographic restrictions**
  - Data location requirements
  - National laws
  - GDPR, data residency
  - Cloud considerations

#### Video 3: Protecting Data
- [ ] **Watched Professor Messer's "Protecting Data" video**

##### Data Protection Techniques
- [ ] **Encryption**
  - Symmetric encryption (AES)
  - Asymmetric encryption (RSA)
  - End-to-end encryption
  - Key management

- [ ] **Hashing**
  - One-way function
  - Integrity verification
  - Password storage
  - Algorithms: SHA-256, SHA-3

- [ ] **Masking**
  - Hide portions of data
  - Display limited characters
  - PAN masking (XXXX-XXXX-XXXX-1234)
  - PII protection

- [ ] **Tokenization**
  - Replace sensitive data with token
  - Token has no meaning
  - Reversible by system
  - Payment processing

- [ ] **Obfuscation**
  - Make data unclear
  - Code obfuscation
  - Security through obscurity
  - Not a primary control

- [ ] **Segmentation**
  - Separate data storage
  - Network segmentation
  - Database partitioning
  - Reduce exposure

##### Data Loss Prevention (DLP)
- [ ] **DLP systems**
  - Prevent data exfiltration
  - Monitor and block
  - Email, web, endpoints

- [ ] **DLP detection methods**
  - Pattern matching
  - Keyword searches
  - Regular expressions
  - Fingerprinting
  - Machine learning

- [ ] **DLP deployment**
  - Network DLP
  - Endpoint DLP
  - Cloud DLP
  - Storage DLP

##### Rights Management
- [ ] **Digital Rights Management (DRM)**
  - Control access and usage
  - Prevent copying
  - Time-based access
  - Geographic restrictions

- [ ] **Information Rights Management (IRM)**
  - Document protection
  - Persistent protection
  - Travels with data
  - Office document protection

##### Geographic Restrictions
- [ ] **Geofencing**
  - Geographic boundaries
  - Access restrictions
  - Mobile device management
  - Location-based policies

- [ ] **Geolocation**
  - Determine user location
  - IP geolocation
  - GPS tracking
  - Compliance enforcement

**Notes:**
[Add your study notes here as you learn]

---

### 3.4 Explain the importance of resilience and recovery in security architecture

#### Video 1: Resiliency
- [ ] **Watched Professor Messer's "Resiliency" video**

##### High Availability Concepts
- [ ] **Redundancy**
  - Duplicate components
  - Eliminate single points of failure
  - Failover capability
  - N+1, N+2, 2N

- [ ] **Server clustering**
  - Multiple servers working together
  - Active-active
  - Active-passive
  - Failover clusters

- [ ] **Load balancing**
  - Distribute workload
  - Improve performance
  - Automatic failover
  - Health monitoring

##### Geographic Distribution
- [ ] **Geographic dispersal**
  - Multiple locations
  - Disaster protection
  - Natural disaster resilience
  - Distance requirements

- [ ] **Hot site**
  - Fully operational
  - Real-time replication
  - Immediate failover
  - Most expensive

- [ ] **Warm site**
  - Partially equipped
  - Some delay for activation
  - Balance cost and recovery time
  - Hardware ready, data needs syncing

- [ ] **Cold site**
  - Basic facilities
  - Longest recovery time
  - Lowest cost
  - Empty data center space

##### Platform Diversity
- [ ] **Multi-cloud systems**
  - Multiple cloud providers
  - Avoid vendor lock-in
  - Geographic distribution
  - Resilience from provider outages

- [ ] **Platform diversity**
  - Different technologies
  - Different vendors
  - Reduce single-vendor risk
  - Defense in depth

#### Video 2: Backups
- [ ] **Watched Professor Messer's "Backups" video**

##### Backup Types
- [ ] **Full backup**
  - Complete copy
  - All data
  - Slowest backup
  - Fastest restore

- [ ] **Incremental backup**
  - Only changed data since last backup
  - Fastest backup
  - Slower restore (need full + all incrementals)
  - Less storage space

- [ ] **Differential backup**
  - Changes since last full backup
  - Moderate backup time
  - Moderate restore (need full + last differential)
  - More storage than incremental

- [ ] **Snapshot**
  - Point-in-time copy
  - Instant backup
  - Storage-level feature
  - Quick rollback

##### Backup Considerations
- [ ] **Backup frequency**
  - Daily, weekly, monthly
  - Recovery Point Objective (RPO)
  - Balance cost and data loss
  - Critical vs. non-critical data

- [ ] **Backup retention**
  - How long to keep
  - Compliance requirements
  - Storage costs
  - Grandfather-father-son rotation

- [ ] **Backup encryption**
  - Protect backup data
  - At rest and in transit
  - Key management
  - Secure recovery

- [ ] **Backup testing**
  - Verify recoverability
  - Regular test restores
  - Validate integrity
  - Document procedures

##### Backup Strategies
- [ ] **On-site backups**
  - Fast recovery
  - Accessible
  - Same location risk
  - Disaster vulnerable

- [ ] **Off-site backups**
  - Geographic separation
  - Disaster protection
  - Slower recovery
  - Third-party storage

- [ ] **Cloud backups**
  - Scalable storage
  - Geographic distribution
  - Automated
  - Subscription-based

- [ ] **Replication**
  - Real-time copying
  - Synchronous vs. asynchronous
  - High availability
  - Geographic redundancy

- [ ] **3-2-1 backup rule**
  - 3 copies of data
  - 2 different media types
  - 1 off-site copy
  - Best practice

#### Video 3: Power Resiliency
- [ ] **Watched Professor Messer's "Power Resiliency" video**

##### Uninterruptible Power Supply (UPS)
- [ ] **UPS types**
  - Offline/standby UPS
  - Line-interactive UPS
  - Online/double-conversion UPS

- [ ] **UPS functions**
  - Battery backup
  - Power conditioning
  - Surge protection
  - Graceful shutdown

- [ ] **UPS runtime**
  - Limited duration
  - Bridge to generator
  - Orderly shutdown
  - Depends on load

##### Backup Power
- [ ] **Generators**
  - Long-term power
  - Diesel or natural gas
  - Automatic transfer switch
  - Fuel storage and maintenance

- [ ] **Power Distribution Units (PDU)**
  - Rack-level distribution
  - Intelligent PDUs
  - Remote monitoring
  - Per-outlet control

##### Power Management
- [ ] **Dual power supplies**
  - Redundant PSUs
  - Different circuits
  - Automatic failover
  - Server and network equipment

- [ ] **Managed Power Distribution**
  - Multiple power sources
  - Load balancing
  - Remote management
  - Alerting and monitoring

#### Video 4: Capacity Planning
- [ ] **Watched Professor Messer's "Capacity Planning" video**

##### Capacity Considerations
- [ ] **People**
  - Staffing levels
  - Skills and training
  - 24/7 coverage
  - Growth planning

- [ ] **Technology**
  - Hardware resources
  - Software licenses
  - Network bandwidth
  - Storage capacity

- [ ] **Infrastructure**
  - Data center space
  - Power and cooling
  - Physical security
  - Rack space

##### Performance Monitoring
- [ ] **Resource utilization**
  - CPU usage
  - Memory consumption
  - Disk I/O
  - Network throughput

- [ ] **Trending analysis**
  - Historical data
  - Growth patterns
  - Capacity forecasting
  - Proactive planning

- [ ] **Scalability planning**
  - Vertical scaling (scale up)
  - Horizontal scaling (scale out)
  - Cloud elasticity
  - Budget considerations

#### Video 5: Recovery Testing
- [ ] **Watched Professor Messer's "Recovery Testing" video**

##### Testing Types
- [ ] **Tabletop exercises**
  - Discussion-based
  - Walk through scenarios
  - Low cost
  - Identify gaps
  - No actual execution

- [ ] **Simulation testing**
  - Role-playing
  - Simulated disaster
  - Test procedures
  - Practice coordination
  - No system impact

- [ ] **Parallel testing**
  - Alternate site activated
  - Production continues
  - Side-by-side operation
  - Validate capabilities
  - No downtime

- [ ] **Full interruption testing**
  - Complete failover
  - Production switched
  - Most realistic
  - Highest risk
  - Business impact

##### Recovery Metrics
- [ ] **Recovery Time Objective (RTO)**
  - Maximum acceptable downtime
  - How long to restore
  - Business requirement
  - Drives technology choices

- [ ] **Recovery Point Objective (RPO)**
  - Maximum acceptable data loss
  - Point-in-time recovery
  - Backup frequency
  - Replication strategy

- [ ] **Mean Time to Repair (MTTR)**
  - Average repair time
  - Historical metric
  - Improve over time
  - Incident response efficiency

- [ ] **Mean Time Between Failures (MTBF)**
  - Reliability metric
  - Expected operational time
  - Hardware specifications
  - Maintenance planning

##### Business Continuity Planning
- [ ] **Business Impact Analysis (BIA)**
  - Identify critical functions
  - Determine impact of disruption
  - Priority ranking
  - Recovery requirements

- [ ] **Continuity of Operations Planning (COOP)**
  - Maintain essential functions
  - During disruption
  - Alternate locations
  - Resource requirements

- [ ] **Disaster Recovery Plan (DRP)**
  - Technical recovery
  - IT systems and data
  - Step-by-step procedures
  - Regular testing

**Notes:**
[Add your study notes here as you learn]

---

## Key Terms & Definitions

**Term** | **Definition**
---------|---------------
**SDN** | Software-Defined Networking - centralized network control
**IaaS** | Infrastructure as a Service - virtualized computing resources
**PaaS** | Platform as a Service - development and deployment platform
**SaaS** | Software as a Service - cloud-delivered applications
**IaC** | Infrastructure as Code - automated infrastructure deployment
**DMZ** | Demilitarized Zone - security buffer between networks
**NGFW** | Next-Generation Firewall - advanced firewall with DPI and IPS
**UTM** | Unified Threat Management - all-in-one security appliance
**WAF** | Web Application Firewall - protects web applications
**IDS/IPS** | Intrusion Detection/Prevention System
**SASE** | Secure Access Service Edge - cloud-delivered security
**DLP** | Data Loss Prevention - prevents data exfiltration
**UPS** | Uninterruptible Power Supply - battery backup
**RTO** | Recovery Time Objective - maximum acceptable downtime
**RPO** | Recovery Point Objective - maximum acceptable data loss
**MTTR** | Mean Time to Repair - average time to fix
**MTBF** | Mean Time Between Failures - reliability metric
**BIA** | Business Impact Analysis - identify critical functions

---

## Acronyms to Know

- **API** - Application Programming Interface
- **BIA** - Business Impact Analysis
- **COOP** - Continuity of Operations Planning
- **DLP** - Data Loss Prevention
- **DMZ** - Demilitarized Zone
- **DRM** - Digital Rights Management
- **DRP** - Disaster Recovery Plan
- **EAP** - Extensible Authentication Protocol
- **FaaS** - Function as a Service
- **IaaS** - Infrastructure as a Service
- **IaC** - Infrastructure as Code
- **ICS** - Industrial Control Systems
- **IDS** - Intrusion Detection System
- **IoT** - Internet of Things
- **IPS** - Intrusion Prevention System
- **IRM** - Information Rights Management
- **MTBF** - Mean Time Between Failures
- **MTTR** - Mean Time to Repair
- **NGFW** - Next-Generation Firewall
- **PaaS** - Platform as a Service
- **PDU** - Power Distribution Unit
- **RPO** - Recovery Point Objective
- **RTO** - Recovery Time Objective
- **SaaS** - Software as a Service
- **SASE** - Secure Access Service Edge
- **SCADA** - Supervisory Control and Data Acquisition
- **SD-WAN** - Software-Defined Wide Area Network
- **SDN** - Software-Defined Networking
- **UPS** - Uninterruptible Power Supply
- **UTM** - Unified Threat Management
- **VLAN** - Virtual Local Area Network
- **WAF** - Web Application Firewall

---

## Practice Questions

### Question 1
[Question text]

**Answer:** [A/B/C/D]
**Explanation:** [Why this is correct]

---

## Real-World Examples

[Add examples of how these concepts apply in real scenarios]

---

## IAM Relevance

### Architecture and IAM
- Network segmentation for identity services
- Jump servers for privileged access management
- Proxy servers for authentication and authorization
- Load balancers for identity provider high availability

### Security Controls
- 802.1X for network access control
- VPN authentication and authorization
- Zero Trust architecture (SASE/ZTNA)
- API security for identity services

### Data Protection
- Protecting identity data (PII)
- Encryption of authentication credentials
- DLP for preventing credential leaks
- Rights management for access control

### Resilience
- High availability for authentication services
- Disaster recovery for identity systems
- Backup and recovery of identity data
- Multi-factor authentication resilience

---

## Exam Tips

- **Understand cloud models** - Know IaaS, PaaS, SaaS differences
- **Know firewall types** - NGFW, UTM, WAF characteristics
- **Memorize data states** - At rest, in transit, in use
- **Understand resiliency** - Hot, warm, cold sites; RTO vs. RPO
- **Know backup types** - Full, incremental, differential
- **Security zones** - DMZ, trusted, untrusted
- **VPN types** - Site-to-site vs. remote access
- **IDS vs. IPS** - Detection vs. prevention
- **This is 18% of exam** - Focus on infrastructure and cloud security!

---

## Related Labs

- [Link to relevant hands-on labs]

---

**Study Status:** Not Started
**Confidence Level:** [1-5]
**Last Reviewed:** [Date]

---

**Note:** This domain contains critical infrastructure and architecture concepts. Understanding these fundamentals is essential for designing secure systems and is heavily tested on the exam!
