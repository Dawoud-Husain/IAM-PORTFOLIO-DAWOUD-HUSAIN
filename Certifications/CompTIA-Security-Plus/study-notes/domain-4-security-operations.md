# Domain 4: Security Operations (28%)

## Domain Weight: 28% of exam (LARGEST DOMAIN!)

---

## Professor Messer's Video Structure

This study guide follows Professor Messer's CompTIA SY0-701 Security+ Training Course structure.
**Total Videos in Domain 4: 28 videos**

### Section 4.1 - Given a scenario, apply common security techniques to computing resources (5 videos)
- [ ] Secure Baselines
- [ ] Hardening Targets
- [ ] Wireless Security Settings
- [ ] Securing Wireless and Mobile
- [ ] Application Security

### Section 4.2 - Explain the security implications of proper hardware, software, and data asset management (1 video)
- [ ] Asset Management

### Section 4.3 - Explain various activities associated with vulnerability management (5 videos)
- [ ] Vulnerability Scanning
- [ ] Analyzing Vulnerabilities
- [ ] Vulnerability Remediation
- [ ] Penetration Testing
- [ ] Threat Intelligence

### Section 4.4 - Explain security alerting and monitoring concepts and tools (2 videos)
- [ ] Security Monitoring
- [ ] Security Tools

### Section 4.5 - Given a scenario, modify enterprise capabilities to enhance security (7 videos)
- [ ] Operating System Security
- [ ] Endpoint Security
- [ ] Email Security
- [ ] Secure Protocols
- [ ] Web Filtering
- [ ] Firewalls
- [ ] Monitoring Data

### Section 4.6 - Given a scenario, implement and maintain identity and access management (4 videos)
- [ ] Identity and Access Management
- [ ] Multifactor Authentication
- [ ] Password Security
- [ ] Access Controls

### Section 4.7 - Explain the importance of automation and orchestration related to secure operations (1 video)
- [ ] Scripting and Automation

### Section 4.8 - Explain appropriate incident response activities (3 videos)
- [ ] Incident Planning
- [ ] Incident Response
- [ ] Digital Forensics

---

## Topics Covered

### 4.1 Given a scenario, apply common security techniques to computing resources

#### Video 1: Secure Baselines
- [ ] **Watched Professor Messer's "Secure Baselines" video**

##### Baseline Configuration
- [ ] 📏 **Security baseline**
  - Standard security configuration
  - Minimum security requirements
  - Consistent across organization
  - Based on industry standards

- [ ] 📚 **Baseline sources**
  - CIS (Center for Internet Security) Benchmarks
  - NIST guidelines
  - STIGs (Security Technical Implementation Guides)
  - Vendor recommendations
  - Industry best practices

##### Establishing Baselines
- [ ] ⚙️ **Configuration management**
  - Document current state
  - Define desired state
  - Track changes over time
  - Version control

- [ ] 💿 **Image deployment**
  - Golden images
  - Automated deployment
  - Consistent configuration
  - Rapid provisioning

- [ ] ⚠️ **Baseline deviation**
  - Detect unauthorized changes
  - Configuration drift
  - Compliance monitoring
  - Remediation procedures

#### Video 2: Hardening Targets
- [ ] **Watched Professor Messer's "Hardening Targets" video**

##### Mobile Device Hardening
- [ ] 📱 **Mobile security controls**
  - Screen lock and password
  - Encryption
  - Remote wipe capability
  - App controls

- [ ] 📲 **Mobile Device Management (MDM)**
  - Centralized management
  - Policy enforcement
  - Application management
  - Device inventory

- [ ] 🔀 **Deployment models**
  - BYOD (Bring Your Own Device)
  - COPE (Corporate Owned, Personally Enabled)
  - CYOD (Choose Your Own Device)
  - Corporate-owned devices

##### Workstation Hardening
- [ ] 💻 **OS hardening**
  - Remove unnecessary services
  - Disable unused features
  - Apply security patches
  - Enable firewall

- [ ] 📦 **Application hardening**
  - Keep applications updated
  - Remove unused applications
  - Configure securely
  - Principle of least privilege

##### Server Hardening
- [ ] 🖥️ **Server security**
  - Minimize installed software
  - Close unnecessary ports
  - Strong authentication
  - Regular patching

- [ ] ⚙️ **Service configuration**
  - Disable default accounts
  - Change default passwords
  - Secure protocols only
  - Access controls

##### Embedded Systems and IoT
- [ ] 🔌 **Embedded system security**
  - Firmware updates
  - Change default credentials
  - Network segmentation
  - Physical security

- [ ] 🌐 **IoT hardening**
  - Unique passwords per device
  - Disable unnecessary features
  - Network isolation
  - Regular updates

- [ ] 🏭 **SCADA/ICS hardening**
  - Air-gapped networks
  - Limited remote access
  - Physical security controls
  - Specialized security tools

#### Video 3: Wireless Security Settings
- [ ] **Watched Professor Messer's "Wireless Security Settings" video**

##### Wireless Encryption
- [ ] ⛔ **WEP (Wired Equivalent Privacy)**
  - Deprecated and insecure
  - Easily cracked
  - Never use

- [ ] 🔓 **WPA (Wi-Fi Protected Access)**
  - TKIP encryption
  - Better than WEP
  - Still vulnerable

- [ ] 🔒 **WPA2**
  - AES encryption (CCMP)
  - Strong security
  - Current standard
  - Personal and Enterprise modes

- [ ] 🔐 **WPA3**
  - Enhanced encryption
  - SAE (Simultaneous Authentication of Equals)
  - Forward secrecy
  - Protection against offline attacks
  - Personal and Enterprise modes

##### Authentication Methods
- [ ] 🔑 **Pre-Shared Key (PSK)**
  - WPA2-Personal / WPA3-Personal
  - Shared password
  - Simpler deployment
  - Less secure for enterprise

- [ ] 🏢 **Enterprise authentication**
  - WPA2-Enterprise / WPA3-Enterprise
  - 802.1X authentication
  - RADIUS server
  - Individual user credentials

- [ ] 🔐 **AAA Framework**
  - Authentication: Prove identity
  - Authorization: Grant access
  - Accounting: Track usage
  - RADIUS and TACACS+ protocols

