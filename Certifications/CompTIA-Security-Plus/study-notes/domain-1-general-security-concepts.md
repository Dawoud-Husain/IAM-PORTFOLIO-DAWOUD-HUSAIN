# Domain 1: General Security Concepts (12%)

## Domain Weight: 12% of exam

---

## Professor Messer's Video Structure

This study guide follows Professor Messer's CompTIA SY0-701 Security+ Training Course structure.
**Total Videos in Domain 1: 18 videos**

### Section 1.1 - Compare and contrast various types of security controls (1 video)
- [ ] Security Controls

### Section 1.2 - Summarize fundamental security concepts (7 videos)
- [ ] The CIA Triad
- [ ] Non-repudiation
- [ ] Authentication, Authorization, and Accounting
- [ ] Gap Analysis
- [ ] Zero Trust
- [ ] Physical Security
- [ ] Deception and Disruption

### Section 1.3 - Explain the importance of change management processes (2 videos)
- [ ] Change Management
- [ ] Technical Change Management

### Section 1.4 - Explain the importance of using appropriate cryptographic solutions (8 videos)
- [ ] Public Key Infrastructure
- [ ] Encryption Technologies
- [ ] Encrypting Data
- [ ] Hashing and Digital Signatures
- [ ] Key Exchange
- [ ] Obfuscation
- [ ] Blockchain Technology
- [ ] Certificates

---

## Topics Covered

### 1.1 Compare and contrast various types of security controls

#### Video: Security Controls
- [ ] **Watched Professor Messer's "Security Controls" video**

#### Security Control Categories
- [ ] 💻 **Technical controls**
  - Controls implemented using systems
  - Operating system controls
  - Firewalls, anti-virus

- [ ] 📋 **Managerial controls**
  - Administrative controls
  - Security policies, standard operating procedures

- [ ] 👥 **Operational controls**
  - Controls implemented by people instead of systems
  - Security guards, awareness programs

- [ ] 🔒 **Physical controls**
  - Limit physical access
  - Guard shack, fences, locks, badge readers

#### Security Control Types
- [ ] 🛡️ **Preventive**
  - Block access to a resource - "you shall not pass"
  - Technical: Firewall rules
  - Managerial: Follow security policy
  - Operational: Guard shack checks identification
  - Physical: Door locks

- [ ] ⚠️ **Deterrent**
  - Discourage an intrusion attempt - does not directly prevent access
  - Technical: Application splash screen
  - Managerial: Demotion
  - Operational: Reception desk
  - Physical: Warning sign

- [ ] 🔍 **Detective**
  - Identify and log an intrusion attempt - may not prevent access
  - Technical: System logs
  - Managerial: Review login reports
  - Operational: Property patrols
  - Physical: Motion detectors

- [ ] 🔧 **Corrective**
  - Apply a control after event has been detected - reverse impact - continue operating with minimal downtime
  - Technical: Backup recovery
  - Managerial: Policies for reporting issues
  - Operational: Contact authorities
  - Physical: Fire extinguisher

- [ ] 🔄 **Compensating**
  - Control using other means - existing controls aren't sufficient, may be temporary
  - Technical: Block instead of patch
  - Managerial: Separation of duties
  - Operational: Require multiple security staff
  - Physical: Power generator

- [ ] 📢 **Directive**
  - Direct a subject towards security compliance - weak security control
  - Technical: File storage policies
  - Managerial: Compliance policies
  - Operational: Security policy training
  - Physical: Sign: "Authorized personnel only"

**Notes:**
[Add your study notes here as you learn]

---

### 1.2 Summarize fundamental security concepts

#### Video 1: The CIA Triad
- [ ] **Watched Professor Messer's "The CIA Triad" video**

##### CIA Triad Components
- [ ] 🔐 **Confidentiality**
  - Prevent disclosure of information to unauthorized individuals or systems
  - Encryption, access controls, two-factor authentication

- [ ] ✅ **Integrity**
  - Messages cannot be modified without detection
  - Hashing, digital signatures, certificates, non-repudiation

