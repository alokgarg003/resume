# Master Prompt: Legacy Enterprise File Transfer — Certificate, Key, Credential & Security Knowledge Model

## Role

Act as a **Senior Enterprise Application Architect, Production Support Architect, Linux Administrator, MFT/SFTP/AS2 Security Specialist, and Technical Documentation Architect**.

Create a formal, enterprise-grade conceptual knowledge document for a **legacy on-premises enterprise file-transfer application** operating on Linux servers.

The environment should be treated as a realistic production platform in which:

- Multiple internal and external users/partners transfer files.
- Linux servers host the file-transfer application and supporting components.
- Different users may have different accounts, directories, permissions, keys, and transfer configurations.
- Multiple file-transfer protocols may coexist.
- Some security artifacts may be shared across the application.
- Other certificates, keys, credentials, and trust relationships may be specific to a partner, connection, user, service account, or file-transfer flow.
- The environment may contain legacy configuration, manually maintained certificates, keystores, truststores, SSH keys, PGP keys, OS-level configuration, and application-level configuration.
- Production support engineers need to understand the relationship between these artifacts when investigating incidents.

The purpose of this document is **conceptual understanding and enterprise knowledge management**, not disclosure of real credentials, private keys, passwords, secrets, production hostnames, or sensitive configuration values.

---

# Primary Objective

Build a detailed **hierarchical security and credential knowledge model** for a legacy enterprise file-transfer platform.

The document must explain the environment from the highest level down to the most specific level:

```text
ENTERPRISE
   │
   └── FILE TRANSFER APPLICATION / PLATFORM
          │
          ├── APPLICATION-LEVEL SECURITY
          │
          ├── SERVER / OS-LEVEL SECURITY
          │
          ├── PROTOCOL-LEVEL SECURITY
          │
          ├── PARTNER / CONNECTION-LEVEL SECURITY
          │
          ├── USER / SERVICE-ACCOUNT LEVEL
          │
          ├── DIRECTORY / FILESYSTEM LEVEL
          │
          ├── TRANSFER FLOW / JOB LEVEL
          │
          └── FILE / TRANSACTION LEVEL
```

Do not treat this merely as a list of certificate file extensions.

Explain the **relationship between the artifacts, their scope, ownership, purpose, dependency, lifecycle, and production impact**.

---

# 1. Start With the Enterprise Application Context

Explain what a legacy enterprise file-transfer platform typically looks like.

Describe:

- Enterprise
- Business applications
- File-transfer platform
- Linux servers
- Network zones
- DMZ/perimeter concepts
- Internal systems
- External partners
- Service accounts
- File-system directories
- Transfer jobs
- Monitoring
- Scheduling
- Logging
- Incident management
- Security controls

Create a conceptual architecture such as:

```text
Internal Application
       │
       ▼
File Transfer Platform
       │
       ├── Linux Server
       │
       ├── MFT Application
       │
       ├── SFTP
       ├── FTPS
       ├── HTTPS
       ├── AS2
       ├── PGP
       └── File System
              │
              ▼
       External Partners
```

Explain the role of each component.

---

# 2. Define Security Artifact Categories

Create a formal taxonomy covering at least:

### Certificates

- X.509 certificates
- TLS/SSL certificates
- Server certificates
- Client certificates
- Mutual TLS certificates
- CA certificates
- Root CA certificates
- Intermediate CA certificates
- Partner certificates
- Signing certificates
- Encryption certificates

### Keys

- Private keys
- Public keys
- SSH host keys
- SSH user keys
- SSH service-account keys
- PGP public keys
- PGP private keys
- Key pairs

### Trust Artifacts

- Truststores
- CA bundles
- Known-host files
- Certificate chains
- Partner trust configuration
- Application trust configuration

### Credential Artifacts

- Linux usernames
- Service accounts
- Application accounts
- SFTP accounts
- API credentials
- Password-based authentication
- SSH-key authentication

### Configuration Artifacts

- Keystores
- Truststores
- Application configuration
- Protocol configuration
- Connection configuration
- Partner profiles
- Transfer jobs
- File-routing configuration