- [ ] 🔑 **EAP methods**
  - EAP-TLS (certificate-based)
  - PEAP (Protected EAP)
  - EAP-TTLS (Tunneled TLS)
  - EAP-FAST (Flexible Authentication via Secure Tunneling)

##### Additional Security
- [ ] 📡 **SSID management**
  - Change default SSID
  - Hide SSID broadcast (security through obscurity)
  - Multiple SSIDs for segmentation

- [ ] 🏷️ **MAC filtering**
  - Whitelist approved devices
  - Easily spoofed
  - Administrative overhead
  - Additional layer only

- [ ] ⚠️ **Wi-Fi Protected Setup (WPS)**
  - Easy setup feature
  - Security vulnerability
  - PIN brute force attack
  - Should be disabled

#### Video 4: Securing Wireless and Mobile
- [ ] **Watched Professor Messer's "Securing Wireless and Mobile" video**

##### Wireless Site Surveys
- [ ] 📡 **Site survey process**
  - Identify coverage areas
  - Detect interference
  - Optimize AP placement
  - Identify rogue APs

- [ ] 🗺️ **Heat mapping**
  - Visual coverage representation
  - Signal strength mapping
  - Dead zones identification
  - Overlap planning

##### Mobile Security
- [ ] 📶 **Mobile connection methods**
  - Cellular (4G, 5G)
  - Wi-Fi
  - Bluetooth
  - NFC (Near Field Communication)

- [ ] 🔒 **Mobile security features**
  - Biometric authentication
  - Device encryption
  - Remote wipe
  - GPS tracking
  - Containerization

- [ ] 📱 **Mobile application security**
  - App vetting process
  - Official app stores only
  - Permission management
  - App sandboxing

- [ ] 📍 **Geofencing**
  - Geographic boundaries
  - Location-based policies
  - Restrict access by location
  - Compliance enforcement

##### BYOD Considerations
- [ ] 📱 **BYOD policies**
  - Acceptable use
  - Security requirements
  - Data ownership
  - Privacy concerns

- [ ] 📦 **Containerization**
  - Separate personal and work data
  - Corporate data protection
  - Selective wipe capability
  - App isolation

#### Video 5: Application Security
- [ ] **Watched Professor Messer's "Application Security" video**

##### Application Hardening
- [ ] ✅ **Input validation**
  - Sanitize user input
  - Whitelist validation
  - Prevent injection attacks
  - Length and type checking

- [ ] 🍪 **Secure cookies**
  - HttpOnly flag
  - Secure flag (HTTPS only)
  - SameSite attribute
  - Expiration timeouts

- [ ] 📝 **Static code analysis**
  - Review source code
  - Automated scanning
  - Find vulnerabilities early
  - Before deployment

- [ ] 🔄 **Dynamic code analysis**
  - Runtime testing
  - Fuzzing
  - Penetration testing
  - Production-like environment

##### Application Security Techniques
- [ ] ✍️ **Code signing**
  - Verify software integrity
  - Confirm publisher identity
  - Digital signatures
  - Certificate-based

- [ ] 📦 **Sandboxing**
  - Isolated execution environment
  - Limited access to system
  - Test suspicious code
  - Malware analysis

- [ ] 📊 **Monitoring**
  - Application logs
  - Performance metrics
  - Security events
  - Anomaly detection

**Notes:**
[Add your study notes here as you learn]

---

### 4.2 Explain the security implications of proper hardware, software, and data asset management

#### Video 1: Asset Management
- [ ] **Watched Professor Messer's "Asset Management" video**

##### Asset Tracking
- [ ] 📋 **Asset inventory**
  - Hardware assets
  - Software assets
  - Data assets
  - Digital assets

- [ ] 🔍 **Inventory systems**
  - Automated discovery
  - Manual tracking
  - Asset tags and barcodes
  - RFID tracking

- [ ] 🏷️ **Asset classification**
  - Criticality rating
  - Sensitivity level
  - Business value
  - Replacement cost

##### Procurement and Lifecycle
- [ ] 🛒 **Procurement process**
  - Vendor selection
  - Security requirements
  - Contract terms
  - Supply chain security

- [ ] 🔄 **Asset lifecycle**
  - Acquisition
  - Deployment
  - Maintenance
  - Retirement/disposal

- [ ] 👤 **Assignment and ownership**
  - User assignment
  - Department allocation
  - Custodian responsibility
  - Change tracking

##### Media Sanitization
- [ ] 🗑️ **Data destruction methods**
  - Overwriting
  - Degaussing
  - Physical destruction
  - Cryptographic erasure

- [ ] 📏 **Sanitization standards**
  - NIST SP 800-88
  - DoD 5220.22-M
  - Multiple pass overwrite
  - Verification required

- [ ] 💥 **Physical destruction**
  - Shredding
  - Pulverizing
  - Incineration
  - Certificate of destruction

##### Disposal and Decommissioning
- [ ] ♻️ **Secure disposal**
  - Data wiping
  - Physical destruction
  - Environmental concerns
  - Recycling programs

- [ ] 📄 **Certificate of destruction**
  - Proof of proper disposal
  - Audit trail
  - Compliance requirement
  - Third-party verification

**Notes:**
[Add your study notes here as you learn]

---

### 4.3 Explain various activities associated with vulnerability management

#### Video 1: Vulnerability Scanning
- [ ] **Watched Professor Messer's "Vulnerability Scanning" video**

##### Vulnerability Scanning Basics
- [ ] 🔍 **Vulnerability scanner**
  - Automated security testing
  - Identify known vulnerabilities
  - Regular scanning schedule
  - Continuous monitoring

- [ ] 📡 **Scanning types**
  - Network scanning
  - Application scanning
  - Database scanning
  - Web application scanning

- [ ] ⏰ **Scanning frequency**
  - Scheduled scans (daily, weekly, monthly)
  - On-demand scans
  - Continuous scanning
  - After changes

##### Scanning Methods
- [ ] 🔑 **Credentialed vs. non-credentialed**
  - Credentialed: Authenticated access, deeper scan
  - Non-credentialed: External perspective
  - Both provide value
  - Different vulnerability visibility

- [ ] 🖥️ **Agent-based vs. agent-less**
  - Agent-based: Software on target
  - Agent-less: Network-based scanning
  - Trade-offs in coverage and performance

