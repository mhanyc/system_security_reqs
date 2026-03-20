# System Security Requirements

Vibrant Emotional Health's system security requirements based on OWASP ASVS 5.0.

## About This Repository

This repository contains Vibrant's development of system security requirements. It is maintained by the InfoSec team.

## Requirements Index (ASVS 5.0)

| # | Chapter | Description |
| :---: | :--- | :--- |
| 1 | [Encoding and Sanitization](./01-encoding-sanitization.md) | Input validation, output encoding, injection prevention |
| 2 | [Validation and Business Logic](./02-validation-business-logic.md) | Business logic controls, data validation |
| 3 | [Web Frontend Security](./03-web-frontend-security.md) | Client-side security, DOM security |
| 4 | [API and Web Service](./04-api-web-service.md) | REST API security, web services |
| 5 | [File Handling](./05-file-handling.md) | File upload, storage, processing |
| 6 | [Authentication](./06-authentication.md) | Identity verification, credential management |
| 7 | [Session Management](./07-session-management.md) | Session lifecycle, token security |
| 8 | [Authorization](./08-authorization.md) | Access control, permissions |
| 9 | [Self-contained Tokens](./09-self-contained-tokens.md) | JWT, bearer tokens, MAC tokens |
| 10 | [OAuth and OIDC](./10-oauth-oidc.md) | Federation, identity providers |
| 11 | [Cryptography](./11-cryptography.md) | Encryption, key management |
| 12 | [Secure Communication](./12-secure-communication.md) | TLS, network security |
| 13 | [Configuration](./13-configuration.md) | Security configuration, deployment |
| 14 | [Data Protection](./14-data-protection.md) | PII, PHI, sensitive data |
| 15 | [Secure Coding and Architecture](./15-secure-coding-architecture.md) | Security architecture, SDLC |
| 16 | [Security Logging and Error Handling](./16-security-logging.md) | Audit logging, error handling |
| 17 | [WebRTC](./17-webrtc.md) | Real-time communication security |

## Understanding Requirement Levels

This repository uses a simplified two-level structure:

| Level | Description | When Required |
| :--- | :--- | :--- |
| **Baseline** | Essential security controls | All systems |
| **Enhanced** | Beyond basics for higher sensitivity | Systems handling sensitive data |

> **Note**: ASVS Level 3 (Advanced) is excluded due to organizational maturity. These requirements represent achievable near-future goals.

## Regulatory Context

These requirements align with:

- **HIPAA Security Rule** (45 CFR 164.302-318) - Technical safeguards for PHI
- **42 CFR Part 2** - Substance use disorder (SUD) record protections
- **NIST SP 800-53 Rev. 5** - Security control catalog (SC, AC, AU families)
- **NOFO FG-26-001** - 988 Lifeline grant requirements

Each requirement includes a regulatory column showing applicable controls.

## System Architecture

```mermaid
%%{init: {'theme':'default', 'themeVariables': {'fontSize':'16px'}}}%%
graph TB
    %% Accessibility attributes
    accTitle: "Vibrant Security Architecture"
    accDescr: "Four-layer security architecture for 988 Lifeline: Untrusted Zone (Browser, Mobile), DMZ/Edge Layer (WAF, API Gateway), Trusted Service Layer (Authentication, Session Management, Access Control, Business Logic, Validation, Emergency Access), and Data Layer (Database, Cache, Secrets Vault, Centralized Logging, Audit Analysis, Token Vault). All communications use HTTPS. Emergency Access provides break-glass functionality for crisis intervention."

    subgraph "Untrusted Zone"
        Browser[🌐 Web Browser<br/>End Users]
        Mobile[📱 Mobile Apps<br/>End Users]
    end

    subgraph "DMZ / Edge Layer"
        WAF[🛡️ Web Application<br/>Firewall]
        API[⚡ API Gateway<br/>Rate Limiting, Auth]
    end

    subgraph "Trusted Service Layer"
        Auth[🔐 Authentication<br/>Service]
        Session[📋 Session<br/>Management]
        Access[✓ Access Control<br/>Enforcement]
        BizLogic[⚙️ Business Logic<br/>Application Services]
        Validation[✓ Input Validation<br/>Output Encoding]
        Emergency[🚨 Emergency Access<br/>Break-Glass]
    end

    subgraph "Data Layer"
        DB[💾 Primary<br/>Database]
        Cache[⚡ Cache/<br/>Session Store]
        Vault[🔒 Secrets<br/>Vault]
        Logs[📄 Centralized<br/>Logging]
        Audit[🔍 Audit Analysis<br/>Log Review]
        TokenVault[🎫 Token Vault<br/>Tokenization]
    end

    Browser -->|🔒 HTTPS Only| WAF
    Mobile -->|🔒 HTTPS Only| API
    WAF --> Validation
    API --> Validation
    Validation --> Auth
    Auth --> Session
    Session --> Access
    Access --> BizLogic
    BizLogic --> DB
    Auth --> Vault
    Auth --> Emergency
    Emergency --> Access
    Session --> Cache
    BizLogic --> Logs
    Auth --> Logs
    Access --> Logs
    Logs --> Audit
    BizLogic --> TokenVault

    %% High-contrast linkStyles for WCAG compliance
    linkStyle 0 stroke:#333333,stroke-width:2px
    linkStyle 1 stroke:#333333,stroke-width:2px
    linkStyle 2 stroke:#333333,stroke-width:2px
    linkStyle 3 stroke:#333333,stroke-width:2px
    linkStyle 4 stroke:#333333,stroke-width:2px
    linkStyle 5 stroke:#333333,stroke-width:2px
    linkStyle 6 stroke:#333333,stroke-width:2px
    linkStyle 7 stroke:#333333,stroke-width:2px
    linkStyle 8 stroke:#333333,stroke-width:2px
    linkStyle 9 stroke:#333333,stroke-width:3px
    linkStyle 10 stroke:#333333,stroke-width:2px
    linkStyle 11 stroke:#333333,stroke-width:2px
    linkStyle 12 stroke:#333333,stroke-width:2px
    linkStyle 13 stroke:#333333,stroke-width:2px
    linkStyle 14 stroke:#333333,stroke-width:2px
    linkStyle 15 stroke:#333333,stroke-width:2px

    %% Standardized color palette with accessibility compliance
    classDef untrusted fill:#ffe5e5,stroke:#cc0000,stroke-width:3px,color:#000000
    classDef dmz fill:#fff3cd,stroke:#856404,stroke-width:3px,color:#212529
    classDef trusted fill:#e8f5e8,stroke:#2e7d32,stroke-width:2px,color:#1b5e20
    classDef data fill:#e3f2fd,stroke:#1565c0,stroke-width:2px,color:#0d47a1
    classDef emergency fill:#fff3cd,stroke:#856404,stroke-width:4px,color:#212529

    class Browser,Mobile untrusted
    class WAF,API dmz
    class Auth,Session,Access,BizLogic,Validation trusted
    class Emergency emergency
    class DB,Cache,Vault,Logs,Audit,TokenVault data
```