Clearly distinguish:

```text
Certificate
≠
Private Key
≠
Public Key
≠
Credential
≠
Trust Relationship
≠
Configuration
```

---

# 3. Build the Complete Hierarchical Scope Model

Create a detailed hierarchy:

```text
LEVEL 0 — ENTERPRISE
    │
LEVEL 1 — APPLICATION / PLATFORM
    │
LEVEL 2 — SERVER / OPERATING SYSTEM
    │
LEVEL 3 — PROTOCOL
    │
LEVEL 4 — PARTNER / CONNECTION
    │
LEVEL 5 — USER / SERVICE ACCOUNT
    │
LEVEL 6 — DIRECTORY / FILESYSTEM
    │
LEVEL 7 — TRANSFER FLOW / JOB
    │
LEVEL 8 — FILE / TRANSACTION
```

For every level explain:

1. What exists here?
2. Which security artifacts belong here?
3. Who owns them?
4. Who uses them?
5. Are they generic/shared or specific?
6. What depends on them?
7. What happens if they become unavailable?
8. What type of production incident could occur?
9. How would a support engineer identify the affected scope?

---

# 4. Application-Level / Platform-Level Certificates

Explain the artifacts that can potentially affect a large portion of the platform.

Include examples such as:

- Root CA
- Intermediate CA
- Shared CA bundle
- Application TLS certificate
- Application keystore
- Application truststore
- Shared encryption configuration
- Common cryptographic policy
- Common certificate chain

Explain the concept:

```text
Platform
   │
   ├── Shared Truststore
   │      ├── Root CA
   │      ├── Intermediate CA
   │      └── Partner CA
   │
   └── Application Keystore
          ├── Server Certificate
          └── Private Key
```

Explain why these are generally considered **high-blast-radius artifacts**.

---

# 5. Linux Server / Operating-System Level

This section is extremely important.

Explain the security model of a Linux production server hosting a file-transfer application.

Cover:

- Linux users
- Groups
- Service accounts
- UID/GID
- Home directories
- File ownership
- File permissions
- chmod
- chown
- umask
- ACLs
- SSH configuration
- sshd
- authorized_keys
- known_hosts
- host keys
- `/etc/ssh`
- user SSH directories
- application directories
- certificate directories
- keystore locations
- truststore locations
- log directories
- inbound directories
- outbound directories
- archive directories
- temporary directories

Do not assume a single user.

Model an environment such as:

```text
Linux Server
│
├── mft_service
│
├── sftp_user_A
│
├── sftp_user_B
│
├── partner_service_A
│
├── partner_service_B
│
└── admin/support users
```

Explain how multiple users can independently transfer files while sharing the same underlying server/application.

---

# 6. Linux SSH Key Hierarchy

Explain the different SSH key concepts separately.

At minimum cover:

### Server Host Key

```text
Linux SFTP Server
       │
       └── SSH Host Key
```

Purpose:

- Server identity
- Client verification
- Protection against impersonation/MITM

### User Authentication Key

```text
Partner/User
      │
      └── SSH Key Pair
             ├── Private Key
             └── Public Key
```

Explain how the public key may be configured on the server while the private key remains with the client.

### Known Hosts

Explain:

```text
Client
  │
  └── known_hosts
          │
          └── Expected Server Host Key
```

Clearly distinguish:

```text
SSH Host Key
vs
SSH User Authentication Key
vs
known_hosts
```

---

# 7. Protocol-Specific Security Model

Create separate sections for:

## SFTP

Explain:

- SSH
- Host key
- User authentication
- Password authentication
- Public/private key authentication
- Known hosts
- User account
- Directory permissions

Create a conceptual hierarchy:

```text
SFTP
│
├── Server Identity
│      └── Host Key
│
├── User Authentication
│      ├── Username
│      └── SSH Public Key
│
└── File Access
       ├── Linux User
       ├── Group
       └── Directory Permissions
```

## FTPS

Explain:

- TLS
- Server certificate
- Client certificate
- CA trust
- Mutual TLS
- Private key
- Certificate chain

