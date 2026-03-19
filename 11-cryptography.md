# V11 Cryptography

## Control Objective

Protect sensitive data at rest and in transit using strong cryptographic algorithms and proper key management.

**Regulations**: SC-13, SC-14 (NIST 800-53), HIPAA 164.312(a)(2)(iv), NOFO SC-14

## V11.1 Encryption Algorithms

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **11.1.1** | Verify AES-128 or stronger for encryption | ✓ | ✓ | SC-13 | Strong symmetric encryption |
| **11.1.2** | Verify RSA-2048 or stronger for keys | ✓ | ✓ | SC-13 | Strong asymmetric encryption |
| **11.1.3** | Verify SHA-256 or stronger for hashing | ✓ | ✓ | SC-13 | Secure hashing |
| **11.1.4** | Verify deprecated algorithms not used | ✓ | ✓ | SC-13 | No MD5, SHA1, DES |

## V11.2 Key Management

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **11.2.1** | Verify keys stored in secure vault | ✓ | ✓ | SC-12, SC-13 | Key protection |
| **11.2.2** | Verify key rotation | ✓ | ✓ | SC-12 | Limits key exposure |
| **11.2.3** | Verify no hardcoded keys | ✓ | ✓ | SC-13 | Prevents source code leakage |
| **11.2.4** | Verify key derivation (PBKDF2/Argon2) | ✓ | ✓ | SC-13 | Secure password hashing |

## V11.3 Data Protection

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **11.3.1** | Verify PHI encrypted at rest | ✓ | ✓ | SC-13, HIPAA 164.312(a)(2)(iv) | HIPAA requirement |
| **11.3.2** | Verify PHI encrypted in transit | ✓ | ✓ | SC-08, HIPAA 164.312(e) | Network protection |
| **11.3.3** | Verify field-level encryption for PHI | | ✓ | SC-13 | NOFO SC-14 requirement |
| **11.3.4** | Verify database encryption (TDE) | | ✓ | SC-13 | Layered defense |

## V11.4 Cryptographic Agility

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **11.4.1** | Verify algorithm configuration | ✓ | ✓ | SC-13 | Not hardcoded |
| **11.4.2** | Verify PQC readiness | | ✓ | SC-13 | Post-quantum preparation |

**Key Principle**: Encryption is only as good as key management. Keys must be protected separately from encrypted data.

## References

- NIST SP 800-57 (Key Management)
- OWASP Cryptographic Failures Cheat Sheet
- NOFO FG-26-001 SC-14