- [ ] 👀 **Passive vs. active scanning**
  - Passive: Monitor traffic, no interaction
  - Active: Probe systems directly
  - Passive less intrusive
  - Active more comprehensive

##### Scan Configuration
- [ ] 🎯 **Scope definition**
  - IP ranges
  - Asset lists
  - Exclusions
  - Scan windows

- [ ] ⚙️ **Scan sensitivity**
  - Intrusive vs. non-intrusive
  - Production impact
  - False positive rate
  - Compliance requirements

#### Video 2: Analyzing Vulnerabilities
- [ ] **Watched Professor Messer's "Analyzing Vulnerabilities" video**

##### Vulnerability Assessment
- [ ] 📚 **Vulnerability databases**
  - CVE (Common Vulnerabilities and Exposures)
  - NVD (National Vulnerability Database)
  - Vendor advisories
  - Security research

- [ ] 📊 **CVSS (Common Vulnerability Scoring System)**
  - Severity rating (0-10)
  - Base score, temporal score, environmental score
  - Critical, High, Medium, Low
  - Prioritization tool

- [ ] 🏷️ **Vulnerability categorization**
  - By severity
  - By asset criticality
  - By exploitability
  - By business impact

##### Analysis Process
- [ ] ⚠️ **False positives**
  - Incorrectly identified vulnerabilities
  - Verify findings
  - Tune scanner
  - Document exceptions

- [ ] ❌ **False negatives**
  - Missed vulnerabilities
  - Scanner limitations
  - Configuration issues
  - Validation testing

- [ ] ✅ **Validation**
  - Confirm vulnerabilities
  - Reproduce findings
  - Assess exploitability
  - Determine impact

##### Reporting
- [ ] 📄 **Vulnerability reports**
  - Executive summary
  - Technical details
  - Affected systems
  - Remediation recommendations

- [ ] ⚖️ **Risk assessment**
  - Likelihood of exploit
  - Potential impact
  - Business context
  - Risk prioritization

#### Video 3: Vulnerability Remediation
- [ ] **Watched Professor Messer's "Vulnerability Remediation" video**

##### Remediation Strategies
- [ ] 🔧 **Patching**
  - Apply security patches
  - Vendor updates
  - Testing before deployment
  - Emergency patches

- [ ] ⚙️ **Configuration changes**
  - Harden settings
  - Remove unnecessary services
  - Update credentials
  - Apply baselines

- [ ] 🛡️ **Compensating controls**
  - Alternative mitigations
  - When patching not possible
  - Temporary measures
  - Risk acceptance

- [ ] 🔀 **Isolation/segmentation**
  - Network segmentation
  - VLAN separation
  - Quarantine systems
  - Air gaps

##### Patch Management
- [ ] 📋 **Patch management process**
  - Identify patches
  - Test patches
  - Approve patches
  - Deploy patches
  - Verify deployment

- [ ] 📅 **Patch scheduling**
  - Critical patches immediately
  - Regular patch cycles
  - Maintenance windows
  - Emergency procedures

- [ ] 🧪 **Patch testing**
  - Lab environment
  - Pilot deployment
  - Compatibility testing
  - Rollback procedures

##### Remediation Tracking
- [ ] ✅ **Tracking and validation**
  - Remediation status
  - Verification scanning
  - Closure documentation
  - Trend analysis

- [ ] ⏱️ **SLA compliance**
  - Remediation timelines
  - Critical: 30 days or less
  - High: 60 days
  - Medium: 90 days

#### Video 4: Penetration Testing
- [ ] **Watched Professor Messer's "Penetration Testing" video**

##### Penetration Testing Basics
- [ ] 🎯 **Penetration test definition**
  - Authorized simulated attack
  - Identify exploitable vulnerabilities
  - Go beyond vulnerability scanning
  - Controlled environment

- [ ] 📋 **Rules of engagement**
  - Scope definition
  - Authorization
  - Timeframe
  - Communication plan
  - Legal considerations

##### Testing Types
- [ ] ⬜ **Known environment (White box)**
  - Full knowledge
  - Network diagrams, credentials
  - Internal perspective
  - Comprehensive assessment

- [ ] ⬛ **Unknown environment (Black box)**
  - No prior knowledge
  - External attacker perspective
  - Realistic scenario
  - Limited information

- [ ] ◼️ **Partially known environment (Gray box)**
  - Some information provided
  - Balanced approach
  - Efficient testing
  - Hybrid perspective

##### Testing Methods
- [ ] 🏢 **Physical penetration testing**
  - Physical security controls
  - Building access
  - Tailgating
  - Social engineering

- [ ] ⚔️ **Offensive testing**
  - Red team exercises
  - Simulate real attacks
  - Test defenses
  - Evade detection

- [ ] 🛡️ **Defensive testing**
  - Blue team exercises
  - Incident response
  - Detection capabilities
  - Defense improvements

- [ ] 🤝 **Integrated testing**
  - Purple team
  - Collaboration between red and blue
  - Knowledge sharing
  - Continuous improvement

##### Testing Activities
- [ ] 🔍 **Reconnaissance**
  - Information gathering
  - OSINT
  - Network mapping
  - Target identification

- [ ] 💥 **Initial exploitation**
  - Gain initial access
  - Exploit vulnerabilities
  - Social engineering
  - Phishing campaigns

- [ ] 📌 **Persistence**
  - Maintain access
  - Backdoors
  - Scheduled tasks
  - Registry modifications

- [ ] ⬆️ **Privilege escalation**
  - Gain higher privileges
  - Admin/root access
  - Exploit misconfigurations
  - Credential theft

- [ ] ↔️ **Lateral movement**
  - Move through network
  - Access other systems
  - Pivot points
  - Network traversal

- [ ] 🧹 **Cleanup**
  - Remove artifacts
  - Restore systems
  - Document findings
  - Detailed report

#### Video 5: Threat Intelligence
- [ ] **Watched Professor Messer's "Threat Intelligence" video**

##### Threat Intelligence Sources
- [ ] 🌐 **Open-source intelligence (OSINT)**
  - Publicly available
  - Social media
  - Forums and blogs
  - News sources

- [ ] 🔒 **Proprietary intelligence**
  - Commercial threat feeds
  - Paid subscriptions
  - Vendor-specific
  - Industry reports