**Key Architectural Principles**:
- All security controls enforced in Trusted Service Layer
- Trust boundaries clearly defined at edge/DMZ
- Communications encrypted between zones
- Input validation occurs before processing
- All access logged for audit trails

## Deviations from ASVS 5.0

This document extends ASVS 5.0 to address healthcare-specific requirements and 988 Lifeline operational needs. The following deviations are documented with rationale:

| Requirement | ASVS Status | Deviation Rationale | Regulatory Basis |
| :--- | :--- | :--- | :--- |
| **V8.4** (New Section) | Not in ASVS | 988 Lifeline requires emergency access for crisis intervention; normal authentication may block life-saving access | HIPAA 164.312(a)(2)(ii) |
| **V14.3.1** (Expanded) | Modified from ASVS | 42 CFR Part 2 requires 8 specific consent elements; generic consent is invalid under criminal penalty framework | 42 CFR 2.31, 2.32 |
| **V14.5** (New Section) | Not in ASVS | Part 2 permits disclosure to medical personnel in bona fide emergencies; lack of procedures could delay crisis intervention | 42 CFR 2.51 |
| **V16.1.5** (New Section) | Modified from ASVS | HIPAA explicitly requires PHI-specific audit controls; OCR investigations focus on PHI access capabilities | HIPAA 164.312(b) |
| **V16.3.3** (Enhanced) | Modified from ASVS | HIPAA mandates 6-year retention for security documentation; ASVS recommends but doesn't specify duration | HIPAA 164.316(b)(1) |
| **V1.5** (New Section) | In ASVS V1.5 | Deserialization attacks achieve RCE; 988 systems are high-value targets requiring L1 compliance | ASVS 5.0 V1.5 |
| **V2.4** (New Section) | In ASVS V2.2 | Separated for auditability; 988 faces targeted credential stuffing (counselor accounts = access to vulnerable callers) | ASVS 5.0 V2.2.6-2.2.7 |
| **V11.3.3, V11.3.4** (Baseline) | Modified from ASVS | Field-level encryption and TDE are baseline HIPAA requirements, not "enhanced" security | HIPAA 164.312(a)(2)(iv) |
| **V14.3.4** (Baseline) | Modified from ASVS | Accounting of disclosures is required by HIPAA §164.528, not optional | HIPAA 164.528 |
| **V8.2.3, V8.2.4** (Baseline) | Modified from ASVS | Time/location-based access controls are standard for PHI protection | HIPAA 164.312(a)(1) |

## Usage

These requirements should match Vibrant's consensus on achievable state-of-the-art. If you see anything that doesn't pass the Vibrant smell test, please reach out!

## Origin

Requirements based on [OWASP Application Security Verification Standard (ASVS) 5.0](https://owasp.org/www-project-application-security-verification-standard/), released May 2025.

ASVS was chosen because:
- Maps directly to NIST guidelines
- Offers tiered requirements for maturity variation
- Represents consensus best practice in application security