- [ ] ⚡ **Availability**
  - Systems and networks must be up and running
  - Redundancy, fault tolerance, patching

#### Video 2: Non-repudiation
- [ ] **Watched Professor Messer's "Non-repudiation" video**
- [ ] ✍️ **Non-repudiation concepts**
  - Cannot deny what you've said - like a contract
  - Proof of integrity and origin
  - Digital signatures provide non-repudiation

#### Video 3: Authentication, Authorization, and Accounting (AAA)
- [ ] **Watched Professor Messer's "AAA" video**
- [ ] 🔑 **Authentication**
  - Proving who you are
  - Username/password, biometrics, MFA

- [ ] 🚪 **Authorization**
  - What resources you can access
  - Permissions and access control lists

- [ ] 📊 **Accounting**
  - Tracking what users do
  - Logging, auditing, monitoring

#### Video 4: Gap Analysis
- [ ] **Watched Professor Messer's "Gap Analysis" video**
- [ ] 📏 **Gap analysis process**
  - Compare current state vs. desired state
  - Identify security gaps
  - Create remediation plan
  - Baseline configuration

#### Video 5: Zero Trust
- [ ] **Watched Professor Messer's "Zero Trust" video**
- [ ] 🎯 **Zero Trust principles**
  - Never trust, always verify
  - Assume breach mentality
  - Least privilege access
  - Micro-segmentation
  - Multi-factor authentication everywhere

#### Video 6: Physical Security
- [ ] **Watched Professor Messer's "Physical Security" video**
- [ ] 🏢 **Physical security controls**
  - Fences, gates, bollards
  - Security guards and reception
  - Badge readers and biometrics
  - Locks and access control vestibules
  - Video surveillance
  - Sensors and alarms

#### Video 7: Deception and Disruption
- [ ] **Watched Professor Messer's "Deception and Disruption" video**
- [ ] 🪤 **Deception technologies**
  - Honeypots - decoy systems
  - Honeynets - network of honeypots
  - Honeyfiles - fake files
  - Honeytokens - fake data
- [ ] 💥 **Disruption techniques**
  - Confuse and delay attackers
  - Gather intelligence on attack methods

**Notes:**
[Add your study notes here as you learn]

---

### 1.3 Explain the importance of change management processes

#### Video 1: Change Management
- [ ] **Watched Professor Messer's "Change Management" video**

##### Change Management Process
- [ ] ✓ **Change approval process**
  - Request for change (RFC)
  - Change management board/committee reviews
  - Documented approval process

- [ ] 📈 **Impact analysis**
  - Business processes impacting security
  - Risk assessment
  - Scope of changes

- [ ] 📝 **Change documentation**
  - Detailed change plans
  - Rollback procedures
  - Version control

- [ ] 🧪 **Testing and validation**
  - Test in non-production environment
  - Validate changes before deployment
  - User acceptance testing (UAT)

- [ ] ⚙️ **Change implementation**
  - Schedule maintenance windows
  - Execute change plan
  - Monitor for issues

#### Video 2: Technical Change Management
- [ ] **Watched Professor Messer's "Technical Change Management" video**

##### Technical Change Components
- [ ] 📋 **Allow lists/deny lists**
  - Restrict or allow changes
  - Application whitelisting
  - Access control lists

- [ ] ⏱️ **Downtime considerations**
  - Plan for service interruptions
  - Communicate maintenance windows
  - Minimize impact on operations

- [ ] 🔄 **Restarts and reboots**
  - System restart requirements
  - Service dependencies
  - Proper restart procedures

- [ ] 🏛️ **Legacy applications**
  - Compatibility concerns
  - Testing with older systems
  - Migration planning

- [ ] 🔗 **Dependencies**
  - Identify system interdependencies
  - Test dependent systems
  - Document relationships

- [ ] 📄 **Documentation**
  - Version control systems
  - Change logs
  - Configuration management database (CMDB)

- [ ] 📖 **Standard operating procedures (SOPs)**
  - Documented processes
  - Consistent implementation
  - Training materials