- [ ] 🤝 **Information sharing**
  - ISACs (Information Sharing and Analysis Centers)
  - Government agencies
  - Industry partnerships
  - Peer organizations

##### Intelligence Types
- [ ] 🎯 **Strategic intelligence**
  - High-level trends
  - Long-term planning
  - Executive decisions
  - Business risk

- [ ] 🛠️ **Tactical intelligence**
  - TTPs (Tactics, Techniques, Procedures)
  - Attack methods
  - Operational defense
  - Security team use

- [ ] ⚡ **Operational intelligence**
  - Ongoing campaigns
  - Specific threats
  - Real-time data
  - Incident response

##### Threat Intelligence Platforms
- [ ] 📡 **Threat feeds**
  - IP reputation lists
  - Domain blacklists
  - File hashes (IOCs)
  - Automated integration

- [ ] 🔄 **STIX/TAXII**
  - STIX: Structured Threat Information Expression
  - TAXII: Trusted Automated Exchange of Intelligence Information
  - Standard formats
  - Automated sharing

- [ ] 🔎 **Threat hunting**
  - Proactive searching
  - Hypothesis-driven
  - Advanced threats
  - Continuous activity

**Notes:**
[Add your study notes here as you learn]

---

### 4.4 Explain security alerting and monitoring concepts and tools

#### Video 1: Security Monitoring
- [ ] **Watched Professor Messer's "Security Monitoring" video**

##### Monitoring Concepts
- [ ] 📊 **Log aggregation**
  - Centralize logs
  - Multiple sources
  - Correlation
  - Long-term storage

- [ ] 📁 **Log sources**
  - Firewall logs
  - IDS/IPS logs
  - System logs
  - Application logs
  - Authentication logs

- [ ] 🔔 **Alerting and thresholds**
  - Define alert conditions
  - Threshold levels
  - Alert fatigue prevention
  - Tuning required

##### Monitoring Activities
- [ ] 📋 **Scanning and reporting**
  - Regular scans
  - Compliance reports
  - Trend analysis
  - Dashboard views

- [ ] 🗄️ **Archiving**
  - Long-term retention
  - Compliance requirements
  - Forensic analysis
  - Secure storage

- [ ] 🚨 **Alert response**
  - Triage alerts
  - Investigate incidents
  - Escalation procedures
  - Documentation

##### Security Metrics
- [ ] 📈 **Key Performance Indicators (KPIs)**
  - Mean time to detect (MTTD)
  - Mean time to respond (MTTR)
  - False positive rate
  - Vulnerability remediation time

- [ ] 🛡️ **Security posture**
  - Overall security health
  - Compliance status
  - Risk level
  - Trend analysis

#### Video 2: Security Tools
- [ ] **Watched Professor Messer's "Security Tools" video**

##### SIEM Systems
- [ ] 🔍 **Security Information and Event Management (SIEM)**
  - Centralized logging
  - Real-time analysis
  - Correlation engine
  - Alerting and reporting

- [ ] ⚙️ **SIEM capabilities**
  - Log collection and aggregation
  - Normalization
  - Correlation rules
  - Incident detection
  - Compliance reporting

- [ ] 🔄 **SIEM workflow**
  - Data collection
  - Parsing and normalization
  - Correlation and analysis
  - Alert generation
  - Incident response

##### Security Orchestration
- [ ] 🤖 **SOAR (Security Orchestration, Automation, and Response)**
  - Automate workflows
  - Integrate tools
  - Playbooks
  - Reduce response time

- [ ] ✨ **Automation benefits**
  - Faster response
  - Consistent actions
  - Reduce human error
  - Scale operations

##### Additional Security Tools
- [ ] 📋 **SCAP (Security Content Automation Protocol)**
  - Standardized vulnerability information
  - Automated compliance checking
  - Configuration assessment
  - NIST standard

- [ ] 📚 **Benchmarks and frameworks**
  - CIS Benchmarks
  - STIG (Security Technical Implementation Guide)
  - Automated compliance
  - Configuration validation

- [ ] 🌐 **Network monitoring tools**
  - Packet analyzers (Wireshark)
  - NetFlow collectors
  - SNMP monitoring
  - Bandwidth monitors

**Notes:**
[Add your study notes here as you learn]

---

### 4.5 Given a scenario, modify enterprise capabilities to enhance security

#### Video 1: Operating System Security
- [ ] **Watched Professor Messer's "Operating System Security" video**

##### OS Hardening
- [ ] 🔄 **Updates and patches**
  - Security patches
  - Feature updates
  - Automated updates
  - Testing procedures

- [ ] 👤 **User accounts**
  - Disable unnecessary accounts
  - Strong password policies
  - Least privilege
  - Regular audits

- [ ] ⚙️ **Services and processes**
  - Disable unnecessary services
  - Startup programs
  - Background processes
  - Service accounts

##### OS Security Features
- [ ] 🪟 **Windows security**
  - Windows Defender
  - BitLocker encryption
  - Windows Firewall
  - User Account Control (UAC)
  - Group Policy

- [ ] 🐧 **Linux security**
  - SELinux / AppArmor
  - iptables / nftables
  - sudo configuration
  - File permissions
  - Package management

- [ ] 🍎 **macOS security**
  - Gatekeeper
  - FileVault encryption
  - XProtect antivirus
  - System Integrity Protection (SIP)

##### Additional OS Security
- [ ] 🔧 **Registry/configuration hardening**
  - Registry security (Windows)
  - Configuration files (Linux/Unix)
  - Secure defaults
  - Audit settings

- [ ] 📝 **Logging and auditing**
  - Enable audit logs
  - Monitor security events
  - Log forwarding
  - Retention policies

#### Video 2: Endpoint Security
- [ ] **Watched Professor Messer's "Endpoint Security" video**

##### Endpoint Protection
- [ ] 🦠 **Antivirus / Anti-malware**
  - Signature-based detection
  - Heuristic analysis
  - Real-time protection
  - Regular updates

- [ ] 🎯 **Endpoint Detection and Response (EDR)**
  - Advanced threat detection
  - Behavioral analysis
  - Automated response
  - Forensic capabilities

- [ ] 🔥 **Host-based firewalls**
  - Inbound/outbound filtering
  - Application control
  - Network profiles
  - Logging