## HTTPS/API

Explain:

- Server TLS certificate
- Client certificate
- mTLS
- CA trust
- API authentication
- TLS termination

## AS2

Explain:

- Signing certificate
- Encryption certificate
- Private key
- Partner public certificate
- Message signing
- Message encryption
- Signature verification
- Decryption
- MDN concepts

## PGP

Explain:

- Public key
- Private key
- Encryption
- Decryption
- Digital signatures
- Key ownership
- Key expiry
- Key rotation
- Partner-specific keys

---

# 8. Partner-Level Security

Create a detailed partner model.

Example:

```text
MFT Platform
│
├── Partner A
│      ├── Connection
│      ├── Protocol
│      ├── Authentication
│      ├── Encryption
│      ├── Signing
│      └── Trust
│
├── Partner B
│
└── Partner C
```

For each partner explain possible artifacts:

- Partner certificate
- Partner CA
- Partner SSH public key
- Partner PGP public key
- Partner-specific trust
- Partner-specific credentials
- Partner-specific connection
- Partner-specific directory
- Partner-specific transfer jobs

Explain why these are usually **specific rather than generic**.

---

# 9. User / Service Account Level

Explain the difference between:

```text
Human User
Service Account
Application Account
SFTP Account
Linux Account
Partner Account
```

Explain how a production environment can contain:

```text
Partner A
   │
   ├── sftp_user_01
   ├── sftp_user_02
   └── service_account_01
```

For each account explain:

- Authentication
- SSH key
- Password
- UID
- Group
- Home directory
- Permissions
- Transfer directory
- Jobs
- Ownership
- Auditability

Explain that an account can be specific even when the underlying server and application are shared.

---

# 10. Directory / Filesystem-Level Security

Explain:

```text
User
 │
 ▼
Directory
 │
 ├── inbound
 ├── outbound
 ├── archive
 ├── error
 └── temporary
```

Cover:

- Unix permissions
- ACL
- Ownership
- Group access
- Read/write/execute
- Directory traversal
- File ownership
- File movement
- File pickup
- File delivery

Explain how a transfer can fail even when:

```text
Certificate = Valid
SSH Authentication = Successful
```

because:

```text
Filesystem Permission = Denied
```

This distinction should be emphasized.

---

# 11. Transfer Flow / Job Level

Explain the most specific level.

Example:

```text
Partner A
   │
   └── SFTP Connection
          │
          └── Service Account
                 │
                 └── Job: Invoice Transfer
                        │
                        ├── Source
                        ├── Destination
                        ├── Schedule
                        ├── Filename Pattern
                        ├── Encryption
                        ├── Compression
                        ├── Retry
                        └── Archive
```

Explain how two jobs for the same partner may use:

- Same server
- Same application
- Same user
- Different directory
- Different PGP key
- Different destination
- Different schedule
- Different encryption policy

Therefore:

> Application scope, credential scope, and flow scope must not automatically be assumed to be identical.

---

# 12. Generic vs Specific Classification

Create a formal classification framework.

Use categories:

### Generic / Shared

Potentially used by many:

- Root CA
- Common intermediate CA
- Shared truststore
- Application TLS certificate
- Shared cryptographic policy

### Semi-Generic

Used by a protocol or subsystem:

- SFTP configuration
- FTPS configuration
- Common PGP configuration
- Protocol-level trust configuration

### Partner-Specific

- Partner certificate
- Partner CA
- Partner PGP public key
- Partner SSH public key
- Partner connection profile

### User-Specific

- SSH public key
- Linux account
- Service account
- User-specific directory

### Flow-Specific

- Job-level encryption
- Job-level signing
- Flow-specific PGP key
- Flow-specific destination
- Flow-specific routing configuration

---

# 13. Storage Scope vs Usage Scope

This distinction must be explicitly documented.

Explain:

```text
Storage Scope
+
Usage Scope
```

Example:

```text
Global Certificate Store
       │
       └── Partner-A Certificate
                 │
                 └── Used by one connection
```

Therefore:

```text
Stored Globally
≠
Used Globally
```

Provide multiple examples.

