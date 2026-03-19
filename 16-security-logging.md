# V16 Security Logging and Error Handling

## Control Objective

Log security events for audit and forensic purposes. Handle errors securely without leaking sensitive information.

**Regulations**: SC-08, SC-10 (NIST 800-53), HIPAA 164.312(b), NOFO SC-10

## V16.1 Security Logging

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **16.1.1** | Verify authentication events logged | ✓ | ✓ | SC-08, AU-02 | Account activity |
| **16.1.2** | Verify authorization events logged | ✓ | ✓ | SC-08 | Access decisions |
| **16.1.3** | Verify sensitive operations logged | ✓ | ✓ | SC-08 | Audit trail |
| **16.1.4** | Verify error conditions logged | ✓ | ✓ | SC-08 | Problem detection |

## V16.2 Log Content

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **16.2.1** | Verify timestamp with timezone | ✓ | ✓ | SC-08 | Forensics |
| **16.2.2** | Verify user identification | ✓ | ✓ | SC-08, AU-03 | Attribution |
| **16.2.3** | Verify outcome included | ✓ | ✓ | SC-08 | Success/failure |
| **16.2.4** | Verify sufficient context | ✓ | ✓ | SC-08 | Investigation support |

## V16.3 Log Protection

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **16.3.1** | Verify log integrity | ✓ | ✓ | SC-08 | Tamper evidence |
| **16.3.2** | Verify log storage protection | ✓ | ✓ | SC-08, HIPAA 164.312(b) | HIPAA requirement |
| **16.3.3** | Verify log retention | ✓ | ✓ | SC-08 | Compliance |
| **16.3.4** | Verify centralized logging | ✓ | ✓ | SC-08 | NOFO SC-10 |

## V16.4 Error Handling

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **16.4.1** | Verify no sensitive data in errors | ✓ | ✓ | SC-04 | Information leakage |
| **16.4.2** | Verify generic error messages | ✓ | ✓ | SC-04 | User enumeration prevention |
| **16.4.3** | Verify stack traces not exposed | ✓ | ✓ | SC-04 | Attack intelligence |
| **16.4.4** | Verify custom error pages | ✓ | ✓ | SC-04 | Consistent UX |

**Key Principle**: Log what's needed for security monitoring and forensics, but never log sensitive data like passwords or PHI.

## References

- OWASP Logging Cheat Sheet
- NIST SP 800-53 SC-08, AU family
- NOFO FG-26-001 SC-10