- [ ] 👁️ **Host-based IDS/IPS (HIDS/HIPS)**
  - Monitor host activities
  - Detect suspicious behavior
  - Prevent exploits
  - File integrity monitoring

##### Endpoint Management
- [ ] 🛡️ **Data Loss Prevention (DLP)**
  - Monitor data movement
  - Prevent exfiltration
  - USB control
  - Email filtering
  - Cloud DLP

- [ ] 📱 **Application control**
  - Application whitelisting
  - Software restriction policies
  - Prevent unauthorized apps
  - Execution control

- [ ] 🔐 **Encryption**
  - Full-disk encryption
  - File-level encryption
  - Removable media encryption
  - Self-encrypting drives (SED)

#### Video 3: Email Security
- [ ] **Watched Professor Messer's "Email Security" video**

##### Email Security Technologies
- [ ] ✉️ **Email encryption**
  - TLS for transport
  - S/MIME for end-to-end
  - PGP/GPG
  - Digital signatures

- [ ] 🚫 **Anti-spam**
  - Spam filtering
  - Bayesian filters
  - Reputation lists
  - Content analysis

- [ ] 🎣 **Anti-phishing**
  - Link analysis
  - Sender verification
  - User training
  - Reporting mechanisms

- [ ] 📧 **DLP for email**
  - Content inspection
  - Policy enforcement
  - Block sensitive data
  - Quarantine

##### Email Authentication
- [ ] 📤 **SPF (Sender Policy Framework)**
  - Authorized sender IPs
  - DNS-based
  - Prevent spoofing

- [ ] 🔑 **DKIM (DomainKeys Identified Mail)**
  - Email signing
  - Cryptographic verification
  - Message integrity

- [ ] ✅ **DMARC (Domain-based Message Authentication, Reporting & Conformance)**
  - Policy framework
  - SPF and DKIM alignment
  - Reporting
  - Reject/quarantine

- [ ] 🚪 **Email gateways**
  - Centralized filtering
  - Cloud-based or on-premises
  - Multiple security layers
  - Sandboxing

#### Video 4: Secure Protocols
- [ ] **Watched Professor Messer's "Secure Protocols" video**

##### Protocol Security
- [ ] 🔒 **HTTPS (HTTP Secure)**
  - TLS/SSL encryption
  - Port 443
  - Certificate-based
  - Replace HTTP

- [ ] 📁 **FTPS (FTP Secure)**
  - FTP over TLS/SSL
  - Explicit or implicit
  - Encrypted file transfer

- [ ] 🔐 **SFTP (SSH File Transfer Protocol)**
  - FTP over SSH
  - Port 22
  - Strong encryption
  - Preferred over FTPS

- [ ] 🖥️ **SSH (Secure Shell)**
  - Encrypted remote access
  - Port 22
  - Key-based authentication
  - Replace Telnet

- [ ] 📊 **SNMPv3**
  - Encrypted SNMP
  - Authentication
  - Message integrity
  - Replace SNMPv1/v2

- [ ] 📖 **LDAPS (LDAP over SSL)**
  - Encrypted LDAP
  - Port 636
  - Secure directory access

- [ ] 📞 **SRTP (Secure Real-time Transport Protocol)**
  - Encrypted VoIP
  - AES encryption
  - Authentication

- [ ] ✉️ **S/MIME and PGP**
  - Email encryption
  - Digital signatures
  - End-to-end security

##### Legacy Protocol Mitigation
- [ ] ⛔ **Disable insecure protocols**
  - Telnet → SSH
  - HTTP → HTTPS
  - FTP → SFTP/FTPS
  - SNMPv1/v2 → SNMPv3

#### Video 5: Web Filtering
- [ ] **Watched Professor Messer's "Web Filtering" video**

##### Web Content Filtering
- [ ] 🌐 **URL filtering**
  - Category-based blocking
  - Blacklists and whitelists
  - Reputation-based
  - Real-time lookup

- [ ] 🔍 **Content filtering**
  - Keyword blocking
  - File type restrictions
  - MIME type filtering
  - Malware scanning

- [ ] 🔀 **Agent-based vs. centralized**
  - Agent on endpoint
  - Centralized proxy/gateway
  - Cloud-based filtering
  - Hybrid approaches

##### Web Security Technologies
- [ ] 🔀 **Web proxy**
  - Explicit proxy
  - Transparent proxy
  - Content caching
  - Access control

- [ ] 🔐 **SSL/TLS inspection**
  - Decrypt HTTPS traffic
  - Inspect encrypted content
  - Re-encrypt to destination
  - Certificate considerations

- [ ] 🛡️ **Web application firewall (WAF)**
  - HTTP/HTTPS protection
  - OWASP Top 10
  - SQL injection prevention
  - XSS protection

#### Video 6: Firewalls
- [ ] **Watched Professor Messer's "Firewalls" video**

##### Firewall Implementation
- [ ] 📋 **Firewall rules**
  - Access control lists (ACLs)
  - Allow/deny rules
  - Order matters
  - Default deny

- [ ] 🗺️ **Firewall zones**
  - Trust zones
  - Untrusted zones
  - DMZ
  - Internal networks

- [ ] 🚫 **Implicit deny**
  - Default deny all
  - Explicit allow rules
  - Security best practice
  - Whitelist approach

##### Firewall Management
- [ ] 🔍 **Rule review**
  - Regular audits
  - Remove unused rules
  - Consolidate rules
  - Document changes

- [ ] 🔥 **Firewall logs**
  - Allowed connections
  - Denied connections
  - Security events
  - Compliance auditing

- [ ] ⚡ **High availability**
  - Redundant firewalls
  - Active/passive
  - Active/active
  - State synchronization

#### Video 7: Monitoring Data
- [ ] **Watched Professor Messer's "Monitoring Data" video**

##### Data Sources
- [ ] 🌐 **Network traffic**
  - Packet captures
  - NetFlow/sFlow
  - Bandwidth monitoring
  - Protocol analysis

- [ ] 📄 **System logs**
  - Syslog
  - Windows Event Logs
  - Authentication logs
  - Application logs

- [ ] 🔥 **Firewall logs**
  - Connection attempts
  - Allowed traffic
  - Denied traffic
  - Policy violations

- [ ] 🚨 **IDS/IPS logs**
  - Alert logs
  - Signatures triggered
  - Attack attempts
  - False positives

