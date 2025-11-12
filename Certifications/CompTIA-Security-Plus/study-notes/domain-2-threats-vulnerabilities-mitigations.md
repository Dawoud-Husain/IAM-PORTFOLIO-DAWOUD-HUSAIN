# Domain 2: Threats, Vulnerabilities, and Mitigations (22%)

## Domain Weight: 22% of exam (LARGEST DOMAIN!)

---

## Professor Messer's Video Structure

This study guide follows Professor Messer's CompTIA SY0-701 Security+ Training Course structure.
**Total Videos in Domain 2: 35+ videos**

### Section 2.1 - Compare and contrast common threat actors and motivations (2 videos)
- [ ] Threat Actors
- [ ] Threat Intelligence

### Section 2.2 - Explain common threat vectors and attack surfaces (5 videos)
- [ ] Common Threat Vectors
- [ ] Phishing
- [ ] Impersonation
- [ ] Watering Hole Attacks
- [ ] Other Social Engineering Attacks

### Section 2.3 - Explain various types of vulnerabilities (15 videos)
- [ ] Operating System Vulnerabilities
- [ ] Zero-day Vulnerabilities
- [ ] Hardware Vulnerabilities
- [ ] Cloud-specific Vulnerabilities
- [ ] Supply Chain Vulnerabilities
- [ ] Mobile Device Vulnerabilities
- [ ] Misconfiguration Vulnerabilities
- [ ] Cross-site Scripting
- [ ] SQL Injection
- [ ] Malicious Updates
- [ ] Race Conditions
- [ ] Buffer Overflows
- [ ] Memory Injections
- [ ] Cryptographic Vulnerabilities
- [ ] Virtualization Vulnerabilities

### Section 2.4 - Given a scenario, analyze indicators of malicious activity (15 videos)
- [ ] An Overview of Malware
- [ ] Viruses and Worms
- [ ] Other Malware Types
- [ ] Spyware and Bloatware
- [ ] Malicious Code
- [ ] Password Attacks
- [ ] Application Attacks
- [ ] Denial of Service
- [ ] DNS Attacks
- [ ] Replay Attacks
- [ ] On-path Attacks
- [ ] Cryptographic Attacks
- [ ] Wireless Attacks
- [ ] Physical Attacks
- [ ] Indicators of Compromise

### Section 2.5 - Explain the purpose of mitigation techniques used to secure the enterprise (3 videos)
- [ ] Mitigation Techniques
- [ ] Segmentation and Access Control
- [ ] Hardening Techniques

---

## Topics Covered

### 2.1 Compare and contrast common threat actors and motivations

#### Video 1: Threat Actors
- [ ] **Watched Professor Messer's "Threat Actors" video**

##### Threat Actor Types
- [ ] 🏛️ **Nation states**
  - Government-sponsored
  - Highly sophisticated
  - Well-funded and organized
  - Motivations: espionage, data exfiltration, disruption

- [ ] 🎮 **Unskilled attackers**
  - Script kiddies
  - Limited technical knowledge
  - Use pre-built tools
  - Motivations: curiosity, recognition

- [ ] ✊ **Hacktivists**
  - Politically or socially motivated
  - Target organizations for ideological reasons
  - Public awareness campaigns
  - Defacement and data leaks

- [ ] 🚪 **Insider threats**
  - Internal employees or contractors
  - Authorized access to systems
  - Motivations: revenge, financial gain, negligence
  - Most difficult to detect

- [ ] 💰 **Organized crime**
  - Professional cybercriminals
  - Financial motivation
  - Ransomware, fraud, theft
  - Well-organized and funded

- [ ] 👻 **Shadow IT**
  - Unauthorized systems and applications
  - Employees bypassing IT controls
  - Security risks from unmanaged resources

##### Threat Actor Attributes
- [ ] 🔄 **Internal vs. external**
  - Inside the organization vs. outside attackers

- [ ] 💵 **Resources and funding**
  - Level of financial backing
  - Access to tools and infrastructure

- [ ] 🎯 **Level of sophistication**
  - Technical capabilities
  - Attack complexity

- [ ] 🧠 **Intent and motivation**
  - Data exfiltration
  - Service disruption
  - Financial gain
  - Philosophical/political reasons
  - Revenge
  - War/espionage

#### Video 2: Threat Intelligence
- [ ] **Watched Professor Messer's "Threat Intelligence" video**

##### Intelligence Sources
- [ ] 🌐 **Open-source intelligence (OSINT)**
  - Publicly available information
  - Social media, forums, websites
  - No special access required