**Notes:**
[Add your study notes here as you learn]

---

### 1.4 Explain the importance of using appropriate cryptographic solutions

#### Video 1: Public Key Infrastructure
- [ ] **Watched Professor Messer's "Public Key Infrastructure" video**

##### PKI Components
- [ ] 🔐 **Public Key Infrastructure (PKI)**
  - Framework for managing digital certificates
  - Certificate authorities (CA)
  - Registration authorities (RA)
  - Certificate revocation lists (CRL)
  - Online Certificate Status Protocol (OCSP)

- [ ] 🤝 **Trust models**
  - Single CA
  - Hierarchical CA
  - Mesh/distributed trust

- [ ] ♻️ **Certificate lifecycle**
  - Creation and issuance
  - Validation and renewal
  - Revocation and expiration

#### Video 2: Encryption Technologies
- [ ] **Watched Professor Messer's "Encryption Technologies" video**

##### Encryption Types
- [ ] 🔑 **Symmetric encryption**
  - Same key for encryption and decryption
  - Fast and efficient
  - Key distribution challenge
  - Algorithms: AES, DES, 3DES, RC4, Blowfish

- [ ] 🗝️ **Asymmetric encryption**
  - Public and private key pair
  - Public key encrypts, private key decrypts
  - Slower than symmetric
  - Algorithms: RSA, ECC (Elliptic Curve Cryptography), DSA

- [ ] 📏 **Key length**
  - Longer keys = stronger encryption
  - Balance between security and performance

#### Video 3: Encrypting Data
- [ ] **Watched Professor Messer's "Encrypting Data" video**

##### Encryption Levels
- [ ] 💾 **Full-disk encryption (FDE)**
  - Encrypt entire drive
  - BitLocker, FileVault, LUKS
  - Protects data at rest

- [ ] 🗂️ **Partition encryption**
  - Encrypt specific partitions
  - Selective protection

- [ ] 📁 **File encryption**
  - Encrypt individual files
  - EFS (Encrypting File System)
  - GPG/PGP

- [ ] 📦 **Volume encryption**
  - Encrypt volumes/containers
  - VeraCrypt, TrueCrypt

- [ ] 🗄️ **Database encryption**
  - Encrypt database fields
  - Transparent Data Encryption (TDE)

- [ ] 📝 **Record-level encryption**
  - Encrypt specific records
  - Granular control

- [ ] 🌐 **Transport/communication encryption**
  - TLS/SSL for network traffic
  - VPNs (IPsec, SSL/TLS VPN)
  - SSH for remote access

#### Video 4: Hashing and Digital Signatures
- [ ] **Watched Professor Messer's "Hashing and Digital Signatures" video**

##### Hashing Concepts
- [ ] #️⃣ **Hash functions**
  - One-way function
  - Fixed-length output
  - Verify data integrity
  - Algorithms: MD5 (weak), SHA-1 (deprecated), SHA-2 (SHA-256, SHA-512), SHA-3

- [ ] 💥 **Hash collision**
  - Two inputs produce same hash
  - Security weakness

- [ ] 🧂 **Salting**
  - Add random data to input
  - Prevents rainbow table attacks
  - Unique salt per password

- [ ] 🏋️ **Key stretching**
  - PBKDF2 (Password-Based Key Derivation Function 2)
  - Bcrypt
  - Makes brute force attacks slower

- [ ] 🔏 **HMAC**
  - Hash-based Message Authentication Code
  - Verify integrity and authenticity
  - Uses secret key with hash

##### Digital Signatures
- [ ] ✍️ **Digital signature process**
  - Hash document
  - Encrypt hash with private key
  - Verify with public key
  - Provides: authenticity, integrity, non-repudiation

- [ ] 🔐 **Digital Signature Algorithm (DSA)**
- [ ] 🔏 **RSA for digital signatures**

#### Video 5: Key Exchange
- [ ] **Watched Professor Messer's "Key Exchange" video**

##### Key Exchange Methods
- [ ] 📞 **Out-of-band key exchange**
  - Separate communication channel
  - Phone, in-person, courier