##### Data Analysis
- [ ] 🔗 **Log correlation**
  - Multiple log sources
  - Timeline reconstruction
  - Pattern identification
  - Incident detection

- [ ] 📊 **Baseline analysis**
  - Normal behavior
  - Anomaly detection
  - Deviation alerts
  - Threshold monitoring

- [ ] 📈 **Trend analysis**
  - Historical data
  - Predictive analysis
  - Capacity planning
  - Security posture

**Notes:**
[Add your study notes here as you learn]

---

### 4.6 Given a scenario, implement and maintain identity and access management

#### Video 1: Identity and Access Management
- [ ] **Watched Professor Messer's "Identity and Access Management" video**

##### IAM Concepts
- [ ] 🔍 **Identification**
  - Username
  - Email address
  - Employee ID
  - Unique identifier

- [ ] 🔑 **Authentication**
  - Prove identity
  - Password
  - Biometrics
  - Multi-factor

- [ ] ✅ **Authorization**
  - What access is granted
  - Permissions
  - Access control
  - Resource access

- [ ] 📊 **Accounting**
  - Track activities
  - Audit logs
  - Monitoring
  - Compliance

##### Identity Services
- [ ] 📖 **Directory services**
  - LDAP (Lightweight Directory Access Protocol)
  - Active Directory
  - Centralized management
  - Authentication source

- [ ] 🤝 **Federation**
  - Trusted relationships
  - Third-party authentication
  - SAML, OAuth, OpenID Connect
  - Single sign-on across organizations

- [ ] 🔐 **Single Sign-On (SSO)**
  - One login, multiple resources
  - Improved user experience
  - Centralized authentication
  - Reduced password fatigue

- [ ] 🔒 **Privileged Access Management (PAM)**
  - Elevated privileges
  - Admin account control
  - Just-in-time access
  - Session recording

##### Identity Provisioning
- [ ] ➕ **Account creation**
  - Automated provisioning
  - User lifecycle
  - Role-based provisioning
  - Approval workflows

- [ ] 🔧 **Account maintenance**
  - Regular reviews
  - Permission updates
  - Role changes
  - Recertification

- [ ] ➖ **Account deprovisioning**
  - Termination procedures
  - Disable accounts
  - Remove access
  - Archive data

#### Video 2: Multifactor Authentication
- [ ] **Watched Professor Messer's "Multifactor Authentication" video**

##### Authentication Factors
- [ ] 🧠 **Something you know**
  - Password
  - PIN
  - Security questions
  - Passphrases

- [ ] 🔑 **Something you have**
  - Smart card
  - Hardware token
  - Soft token (mobile app)
  - USB security key

- [ ] 👤 **Something you are**
  - Fingerprint
  - Face recognition
  - Iris scan
  - Voice recognition

- [ ] 📍 **Somewhere you are**
  - Geolocation
  - IP address
  - Network location
  - GPS coordinates

- [ ] ✋ **Something you do**
  - Typing patterns
  - Gait analysis
  - Signature
  - Behavioral biometrics

##### MFA Implementation
- [ ] 🔐 **MFA methods**
  - SMS codes (less secure)
  - Authenticator apps (TOTP)
  - Push notifications
  - Hardware tokens
  - Biometric + password

- [ ] ⏰ **TOTP (Time-based One-Time Password)**
  - Google Authenticator
  - Microsoft Authenticator
  - Time-synchronized
  - 30-second intervals

- [ ] 🔢 **HOTP (HMAC-based One-Time Password)**
  - Counter-based
  - Generates codes on demand
  - No time synchronization

- [ ] 📱 **Push notifications**
  - Mobile app approval
  - Out-of-band authentication
  - User confirmation required
  - Context information

##### Advanced Authentication
- [ ] 🧠 **Adaptive authentication**
  - Risk-based
  - Context-aware
  - Behavioral analysis
  - Dynamic requirements

- [ ] 📜 **Certificate-based authentication**
  - Digital certificates
  - PKI infrastructure
  - Mutual authentication
  - Smart cards

#### Video 3: Password Security
- [ ] **Watched Professor Messer's "Password Security" video**

##### Password Best Practices
- [ ] 🔐 **Password complexity**
  - Length requirements (minimum 8-12 characters)
  - Character variety
  - Upper/lowercase
  - Numbers and symbols

- [ ] 📋 **Password policies**
  - Minimum length
  - Complexity requirements
  - Expiration (if used)
  - History (prevent reuse)
  - Account lockout

- [ ] 🔑 **Password managers**
  - Generate strong passwords
  - Encrypted storage
  - Auto-fill capability
  - Sync across devices

- [ ] 🚀 **Passwordless authentication**
  - FIDO2
  - Windows Hello
  - Biometrics
  - Security keys

##### Password Attacks and Defense
- [ ] ⚔️ **Password attacks**
  - Brute force
  - Dictionary attacks
  - Password spraying
  - Credential stuffing

- [ ] 🔒 **Password storage**
  - Never plain text
  - Hashing (bcrypt, Argon2)
  - Salting
  - Key stretching

- [ ] 🔄 **Password reset procedures**
  - Self-service portal
  - Identity verification
  - Secure delivery method
  - Temporary passwords

##### Account Security
- [ ] 🔒 **Account lockout**
  - Failed login threshold
  - Lockout duration
  - Administrator unlock
  - Prevent brute force

- [ ] ⏰ **Password age**
  - Regular changes (controversial)
  - Balance security and usability
  - Modern guidance: change when compromised

#### Video 4: Access Controls
- [ ] **Watched Professor Messer's "Access Controls" video**

##### Access Control Models
- [ ] 🔒 **MAC (Mandatory Access Control)**
  - Classification-based
  - System-enforced
  - Labels (Top Secret, Secret, etc.)
  - Government/military use

- [ ] 👤 **DAC (Discretionary Access Control)**
  - Owner-controlled
  - User discretion
  - ACLs (Access Control Lists)
  - Common in business

- [ ] 👥 **RBAC (Role-Based Access Control)**
  - Role-based permissions
  - Group membership
  - Easier administration
  - Most common model

- [ ] 🏷️ **ABAC (Attribute-Based Access Control)**
  - Attribute-based decisions
  - Dynamic policies
  - Context-aware
  - Fine-grained control