---

# 14. Blast Radius / Production Impact Model

Introduce an impact classification.

For example:

```text
P0 / Platform-wide
P1 / Protocol-wide
P2 / Partner-wide
P3 / User-specific
P4 / Flow-specific
```

Do not assume these exact incident priorities are universal; explain them as a conceptual model.

Examples:

```text
Root CA failure
→ potentially many partners affected

Application TLS certificate failure
→ potentially application-wide impact

Partner certificate failure
→ one partner affected

User SSH key failure
→ one account affected

Flow-specific PGP key failure
→ one transfer flow affected
```

Explain how this helps production support engineers prioritize incidents.

---

# 15. Certificate and Key Lifecycle

Create a complete lifecycle:

```text
Generate
   ↓
Request
   ↓
Approve
   ↓
Issue
   ↓
Install
   ↓
Configure
   ↓
Validate
   ↓
Monitor
   ↓
Renew
   ↓
Rotate
   ↓
Revoke
   ↓
Remove
```

Explain:

- Expiry
- Renewal
- Rotation
- Revocation
- Replacement
- Certificate chain changes
- Key rotation
- Truststore updates
- Application restart requirements
- Validation after change

---

# 16. Production Troubleshooting Mapping

Create a troubleshooting matrix.

Examples:

| Symptom | Possible Security Artifact | Scope |
|---|---|---|
| TLS handshake failure | Server/client certificate | Application/connection |
| Certificate expired | X.509 certificate | Partner/application |
| Unknown CA | Truststore/CA | Application/connection |
| SSH host verification failure | Host key/known_hosts | Server/connection |
| SSH authentication failure | User public key/account | User |
| PGP decryption failure | Private key | Partner/flow |
| Signature verification failure | Public certificate/key | Partner/flow |
| Permission denied | Linux permissions/ACL | User/filesystem |
| File not found | Directory/path/job | Flow |
| Connection refused | Service/network | Platform |
| Only one partner failing | Partner-specific configuration | Partner |
| All partners failing | Platform/shared configuration | Application |

Expand this considerably.

---

# 17. Dependency Graph

Create dependency chains such as:

```text
Application
    ↓
Protocol
    ↓
Partner Connection
    ↓
User / Service Account
    ↓
Directory
    ↓
Transfer Job
    ↓
File
```

Then show security dependencies:

```text
Transfer Job
    ↓
Connection
    ↓
Authentication
    ↓
Certificate / SSH Key
    ↓
Trust
    ↓
Network
    ↓
Filesystem Permission
```

Explain how a production engineer can walk this dependency chain during RCA.

---

# 18. Certificate Inventory Model

Design a generic inventory structure.

For each certificate/key/security artifact include:

```text
Artifact ID
Artifact Type
Technology
Protocol
Name / Alias
Purpose
Scope
Storage Location
Usage Location
Owner
Application
Server
Partner
User
Connection
Flow
Issue Date
Expiry Date
Rotation Frequency
Trust Relationship
Dependency
Blast Radius
Monitoring
Change Procedure
Validation Procedure
Incident Symptoms
```

Do not include actual secrets.

---

# 19. Enterprise Documentation Hierarchy

Recommend a documentation hierarchy such as:

```text
01_Application_Overview
02_Architecture
03_Server_and_Linux
04_Protocols
05_Certificate_and_Key_Management
06_Partner_Connections
07_User_and_Service_Accounts
08_Directories_and_Permissions
09_Transfer_Flows
10_Monitoring
11_Incident_Troubleshooting
12_RCA_Knowledge
13_Certificate_Lifecycle
14_Change_Management
15_Disaster_Recovery
16_Security_and_Compliance
17_Glossary
```

Explain what should belong in each document.

---

# 20. Important Conceptual Distinctions

Explicitly teach the following distinctions:

```text
Authentication
vs
Authorization

Encryption
vs
Signing

Certificate
vs
Private Key

Public Key
vs
Private Key

Trust
vs
Identity

Server Identity
vs
User Identity

Application Scope
vs
Partner Scope

Partner Scope
vs
User Scope

User Scope
vs
Flow Scope

Storage Scope
vs
Usage Scope

Certificate Expiry
vs
Certificate Revocation

Network Failure
vs
Authentication Failure

Authentication Success
vs
Filesystem Authorization
```