- [ ] 🔗 **In-band key exchange**
  - Same communication channel
  - Diffie-Hellman

- [ ] 🤝 **Diffie-Hellman (DH)**
  - Key agreement protocol
  - Establish shared secret
  - DHE (Ephemeral), ECDHE (Elliptic Curve)

- [ ] 🛡️ **Perfect Forward Secrecy (PFS)**
  - Session keys not compromised if long-term key compromised
  - Uses ephemeral keys

- [ ] 🏦 **Key escrow**
  - Third party holds decryption keys
  - Recovery option
  - Privacy concerns

#### Video 6: Obfuscation
- [ ] **Watched Professor Messer's "Obfuscation" video**

##### Obfuscation Techniques
- [ ] 🌫️ **Code obfuscation**
  - Make code difficult to understand
  - Protect intellectual property
  - Security through obscurity (not recommended as sole security)

- [ ] 🖼️ **Steganography**
  - Hide data within other data
  - Images, audio, video files
  - Not encryption, just hiding

- [ ] 🎫 **Tokenization**
  - Replace sensitive data with token
  - Token has no meaning outside system
  - Common in payment processing

- [ ] 🎭 **Data masking**
  - Hide original data
  - Show only necessary parts (e.g., last 4 digits of SSN)
  - Protects PII

#### Video 7: Blockchain Technology
- [ ] **Watched Professor Messer's "Blockchain Technology" video**

##### Blockchain Concepts
- [ ] 📒 **Distributed ledger**
  - Decentralized database
  - Multiple copies across network
  - No single point of control

- [ ] ⛓️ **Blockchain characteristics**
  - Immutable records
  - Transparent and verifiable
  - Consensus mechanisms

- [ ] 🧱 **Blocks**
  - Container for data
  - Hash of previous block
  - Timestamp
  - Creates chain

- [ ] 📜 **Smart contracts**
  - Self-executing contracts
  - Code-based agreements
  - Automated enforcement

- [ ] 💎 **Blockchain applications**
  - Cryptocurrencies (Bitcoin, Ethereum)
  - Supply chain tracking
  - Digital identity
  - Voting systems

#### Video 8: Certificates
- [ ] **Watched Professor Messer's "Certificates" video**

##### Certificate Types and Uses
- [ ] 📜 **Digital certificates**
  - X.509 standard
  - Bind public key to identity
  - Issued by Certificate Authority

- [ ] 📄 **Certificate contents**
  - Subject name
  - Public key
  - Issuer information
  - Validity period
  - Serial number
  - Digital signature

- [ ] 🏷️ **Certificate types**
  - Domain Validation (DV)
  - Organization Validation (OV)
  - Extended Validation (EV)
  - Wildcard certificates
  - Subject Alternative Name (SAN)

- [ ] 📋 **Certificate formats**
  - PEM (Privacy Enhanced Mail)
  - DER (Distinguished Encoding Rules)
  - PFX/P12 (PKCS#12)
  - CER, CRT

- [ ] 📌 **Certificate pinning**
  - Associate certificate with host
  - Prevent man-in-the-middle attacks
  - Mobile apps

- [ ] 🔗 **Certificate chaining**
  - Root CA → Intermediate CA → End certificate
  - Chain of trust

**Notes:**
[Add your study notes here as you learn]

---

## Key Terms & Definitions

**Term** | **Definition**
---------|---------------
[Term] | [Definition]

---

## Acronyms to Know

- **CIA** - Confidentiality, Integrity, Availability
- **AAA** - Authentication, Authorization, Accounting
- **PKI** - Public Key Infrastructure
- [Add more as you study]

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

[How do these concepts relate to Identity and Access Management?]

---

## Exam Tips

- [Tip 1]
- [Tip 2]
- [Tip 3]

---

## Related Labs

- [Link to relevant hands-on labs]

---

**Study Status:** Not Started
**Confidence Level:** [1-5]
**Last Reviewed:** [Date]

---

**Notes:** This is a template. Fill in as you study Domain 1.