##### Access Control Principles
- [ ] 🔐 **Least privilege**
  - Minimum necessary access
  - Default deny
  - Just enough permissions
  - Regular review

- [ ] 🔀 **Separation of duties**
  - Divide responsibilities
  - No single person controls entire process
  - Fraud prevention
  - Dual control

- [ ] ⏰ **Time-of-day restrictions**
  - Limit access hours
  - Business hours only
  - Reduce attack window
  - Compliance requirement

- [ ] 📍 **Location-based restrictions**
  - Geographic limitations
  - IP address filtering
  - Geofencing
  - VPN requirements

##### Access Review
- [ ] 🔍 **User access review**
  - Regular audits
  - Quarterly or annual
  - Remove unnecessary access
  - Recertification

- [ ] ⬆️ **Privilege creep**
  - Accumulation of permissions
  - Job changes
  - Regular cleanup
  - Access governance

**Notes:**
[Add your study notes here as you learn]

---

### 4.7 Explain the importance of automation and orchestration related to secure operations

#### Video 1: Scripting and Automation
- [ ] **Watched Professor Messer's "Scripting and Automation" video**

##### Automation Benefits
- [ ] ⚡ **Efficiency**
  - Reduce manual effort
  - Faster execution
  - Consistent results
  - Scale operations

- [ ] ✅ **Accuracy**
  - Eliminate human error
  - Repeatable processes
  - Standardized configurations
  - Reliable outcomes

- [ ] 📊 **Continuous monitoring**
  - 24/7 operations
  - Automated responses
  - Real-time alerts
  - Proactive detection

##### Automation Use Cases
- [ ] ➕ **User provisioning**
  - Automated account creation
  - Role-based access
  - Onboarding workflows
  - Deprovisioning

- [ ] 🎫 **Ticket creation**
  - Automated incident tickets
  - SIEM integration
  - Workflow triggers
  - Assignment rules

- [ ] 🚨 **Security responses**
  - Isolate compromised systems
  - Block malicious IPs
  - Quarantine files
  - Reset passwords

- [ ] ✅ **Compliance**
  - Automated audits
  - Configuration checks
  - Report generation
  - Remediation tracking

##### Scripting
- [ ] 💻 **Scripting languages**
  - PowerShell (Windows)
  - Bash/Shell (Linux)
  - Python (cross-platform)
  - Perl, Ruby

- [ ] 📝 **Use cases**
  - Log analysis
  - Security scanning
  - Configuration management
  - Data processing

- [ ] ✨ **Best practices**
  - Version control
  - Code review
  - Testing
  - Documentation
  - Error handling

##### Orchestration
- [ ] 🎭 **Security orchestration**
  - Coordinate multiple tools
  - Automated workflows
  - Playbooks
  - Integration platform

- [ ] 🤖 **SOAR capabilities**
  - Security orchestration
  - Automation
  - Response
  - Case management

- [ ] 📝 **Infrastructure as Code**
  - Terraform
  - Ansible
  - Puppet, Chef
  - CloudFormation

**Notes:**
[Add your study notes here as you learn]

---

### 4.8 Explain appropriate incident response activities

#### Video 1: Incident Planning
- [ ] **Watched Professor Messer's "Incident Planning" video**

##### Incident Response Planning
- [ ] 📋 **Incident response plan**
  - Documented procedures
  - Roles and responsibilities
  - Communication plan
  - Escalation procedures

- [ ] 👥 **Incident response team**
  - Security analysts
  - IT staff
  - Management
  - Legal counsel
  - PR/communications
  - External partners

- [ ] 📞 **Communication plan**
  - Internal notifications
  - External communications
  - Media relations
  - Customer notifications
  - Regulatory reporting

##### Incident Response Testing
- [ ] 📋 **Tabletop exercises**
  - Discussion-based
  - Walk through scenarios
  - Test procedures
  - Identify gaps
  - Low cost, low risk

- [ ] 🎭 **Simulations**
  - Live exercises
  - Simulated incidents
  - Test tools and processes
  - Practice coordination
  - More realistic

- [ ] 🔍 **Root cause analysis**
  - Identify underlying cause
  - Prevent recurrence
  - Improve processes
  - Lessons learned

##### Documentation
- [ ] 📝 **Incident documentation**
  - Timeline of events
  - Actions taken
  - Evidence collected
  - Decisions made
  - Lessons learned

- [ ] 📅 **Retention requirements**
  - Legal requirements
  - Compliance mandates
  - Organization policy
  - Evidence preservation

#### Video 2: Incident Response
- [ ] **Watched Professor Messer's "Incident Response" video**

##### Incident Response Process
- [ ] 1️⃣ **1. Preparation**
  - Tools and resources
  - Training
  - Policies and procedures
  - Communication plan

- [ ] 2️⃣ **2. Identification (Detection)**
  - Detect incidents
  - Determine scope
  - Categorize severity
  - Alert appropriate parties

- [ ] 3️⃣ **3. Containment**
  - **Short-term containment**
    - Immediate actions
    - Isolate systems
    - Prevent spread
    - Quick response
  - **Long-term containment**
    - Temporary fixes
    - Clean systems
    - Apply patches
    - Monitor activity

- [ ] 4️⃣ **4. Eradication**
  - Remove threat
  - Delete malware
  - Patch vulnerabilities
  - Reset passwords
  - Fix root cause

- [ ] 5️⃣ **5. Recovery**
  - Restore systems
  - Verify normal operation
  - Monitor for reinfection
  - Return to production
  - Gradual restoration

- [ ] 6️⃣ **6. Lessons Learned**
  - Post-incident review
  - What went well
  - What needs improvement
  - Update procedures
  - Document findings

##### Containment Strategies
- [ ] 🔒 **Isolation**
  - Disconnect from network
  - Physical isolation
  - VLAN segmentation
  - Firewall rules

- [ ] 🔀 **Segmentation**
  - Limit lateral movement
  - Contain to segment
  - Protect critical assets
  - Micro-segmentation

##### Evidence Handling
- [ ] 📦 **Evidence collection**
  - Order of volatility
  - Proper procedures
  - Chain of custody
  - Forensic tools

- [ ] ⚖️ **Legal hold**
  - Preserve evidence
  - Litigation support
  - Regulatory compliance
  - No destruction