- [ ] 🔒 **Closed/proprietary intelligence**
  - Commercial threat feeds
  - Paid subscription services
  - Specialized vendors

- [ ] 📚 **Vulnerability databases**
  - CVE (Common Vulnerabilities and Exposures)
  - NVD (National Vulnerability Database)
  - Vendor advisories

- [ ] 🤝 **Information sharing organizations**
  - ISACs (Information Sharing and Analysis Centers)
  - Industry-specific collaboration
  - Government partnerships

- [ ] 🕵️ **Dark web**
  - Underground forums
  - Criminal marketplaces
  - Credential dumps and exploits

##### Threat Intelligence Types
- [ ] 🎯 **Strategic intelligence**
  - High-level trends and risks
  - Long-term planning
  - Executive decision-making

- [ ] 🛠️ **Tactical intelligence**
  - TTPs (Tactics, Techniques, and Procedures)
  - Attacker behaviors
  - Operational defense

- [ ] ⚡ **Operational intelligence**
  - Specific campaigns and threats
  - Real-time information
  - Incident response

**Notes:**
[Add your study notes here as you learn]

---

### 2.2 Explain common threat vectors and attack surfaces

#### Video 1: Common Threat Vectors
- [ ] **Watched Professor Messer's "Common Threat Vectors" video**

##### Attack Vectors
- [ ] 📧 **Message-based**
  - Email attachments
  - Malicious links
  - SMS/text messages

- [ ] 🖼️ **Image-based**
  - Steganography
  - Malicious image files
  - SVG exploits

- [ ] 📄 **File-based**
  - Office documents with macros
  - PDF exploits
  - Zip files with malware

- [ ] 📞 **Voice calls**
  - Vishing (voice phishing)
  - Social engineering via phone
  - Caller ID spoofing

- [ ] 💾 **Removable devices**
  - USB drives
  - External hard drives
  - SD cards with malware

- [ ] 📡 **Unsecured networks**
  - Open Wi-Fi
  - Rogue access points
  - Man-in-the-middle attacks

##### Attack Surfaces
- [ ] 🔑 **Default credentials**
  - Unchanged passwords
  - Factory defaults
  - Common admin accounts

- [ ] 🔗 **Supply chain**
  - Third-party vendors
  - Software dependencies
  - Hardware tampering

- [ ] 🚪 **Open service ports**
  - Unnecessary services running
  - Unpatched vulnerabilities
  - Network exposure

- [ ] ⚙️ **Weak configurations**
  - Permissive firewall rules
  - Insecure protocols
  - Missing security controls

#### Video 2: Phishing
- [ ] **Watched Professor Messer's "Phishing" video**

##### Phishing Types
- [ ] 🎣 **Email phishing**
  - Mass emails to many targets
  - Generic messages
  - Fake login pages

- [ ] 🎯 **Spear phishing**
  - Targeted at specific individuals
  - Personalized content
  - Research on victims

- [ ] 🐋 **Whaling**
  - Target high-level executives
  - CEO fraud
  - Business email compromise (BEC)

- [ ] 📞 **Vishing**
  - Voice phishing via phone calls
  - Pretending to be IT support
  - Urgency and pressure tactics

- [ ] 📱 **Smishing**
  - SMS/text message phishing
  - Malicious links
  - Fake alerts and notifications

##### Phishing Techniques
- [ ] 🎭 **Pretexting**
  - Creating fabricated scenario
  - Building trust with victim
  - Information gathering

- [ ] ⌨️ **Typosquatting**
  - Similar-looking domain names
  - Common typos in URLs
  - Fake websites

- [ ] 📨 **Prepending**
  - Adding familiar terms to subject
  - RE: or FWD: in subject lines
  - Appears as reply to conversation

#### Video 3: Impersonation
- [ ] **Watched Professor Messer's "Impersonation" video**

##### Impersonation Techniques
- [ ] 🏢 **Brand impersonation**
  - Fake company websites
  - Spoofed logos and branding
  - Trusted company names

- [ ] 👤 **Identity fraud**
  - Stealing someone's identity
  - Using credentials
  - Posing as employees

- [ ] 💸 **Invoice scams**
  - Fake invoices
  - Wire transfer fraud
  - Vendor impersonation

- [ ] 🔓 **Credential harvesting**
  - Fake login pages
  - Collecting usernames and passwords
  - Account takeover

#### Video 4: Watering Hole Attacks
- [ ] **Watched Professor Messer's "Watering Hole Attacks" video**

