# V6 Authentication

## Control Objective

Verify user identity securely using modern, evidence-based authentication methods. Protect against credential theft, brute force, and account takeover.

**Regulations**: SC-12 (NIST 800-53 IA family), HIPAA 164.312(d), NOFO SC-12

## V6.1 Password Security

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **6.1.1** | Verify minimum password length (12+ characters) | ✓ | ✓ | SC-12, NIST 800-63b | Length over complexity |
| **6.1.2** | Verify maximum password length (128 chars) | ✓ | ✓ | SC-12 | Buffer overflow prevention |
| **6.1.3** | Verify breach database checking | ✓ | ✓ | SC-12 | Prevents compromised passwords |
| **6.1.4** | Verify no password composition rules | ✓ | ✓ | SC-12 | Allows passphrases |
| **6.1.5** | Verify no periodic password rotation | ✓ | ✓ | SC-12 | NIST recommendation |
| **6.1.6** | Verify password manager compatibility | ✓ | ✓ | SC-12 | UX and security |

## V6.2 Authenticator Security

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **6.2.1** | Verify rate limiting on login | ✓ | ✓ | SC-12 | Prevents brute force |
| **6.2.2** | Verify account lockout policy | ✓ | ✓ | SC-12 | Limits brute force attempts |
| **6.2.3** | Verify generic error messages | ✓ | ✓ | SC-12 | Prevents user enumeration |
| **6.2.4** | Verify secure password reset flow | ✓ | ✓ | SC-12 | No account takeover via reset |

## V6.3 Multi-Factor Authentication

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **6.3.1** | Verify MFA is offered to all users | ✓ | ✓ | SC-12 | Stronger account protection |
| **6.3.2** | Verify MFA is required for privileged accounts | | ✓ | SC-12 | NOFO SC-12 requirement |
| **6.3.3** | Verify TOTP or hardware keys preferred | ✓ | ✓ | SC-12 | Phishing-resistant methods |
| **6.3.4** | Verify SMS/email are secondary only | ✓ | ✓ | SC-12 | NIST restricted methods |

## V6.4 Credential Storage

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **6.4.1** | Verify password hashing (Argon2/bcrypt) | ✓ | ✓ | SC-13, SC-14 | Protects against breach |
| **6.4.2** | Verify salt per password | ✓ | ✓ | SC-13 | Prevents rainbow tables |
| **6.4.3** | Verify constant-time comparison | ✓ | ✓ | SC-13 | Prevents timing attacks |

**Key Principle**: Modern authentication emphasizes length over complexity, breach checking, and MFA.

## References

- NIST SP 800-63b
- OWASP Authentication Cheat Sheet
- NOFO FG-26-001 SC-12