#### Video 3: Digital Forensics
- [ ] **Watched Professor Messer's "Digital Forensics" video**

##### Forensics Process
- [ ] 📊 **Order of volatility**
  - Most volatile first
  - CPU cache and registers
  - RAM (memory dump)
  - Network traffic
  - Disk drives
  - Backups and logs

- [ ] 💾 **Data acquisition**
  - Forensic imaging
  - Bit-by-bit copy
  - Write blockers
  - Hash verification
  - Maintain integrity

- [ ] 🔗 **Chain of custody**
  - Document handling
  - Who, what, when, where
  - Maintain integrity
  - Court admissibility
  - Unbroken chain

##### Forensic Techniques
- [ ] 🧠 **Memory forensics**
  - RAM analysis
  - Running processes
  - Network connections
  - Encryption keys
  - Volatile data

- [ ] 💿 **Disk forensics**
  - File system analysis
  - Deleted file recovery
  - Timeline analysis
  - Metadata examination

- [ ] 🌐 **Network forensics**
  - Packet capture
  - Flow analysis
  - Protocol analysis
  - Network logs

##### Legal and Compliance
- [ ] 🔍 **E-discovery**
  - Electronic evidence
  - Legal proceedings
  - Search and collection
  - Relevance and privilege

- [ ] 📄 **Forensic reporting**
  - Detailed findings
  - Technical analysis
  - Timeline reconstruction
  - Expert testimony

- [ ] ⚖️ **Legal considerations**
  - Admissibility
  - Privacy laws
  - Data protection
  - Jurisdictional issues

**Notes:**
[Add your study notes here as you learn]

---

## Key Terms & Definitions

**Term** | **Definition**
---------|---------------
**Baseline** | Standard configuration for security settings
**MDM** | Mobile Device Management - centralized mobile management
**CVE** | Common Vulnerabilities and Exposures - vulnerability database
**CVSS** | Common Vulnerability Scoring System - severity rating
**Penetration Test** | Authorized simulated cyberattack
**SIEM** | Security Information and Event Management - log analysis platform
**SOAR** | Security Orchestration, Automation, and Response
**EDR** | Endpoint Detection and Response - advanced endpoint security
**DLP** | Data Loss Prevention - prevent data exfiltration
**SPF** | Sender Policy Framework - email authentication
**DKIM** | DomainKeys Identified Mail - email signing
**DMARC** | Domain-based Message Authentication - email policy
**SSO** | Single Sign-On - one login, multiple resources
**MFA** | Multifactor Authentication - multiple authentication factors
**TOTP** | Time-based One-Time Password - authenticator app codes
**PAM** | Privileged Access Management - admin account control
**RBAC** | Role-Based Access Control - permissions by role
**ABAC** | Attribute-Based Access Control - policy-based access
**IRP** | Incident Response Plan - documented IR procedures
**Chain of Custody** | Documentation of evidence handling

---

## Acronyms to Know

- **AAA** - Authentication, Authorization, and Accounting
- **ABAC** - Attribute-Based Access Control
- **ACL** - Access Control List
- **BYOD** - Bring Your Own Device
- **COPE** - Corporate Owned, Personally Enabled
- **CVE** - Common Vulnerabilities and Exposures
- **CVSS** - Common Vulnerability Scoring System
- **CYOD** - Choose Your Own Device
- **DAC** - Discretionary Access Control
- **DKIM** - DomainKeys Identified Mail
- **DLP** - Data Loss Prevention
- **DMARC** - Domain-based Message Authentication, Reporting & Conformance
- **EDR** - Endpoint Detection and Response
- **HIDS** - Host-based Intrusion Detection System
- **HIPS** - Host-based Intrusion Prevention System
- **HOTP** - HMAC-based One-Time Password
- **IRP** - Incident Response Plan
- **MAC** - Mandatory Access Control
- **MDM** - Mobile Device Management
- **MFA** - Multifactor Authentication
- **NVD** - National Vulnerability Database
- **PAM** - Privileged Access Management
- **RBAC** - Role-Based Access Control
- **SCAP** - Security Content Automation Protocol
- **SIEM** - Security Information and Event Management
- **SOAR** - Security Orchestration, Automation, and Response
- **SPF** - Sender Policy Framework
- **SSO** - Single Sign-On
- **TOTP** - Time-based One-Time Password
- **WAF** - Web Application Firewall
- **WPA** - Wi-Fi Protected Access

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

## IAM Relevance (CRITICAL FOR YOUR CAREER!)

### Identity and Access Management Focus
- **Section 4.6** is dedicated entirely to IAM
- Identity services (LDAP, AD, federation, SSO)
- Authentication methods and MFA
- Access control models (RBAC, ABAC, MAC, DAC)
- Privileged access management (PAM)
- Password security and passwordless authentication

### IAM Throughout Domain 4
- Hardening: Secure authentication on all systems
- Asset Management: Track identity and access systems
- Monitoring: Audit authentication and authorization events
- Incident Response: Identity-based attacks and compromises
- Automation: Automated provisioning and deprovisioning

### Key IAM Concepts to Master
- Federation and SSO implementations
- MFA technologies and best practices
- Access control principles (least privilege, separation of duties)
- Identity lifecycle management
- Privileged account security

---

## Exam Tips

- **This is 28% of exam** - Largest domain, allocate significant study time!
- **Know incident response phases** - Preparation, Identification, Containment, Eradication, Recovery, Lessons Learned
- **Understand MFA factors** - Something you know/have/are, somewhere you are
- **Memorize secure protocols** - HTTPS, SFTP, SSH, SNMPv3, SRTP
- **Know wireless security** - WPA2/WPA3, 802.1X, EAP methods
- **Vulnerability management** - Scanning, analysis, remediation, verification
- **SIEM vs. SOAR** - Understand the differences and uses
- **Access control models** - MAC, DAC, RBAC, ABAC
- **Order of volatility** - Memory before disk in forensics
- **Email security protocols** - SPF, DKIM, DMARC

---

## Related Labs

- [Link to relevant hands-on labs]

---

**Study Status:** Not Started
**Confidence Level:** [1-5]
**Last Reviewed:** [Date]

---

**Note:** Domain 4 is the LARGEST domain at 28% of the exam. It covers the day-to-day security operations that are critical for any security role. Section 4.6 (IAM) is especially relevant for your career focus!