##### Watering Hole Concepts
- [ ] 💧 **Attack methodology**
  - Target websites frequented by victims
  - Compromise legitimate sites
  - Wait for victims to visit

- [ ] 🦠 **Infection process**
  - Drive-by downloads
  - Browser exploits
  - Malware deployment

- [ ] 🛡️ **Defense strategies**
  - Keep browsers updated
  - Use security software
  - Restrict browsing

#### Video 5: Other Social Engineering Attacks
- [ ] **Watched Professor Messer's "Other Social Engineering Attacks" video**

##### Additional Attack Types
- [ ] 📰 **Misinformation and disinformation**
  - False information spread
  - Propaganda campaigns
  - Fake news

- [ ] 💼 **Business email compromise (BEC)**
  - Compromised email accounts
  - Fraudulent wire transfers
  - Executive impersonation

- [ ] 🚶 **Tailgating**
  - Following authorized person
  - Physical access without credentials
  - Piggybacking

- [ ] 🗑️ **Dumpster diving**
  - Searching trash for information
  - Discarded documents
  - Hardware disposal

- [ ] 👀 **Shoulder surfing**
  - Watching over someone's shoulder
  - Observing passwords and PINs
  - Public spaces

**Notes:**
[Add your study notes here as you learn]

---

### 2.3 Explain various types of vulnerabilities

#### Video 1: Operating System Vulnerabilities
- [ ] **Watched Professor Messer's "Operating System Vulnerabilities" video**

##### OS Vulnerability Types
- [ ] 🔄 **Unpatched systems**
  - Missing security updates
  - Known vulnerabilities
  - Exploit availability

- [ ] ⚙️ **Default settings**
  - Unchanged configurations
  - Open permissions
  - Unnecessary services

- [ ] ⬆️ **Privilege escalation**
  - Gaining higher access levels
  - Exploiting OS flaws
  - Root/admin access

#### Video 2: Zero-day Vulnerabilities
- [ ] **Watched Professor Messer's "Zero-day Vulnerabilities" video**

##### Zero-day Concepts
- [ ] 🎯 **Definition**
  - Unknown to vendor
  - No patch available
  - High-value targets

- [ ] ⚡ **Characteristics**
  - Actively exploited
  - Limited detection
  - Valuable to attackers

- [ ] 🛡️ **Response strategies**
  - Virtual patching
  - IPS signatures
  - Workarounds and mitigations

#### Video 3: Hardware Vulnerabilities
- [ ] **Watched Professor Messer's "Hardware Vulnerabilities" video**

##### Hardware Issues
- [ ] 💿 **Firmware vulnerabilities**
  - BIOS/UEFI exploits
  - Persistent malware
  - Difficult to detect and remove

- [ ] ⏳ **End-of-life (EOL)**
  - No more security updates
  - Legacy systems
  - Unsupported hardware

- [ ] 🏛️ **Legacy platforms**
  - Outdated technology
  - Compatibility issues
  - Security gaps

#### Video 4: Cloud-specific Vulnerabilities
- [ ] **Watched Professor Messer's "Cloud-specific Vulnerabilities" video**

##### Cloud Vulnerabilities
- [ ] ☁️ **Misconfigurations**
  - Open storage buckets
  - Incorrect permissions
  - Exposed databases

- [ ] 🔌 **Insecure APIs**
  - Authentication bypass
  - Injection attacks
  - Data exposure

- [ ] 🏢 **Multi-tenancy issues**
  - Shared resources
  - Data isolation
  - Cross-tenant attacks

- [ ] 🔓 **Account hijacking**
  - Stolen credentials
  - Session hijacking
  - Unauthorized access

#### Video 5: Supply Chain Vulnerabilities
- [ ] **Watched Professor Messer's "Supply Chain Vulnerabilities" video**

##### Supply Chain Risks
- [ ] 🔗 **Third-party risks**
  - Vendor compromises
  - Software dependencies
  - Trusted relationships

- [ ] 🔧 **Hardware tampering**
  - Modified components
  - Counterfeit parts
  - Backdoors in hardware

- [ ] 📦 **Software supply chain**
  - Compromised updates
  - Malicious libraries
  - Build process attacks

#### Video 6: Mobile Device Vulnerabilities
- [ ] **Watched Professor Messer's "Mobile Device Vulnerabilities" video**

##### Mobile Threats
- [ ] 🍎 **Jailbreaking (iOS)**
  - Removes OS restrictions
  - Security bypass
  - Unofficial apps