For each distinction provide:

1. Definition
2. Example
3. Production scenario
4. Typical failure symptom
5. Troubleshooting direction

---

# 21. Realistic Legacy Enterprise Scenario

Create a fictional but realistic example.

For example:

```text
Enterprise MFT Platform
│
├── Linux Server 01
│
├── Linux Server 02
│
├── SFTP
│
├── FTPS
│
├── AS2
│
├── PGP
│
├── Partner A
│     ├── User A
│     └── Invoice Flow
│
├── Partner B
│     ├── Service Account B
│     └── Customer Flow
│
└── Partner C
      └── AS2 Flow
```

Then map all relevant certificates, keys, accounts, directories, protocols, trust relationships, and jobs onto this architecture.

Use fictional names only.

---

# 22. Production Engineer Mental Model

End the document with a practical mental model.

When a file transfer fails, teach the engineer to ask:

```text
1. Which application?
2. Which server?
3. Which protocol?
4. Which partner?
5. Which connection?
6. Which user/service account?
7. Which directory?
8. Which transfer job?
9. Which security mechanism?
10. Which certificate/key/trust artifact?
11. Is the artifact shared or specific?
12. What is the blast radius?
13. Has it expired?
14. Has configuration changed?
15. Is authentication successful?
16. Is authorization successful?
17. Can the filesystem be accessed?
18. Can the file be processed?
19. Can the destination be reached?
20. Is the issue reproducible for other users/partners?
```

The goal is to teach **scope-based troubleshooting rather than random certificate checking**.

---

# 23. Final Knowledge Model

Provide a final consolidated hierarchy:

```text
ENTERPRISE
│
└── FILE TRANSFER PLATFORM
    │
    ├── APPLICATION
    │   ├── TLS Certificates
    │   ├── Keystores
    │   ├── Truststores
    │   └── Shared CA
    │
    ├── LINUX SERVER
    │   ├── Host Keys
    │   ├── Users
    │   ├── Groups
    │   ├── Permissions
    │   └── Filesystem
    │
    ├── PROTOCOL
    │   ├── SFTP
    │   ├── FTPS
    │   ├── HTTPS
    │   └── AS2
    │
    ├── PARTNER
    │   ├── Certificates
    │   ├── SSH Keys
    │   ├── PGP Keys
    │   ├── Trust
    │   └── Connections
    │
    ├── USER / SERVICE ACCOUNT
    │   ├── Username
    │   ├── SSH Key
    │   ├── Permissions
    │   └── Home Directory
    │
    ├── DIRECTORY
    │   ├── Inbound
    │   ├── Outbound
    │   ├── Archive
    │   └── Error
    │
    └── FLOW / JOB
        ├── Authentication
        ├── Encryption
        ├── Signing
        ├── Routing
        ├── Scheduling
        └── File Transfer
```

---

# Documentation Standards

The resulting document must be:

- Enterprise-grade
- Vendor-neutral where possible
- Suitable for production-support knowledge management
- Suitable for onboarding engineers
- Hierarchical
- Conceptual before implementation-specific
- Detailed enough for an engineer with Linux/application-support experience
- Written in professional technical language
- Explicit about assumptions
- Clear about generic vs specific artifacts
- Clear about dependency and blast radius
- Clear about certificate/key lifecycle
- Clear about authentication, authorization, encryption, signing and trust
- Safe for documentation: never expose real secrets, passwords, private keys, tokens or production credentials

Do not invent vendor-specific behavior unless explicitly identified as an example.

Where a behavior differs between technologies or products, explain the variation rather than presenting one implementation as universally true.

Use diagrams, tables, examples, hierarchies, dependency models, troubleshooting scenarios, and glossary definitions wherever they improve understanding.

The final document should function as a **foundational Enterprise File Transfer Security & Credential Knowledge Repository**, not merely a certificate inventory.