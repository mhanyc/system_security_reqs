# V7 Session Management

## Control Objective

Securely manage user sessions from creation to destruction. Prevent session hijacking, fixation, and unauthorized access.

**Regulations**: SC-12, SC-13 (NIST 800-53), HIPAA 164.312(a)

## V7.1 Session Creation

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **7.1.1** | Verify cryptographically random session IDs (128+ bits) | ✓ | ✓ | SC-13 | Prevents guessing |
| **7.1.2** | Verify new session after authentication | ✓ | ✓ | SC-12 | Prevents session fixation |
| **7.1.3** | Verify session ID not in URL | ✓ | ✓ | SC-12 | Prevents log leakage |

## V7.2 Session Security

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **7.2.1** | Verify Secure cookie flag | ✓ | ✓ | SC-08 | HTTPS only |
| **7.2.2** | Verify HttpOnly cookie flag | ✓ | ✓ | SC-04 | Prevents XSS theft |
| **7.2.3** | Verify SameSite cookie attribute | ✓ | ✓ | SC-04 | CSRF prevention |
| **7.2.4** | Verify session timeout | ✓ | ✓ | SC-12 | Limits exposure window |

## V7.3 Session Lifecycle

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **7.3.1** | Verify session termination on logout | ✓ | ✓ | SC-12 | User control over session |
| **7.3.2** | Verify session timeout on inactivity | ✓ | ✓ | SC-12 | NOFO SC-13 requirement |
| **7.3.3** | Verify session invalidation on password change | | ✓ | SC-12 | Prevents continued access |
| **7.3.4** | Verify concurrent session limits | | ✓ | SC-12 | Prevents account sharing |

## V7.4 Session Binding

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **7.4.1** | Verify device fingerprinting | | ✓ | SC-12 | Detects session theft |
| **7.4.2** | Verify re-authentication for sensitive actions | | ✓ | SC-12 | Extra verification |

**Key Principle**: Session IDs must be unguessable, transmitted securely, and invalidated when logged out or expired.

## References

- OWASP Session Management Cheat Sheet
- NIST SP 800-53 SC-12, SC-13