- [ ] 🤖 **Rooting (Android)**
  - Gaining root access
  - Custom ROMs
  - Security risks

- [ ] 📲 **Sideloading**
  - Installing apps outside official store
  - Malicious apps
  - Bypass security checks

- [ ] 📱 **Mobile malware**
  - Malicious apps
  - Data theft
  - Spyware

#### Video 7: Misconfiguration Vulnerabilities
- [ ] **Watched Professor Messer's "Misconfiguration Vulnerabilities" video**

##### Configuration Issues
- [ ] 🔓 **Open permissions**
  - Overly permissive access
  - Everyone access
  - Privilege creep

- [ ] 🚪 **Unsecured admin pages**
  - Admin interfaces exposed
  - Weak authentication
  - Default credentials

- [ ] 🔐 **Weak encryption**
  - Outdated algorithms
  - Short key lengths
  - Insecure protocols

- [ ] ⚠️ **Error messages**
  - Detailed error information
  - Information disclosure
  - System details leaked

#### Video 8: Cross-site Scripting (XSS)
- [ ] **Watched Professor Messer's "Cross-site Scripting" video**

##### XSS Types
- [ ] ↩️ **Reflected XSS**
  - Non-persistent
  - Immediate response
  - URL-based attacks

- [ ] 💾 **Stored XSS**
  - Persistent
  - Stored in database
  - Affects all users

- [ ] 🌐 **DOM-based XSS**
  - Client-side vulnerability
  - JavaScript execution
  - Browser-based

##### XSS Impact
- [ ] 🍪 **Session hijacking**
  - Stealing cookies
  - Account takeover
  - Credential theft

- [ ] 🎨 **Defacement**
  - Altering webpage content
  - Malicious redirects

#### Video 9: SQL Injection
- [ ] **Watched Professor Messer's "SQL Injection" video**

##### SQL Injection Concepts
- [ ] 💉 **Attack mechanism**
  - Injecting SQL commands
  - Manipulating database queries
  - Input validation bypass

