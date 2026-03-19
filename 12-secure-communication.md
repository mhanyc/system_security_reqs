# V12 Secure Communication

## Control Objective

Ensure all network communications are encrypted and authenticated. Protect against interception, man-in-the-middle, and data leakage.

**Regulations**: SC-08, SC-09 (NIST 800-53), HIPAA 164.312(e), NOFO SC-15

## V12.1 Transport Layer Security

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **12.1.1** | Verify TLS 1.2 or 1.3 only | ✓ | ✓ | SC-08 | Secure protocols |
| **12.1.2** | Verify strong cipher suites | ✓ | ✓ | SC-08 | No weak encryption |
| **12.1.3** | Verify TLS configuration | ✓ | ✓ | SC-08 | No protocol downgrade |
| **12.1.4** | Verify certificate validation | ✓ | ✓ | SC-08 | Prevents MITM |

## V12.2 Application Layer Security

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **12.2.1** | Verify HTTPS everywhere | ✓ | ✓ | SC-08 | No cleartext HTTP |
| **12.2.2** | Verify HSTS header | ✓ | ✓ | SC-08 | Enforces HTTPS |
| **12.2.3** | Verify certificate pinning | | ✓ | SC-08 | Prevents MITM |

## V12.3 Internal Communication

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **12.3.1** | Verify service-to-service TLS | ✓ | ✓ | SC-08 | Internal encryption |
| **12.3.2** | Verify mTLS for sensitive services | | ✓ | SC-08 | Mutual authentication |
| **12.3.3** | Verify database TLS | ✓ | ✓ | SC-08 | Database encryption |

## V12.4 Network Security

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **12.4.1** | Verify network segmentation | ✓ | ✓ | SC-07 | Isolate sensitive systems |
| **12.4.2** | Verify firewall rules | ✓ | ✓ | SC-07 | Minimal exposure |
| **12.4.3** | Verify no sensitive data in URLs | ✓ | ✓ | SC-08 | Prevents log leakage |

**Key Principle**: All communication must be encrypted—internal services are not trusted by default.

## References

- OWASP Transport Layer Protection Cheat Sheet
- NIST SP 800-52
- NOFO FG-26-001 SC-15