- [ ] 🛠️ **Common techniques**
  - 1=1 (always true)
  - OR statements
  - UNION queries
  - Comment injection (-- or /*)

- [ ] 💥 **Impact**
  - Data extraction
  - Database modification
  - Authentication bypass
  - Complete database compromise

- [ ] 🛡️ **Prevention**
  - Prepared statements
  - Parameterized queries
  - Input validation
  - Least privilege database accounts

#### Video 10: Malicious Updates
- [ ] **Watched Professor Messer's "Malicious Updates" video**

##### Update Vulnerabilities
- [ ] 🖥️ **Compromised update servers**
  - Man-in-the-middle attacks
  - Fake updates
  - Malware distribution

- [ ] 🔗 **Supply chain attacks via updates**
  - Trusted update mechanisms
  - Automatic updates exploited
  - Wide distribution

#### Video 11: Race Conditions
- [ ] **Watched Professor Messer's "Race Conditions" video**

##### Race Condition Concepts
- [ ] ⏱️ **Time-of-check to time-of-use (TOCTOU)**
  - Timing vulnerabilities
  - State changes between checks
  - Concurrent access issues

- [ ] ⚡ **Exploitation**
  - Multiple simultaneous transactions
  - Resource manipulation
  - Permission bypass

#### Video 12: Buffer Overflows
- [ ] **Watched Professor Messer's "Buffer Overflows" video**

##### Buffer Overflow Concepts
- [ ] 📊 **How buffer overflows work**
  - Writing beyond memory boundaries
  - Overwriting adjacent memory
  - Stack and heap overflows

- [ ] 💣 **Exploitation**
  - Code execution
  - Return address modification
  - Shell code injection

- [ ] 🛡️ **Prevention**
  - Input validation
  - Bounds checking
  - Address Space Layout Randomization (ASLR)
  - Data Execution Prevention (DEP)

#### Video 13: Memory Injections
- [ ] **Watched Professor Messer's "Memory Injections" video**

##### Injection Types
- [ ] 📚 **DLL injection**
  - Loading malicious libraries
  - Process hijacking
  - Stealth techniques

- [ ] ⚙️ **Process injection**
  - Code injection into running processes
  - Hiding malware
  - Evasion techniques

#### Video 14: Cryptographic Vulnerabilities
- [ ] **Watched Professor Messer's "Cryptographic Vulnerabilities" video**

##### Crypto Weaknesses
- [ ] 🔓 **Weak algorithms**
  - Deprecated encryption (DES, RC4)
  - MD5, SHA-1 weaknesses
  - Collision attacks

- [ ] 🔑 **Poor key management**
  - Hardcoded keys
  - Weak key generation
  - Key reuse

- [ ] 🐛 **Implementation flaws**
  - Side-channel attacks
  - Timing attacks
  - Padding oracle attacks

#### Video 15: Virtualization Vulnerabilities
- [ ] **Watched Professor Messer's "Virtualization Vulnerabilities" video**

##### Virtualization Risks
- [ ] 🏃 **VM escape**
  - Breaking out of VM
  - Accessing hypervisor
  - Compromising host

- [ ] 💻 **Hypervisor vulnerabilities**
  - Host OS attacks
  - Management interface exploits

- [ ] 📈 **VM sprawl**
  - Unmanaged VMs
  - Forgotten instances
  - Security gaps

**Notes:**
[Add your study notes here as you learn]

---

### 2.4 Given a scenario, analyze indicators of malicious activity

#### Video 1: An Overview of Malware
- [ ] **Watched Professor Messer's "An Overview of Malware" video**

##### Malware Fundamentals
- [ ] 🦠 **Definition and purpose**
  - Malicious software
  - Unauthorized access and damage
  - Various delivery methods

- [ ] 💀 **Common malware types**
  - Ransomware
  - Trojans
  - Worms and viruses
  - Rootkits
  - Keyloggers

#### Video 2: Viruses and Worms
- [ ] **Watched Professor Messer's "Viruses and Worms" video**

##### Viruses
- [ ] 💻 **Program viruses**
  - Attach to executables
  - Requires user action
  - Spreads through file sharing

- [ ] 🥾 **Boot sector viruses**
  - Infect boot records
  - Run before OS loads
  - Difficult to remove

- [ ] 📜 **Script viruses**
  - OS and browser scripts
  - Macro viruses in documents
  - Email-based spread

- [ ] 👻 **Fileless viruses**
  - Operate in memory (RAM)
  - No files on disk
  - Difficult to detect

##### Worms
- [ ] 🐛 **Self-replicating**
  - Spreads automatically
  - No user interaction needed
  - Network propagation

- [ ] ⚡ **Famous examples**
  - WannaCry
  - Code Red
  - Stuxnet

#### Video 3: Other Malware Types
- [ ] **Watched Professor Messer's "Other Malware Types" video**

##### Additional Malware
- [ ] ⌨️ **Keyloggers**
  - Capture keystrokes
  - Steal credentials
  - Hardware or software-based

- [ ] 💣 **Logic bombs**
  - Triggered by conditions
  - Time-based or event-based
  - Insider threats

- [ ] 🌳 **Rootkits**
  - Hide other malware
  - Kernel-level access
  - Very difficult to detect

#### Video 4: Spyware and Bloatware
- [ ] **Watched Professor Messer's "Spyware and Bloatware" video**

##### Unwanted Software
- [ ] 🕵️ **Spyware**
  - Monitors user activity
  - Collects personal information
  - Advertisements and tracking

- [ ] 📺 **Adware**
  - Displays advertisements
  - Pop-ups and banners
  - Revenue generation

- [ ] 📦 **Bloatware**
  - Pre-installed software
  - Unnecessary applications
  - Performance impact

#### Video 5: Malicious Code
- [ ] **Watched Professor Messer's "Malicious Code" video**

##### Code-based Attacks
- [ ] 🐴 **Trojans**
  - Appears legitimate
  - Hidden malicious functionality
  - Remote access trojans (RATs)

- [ ] 🔐 **Ransomware**
  - Encrypts data
  - Demands payment
  - Crypto-ransomware vs. locker ransomware

- [ ] ⚠️ **Potentially unwanted programs (PUPs)**
  - Questionable software
  - Bundled applications
  - May not be explicitly malicious

#### Video 6: Password Attacks
- [ ] **Watched Professor Messer's "Password Attacks" video**

##### Attack Methods
- [ ] 🔨 **Brute force**
  - Try all combinations
  - Time-consuming
  - Success depends on complexity

- [ ] 📖 **Dictionary attacks**
  - Common words and phrases
  - Password lists
  - Faster than brute force

- [ ] 💦 **Password spraying**
  - Common passwords
  - Multiple accounts
  - Avoid account lockouts

- [ ] 🔄 **Credential stuffing**
  - Reused passwords
  - Breached credentials
  - Automated attacks

- [ ] 🌈 **Rainbow tables**
  - Pre-computed hashes
  - Fast password cracking
  - Defeated by salting

- [ ] #️⃣ **Pass the hash**
  - Using password hash
  - No need for plaintext
  - Lateral movement

#### Video 7: Application Attacks
- [ ] **Watched Professor Messer's "Application Attacks" video**

##### Web Application Attacks
- [ ] 💉 **Injection attacks**
  - SQL injection
  - LDAP injection
  - XML injection

- [ ] ⬆️ **Privilege escalation**
  - Vertical: gaining admin rights
  - Horizontal: accessing other user data
  - Exploiting vulnerabilities

- [ ] 📂 **Directory traversal**
  - Path traversal attacks
  - Accessing files outside web root
  - ../ attacks

- [ ] 🔀 **Cross-site request forgery (CSRF)**
  - Unauthorized commands
  - Exploiting trust
  - Using authenticated sessions

- [ ] 🖥️ **Server-side request forgery (SSRF)**
  - Force server to make requests
  - Internal network access
  - Cloud metadata exploitation

- [ ] 💻 **Remote code execution (RCE)**
  - Execute arbitrary code
  - Complete system compromise
  - Critical vulnerability

#### Video 8: Denial of Service
- [ ] **Watched Professor Messer's "Denial of Service" video**

##### DoS Attack Types
- [ ] 🚫 **Denial of Service (DoS)**
  - Single source attack
  - Resource exhaustion
  - Service unavailability

- [ ] 💥 **Distributed Denial of Service (DDoS)**
  - Multiple sources
  - Botnets
  - Difficult to block

- [ ] 📢 **Amplification attacks**
  - Small request, large response
  - DNS amplification
  - NTP amplification

- [ ] 🔄 **Reflected attacks**
  - Spoofed source address
  - Bouncing off third party
  - Hiding attacker

#### Video 9: DNS Attacks
- [ ] **Watched Professor Messer's "DNS Attacks" video**

##### DNS Attack Types
- [ ] ☠️ **DNS poisoning**
  - Cache poisoning
  - Corrupted DNS records
  - Redirecting users

- [ ] 🏴‍☠️ **Domain hijacking**
  - Unauthorized domain transfers
  - Registrar compromise
  - Lost control of domain

- [ ] ⌨️ **URL hijacking (typosquatting)**
  - Similar domain names
  - Common typos
  - Homograph attacks

- [ ] 🚇 **DNS tunneling**
  - Data exfiltration
  - Command and control
  - Bypass firewalls

#### Video 10: Replay Attacks
- [ ] **Watched Professor Messer's "Replay Attacks" video**

##### Replay Attack Concepts
- [ ] 🔁 **Session replay**
  - Capturing valid sessions
  - Reusing authentication
  - Session hijacking

- [ ] #️⃣ **Pass the hash**
  - Replaying password hashes
  - No need to crack
  - Windows authentication

- [ ] 🎫 **Kerberos attacks**
  - Golden ticket
  - Silver ticket
  - Pass the ticket

- [ ] 🛡️ **Prevention**
  - Timestamps
  - Nonces (number used once)
  - Sequence numbers
  - Short session timeouts

#### Video 11: On-path Attacks
- [ ] **Watched Professor Messer's "On-path Attacks" video**

##### Man-in-the-Middle Concepts
- [ ] 👤 **Interception**
  - Positioning between parties
  - Eavesdropping on communication
  - Real-time manipulation

- [ ] 🎯 **Attack techniques**
  - ARP spoofing
  - DNS spoofing
  - Rogue access points

- [ ] 🔓 **SSL stripping**
  - Downgrading HTTPS to HTTP
  - Removing encryption
  - Transparent proxy

- [ ] 🛡️ **Prevention**
  - Encryption (TLS/SSL)
  - Certificate pinning
  - HSTS (HTTP Strict Transport Security)

#### Video 12: Cryptographic Attacks
- [ ] **Watched Professor Messer's "Cryptographic Attacks" video**

##### Crypto Attack Types
- [ ] 🎂 **Birthday attacks**
  - Hash collision exploitation
  - Probability-based
  - Weaken hash functions

- [ ] 💥 **Collision attacks**
  - Two inputs, same output
  - Breaking hash integrity
  - Certificate forgery

- [ ] ⬇️ **Downgrade attacks**
  - Force weaker encryption
  - Protocol negotiation exploits
  - POODLE, BEAST attacks

#### Video 13: Wireless Attacks
- [ ] **Watched Professor Messer's "Wireless Attacks" video**

##### Wireless Attack Types
- [ ] 👿 **Evil twin**
  - Rogue access point
  - Identical SSID
  - Man-in-the-middle

- [ ] 🚫 **Deauthentication attacks**
  - Disconnect wireless clients
  - Force reconnection
  - Capture handshakes

- [ ] 📡 **RF jamming**
  - Radio frequency interference
  - Denial of service
  - Signal blocking

- [ ] 📌 **WPS attacks**
  - Brute force PIN
  - Easy access
  - Weak authentication

- [ ] 🔓 **WEP/WPA attacks**
  - Cracking weak encryption
  - Capturing handshakes
  - Dictionary attacks

- [ ] 🔵 **Bluetooth attacks**
  - Bluejacking
  - Bluesnarfing
  - Bluesmack

#### Video 14: Physical Attacks
- [ ] **Watched Professor Messer's "Physical Attacks" video**

##### Physical Attack Types
- [ ] 🔨 **Brute force physical**
  - Forcing entry
  - Breaking locks
  - Destructive access

- [ ] 📡 **RFID cloning**
  - Copying access badges
  - Skimming cards
  - Unauthorized duplication

- [ ] 🌡️ **Environmental attacks**
  - Temperature manipulation
  - Humidity control
  - Power issues

- [ ] 🔌 **Hardware attacks**
  - Malicious USB cables
  - Card skimmers
  - Flash drives

#### Video 15: Indicators of Compromise
- [ ] **Watched Professor Messer's "Indicators of Compromise" video**

##### Network Indicators
- [ ] 🌐 **Unusual network traffic**
  - Unexpected protocols
  - Large data transfers
  - Odd connection patterns

- [ ] 🤖 **Rogue devices**
  - Unauthorized hardware
  - Unknown MAC addresses
  - Suspicious access points

- [ ] 📊 **Bandwidth consumption**
  - Unexpected spikes
  - Data exfiltration
  - DDoS participation

##### Host Indicators
- [ ] 🔒 **Account lockouts**
  - Multiple failed logins
  - Brute force attempts
  - Compromised accounts

- [ ] ✈️ **Impossible travel**
  - Logins from different locations
  - Geographically impossible
  - Account sharing or compromise

- [ ] 💻 **Resource consumption**
  - High CPU usage
  - Memory exhaustion
  - Disk activity

- [ ] ⚠️ **Unauthorized changes**
  - Modified files
  - New user accounts
  - Configuration changes

##### Application Indicators
- [ ] 🔍 **Anomalous activity**
  - Unusual behavior
  - Off-hours access
  - Privilege escalation attempts

- [ ] 📁 **File system changes**
  - New files
  - Modified binaries
  - Missing logs

**Notes:**
[Add your study notes here as you learn]

---

### 2.5 Explain the purpose of mitigation techniques used to secure the enterprise

#### Video 1: Mitigation Techniques
- [ ] **Watched Professor Messer's "Mitigation Techniques" video**

##### Key Mitigation Strategies
- [ ] 🔄 **Patching and updates**
  - Operating systems
  - Applications
  - Firmware

- [ ] 🔐 **Encryption**
  - Data at rest
  - Data in transit
  - End-to-end encryption

- [ ] 📊 **Monitoring and logging**
  - SIEM systems
  - Log analysis
  - Real-time alerts

- [ ] 🔒 **Least privilege**
  - Minimum necessary access
  - Just-in-time access
  - Regular review

- [ ] ⚙️ **Configuration management**
  - Baseline configurations
  - Change control
  - Automated deployment

- [ ] 🗑️ **Decommissioning**
  - Proper asset disposal
  - Data sanitization
  - Certificate revocation

#### Video 2: Segmentation and Access Control
- [ ] **Watched Professor Messer's "Segmentation and Access Control" video**

##### Network Segmentation
- [ ] 🌐 **VLAN segmentation**
  - Logical separation
  - Broadcast domain isolation
  - Security boundaries

- [ ] 🔌 **Physical segmentation**
  - Air gaps
  - Separate networks
  - Isolated systems

- [ ] 🔬 **Microsegmentation**
  - Granular controls
  - Zero Trust implementation
  - Workload isolation

##### Access Control Methods
- [ ] 📋 **Access Control Lists (ACLs)**
  - Network ACLs
  - File system permissions
  - Router and firewall rules

- [ ] ✅ **Application allow lists**
  - Whitelisting applications
  - Executable control
  - Preventing unauthorized software

- [ ] 🚫 **Application deny lists**
  - Blacklisting applications
  - Known malware
  - Reactive approach

#### Video 3: Hardening Techniques
- [ ] **Watched Professor Messer's "Hardening Techniques" video**

##### System Hardening
- [ ] 🚫 **Disable unnecessary services**
  - Reduce attack surface
  - Close unused ports
  - Remove unnecessary software

- [ ] 🔑 **Change default passwords**
  - Unique credentials
  - Strong passwords
  - Regular rotation

- [ ] 👤 **Disable default accounts**
  - Remove guest accounts
  - Rename admin accounts
  - Disable built-in accounts

- [ ] 🔐 **Encryption**
  - Full-disk encryption
  - File-level encryption
  - Network encryption

##### Configuration Hardening
- [ ] 📏 **Security baselines**
  - CIS benchmarks
  - STIG guidelines
  - Vendor best practices

- [ ] 👥 **Group policies**
  - Centralized management
  - Consistent configurations
  - Windows domain environments

- [ ] 🔒 **Secure protocols**
  - Disable legacy protocols
  - Use SSH instead of Telnet
  - HTTPS instead of HTTP
  - SNMPv3 instead of v1/v2

- [ ] 🔍 **Regular audits**
  - Vulnerability scanning
  - Penetration testing
  - Configuration reviews

**Notes:**
[Add your study notes here as you learn]

---

## Key Terms & Definitions

**Term** | **Definition**
---------|---------------
**APT** | Advanced Persistent Threat - prolonged targeted attack
**C2/C&C** | Command and Control server used by attackers
**IOC** | Indicator of Compromise - evidence of security breach
**OSINT** | Open Source Intelligence - publicly available information
**TTP** | Tactics, Techniques, and Procedures used by threat actors
**Zero-day** | Vulnerability unknown to vendor with no patch available
**RAT** | Remote Access Trojan - malware allowing remote control
**XSS** | Cross-Site Scripting - web application vulnerability
**SQLi** | SQL Injection - database attack via input manipulation
**CSRF** | Cross-Site Request Forgery - unauthorized command exploit
**SSRF** | Server-Side Request Forgery - force server to make requests
**DoS/DDoS** | Denial of Service / Distributed Denial of Service
**MitM** | Man-in-the-Middle attack (also called On-path attack)
**TOCTOU** | Time-of-Check to Time-of-Use race condition
**ASLR** | Address Space Layout Randomization - memory protection
**DEP** | Data Execution Prevention - prevents code execution in data memory

---

## Acronyms to Know

- **APT** - Advanced Persistent Threat
- **BEC** - Business Email Compromise
- **C2/C&C** - Command and Control
- **CVE** - Common Vulnerabilities and Exposures
- **DDoS** - Distributed Denial of Service
- **EOL** - End of Life
- **IOC** - Indicator of Compromise
- **ISAC** - Information Sharing and Analysis Center
- **NVD** - National Vulnerability Database
- **OSINT** - Open Source Intelligence
- **PUP** - Potentially Unwanted Program
- **RAT** - Remote Access Trojan
- **RCE** - Remote Code Execution
- **SIEM** - Security Information and Event Management
- **TTP** - Tactics, Techniques, and Procedures
- **XSS** - Cross-Site Scripting

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

### Identity-Based Threats
- Credential theft and password attacks
- Account takeover and session hijacking
- Insider threats with authorized access
- Privilege escalation attacks

### IAM Mitigation Strategies
- Multi-factor authentication (MFA)
- Least privilege access
- Just-in-time (JIT) access
- Identity governance and administration
- Privileged access management (PAM)
- Behavioral analytics and monitoring

---

## Exam Tips

- **Know threat actor motivations** - Understand what drives different attackers
- **Recognize social engineering** - Phishing is heavily tested
- **Understand vulnerability types** - Be able to classify different vulnerabilities
- **Memorize common attack types** - XSS, SQLi, DoS, MitM, etc.
- **Know IoC examples** - Be able to identify signs of compromise
- **Understand mitigation techniques** - Match vulnerabilities to appropriate mitigations
- **This is 22% of the exam** - Spend adequate study time here!

---

## Related Labs

- [Link to relevant hands-on labs]

---

**Study Status:** Not Started
**Confidence Level:** [1-5]
**Last Reviewed:** [Date]

---

**Note:** This domain is 22% of the exam - spend adequate time here! It's the largest domain in the Security+ certification.
