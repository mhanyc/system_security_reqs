# V8 Authorization

## Control Objective

Ensure users can only access resources they are authorized for. Implement fine-grained access control with the principle of least privilege.

**Regulations**: SC-12, SC-13 (NIST 800-53), HIPAA 164.312(a), NOFO SC-13

## V8.1 Access Control Architecture

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **8.1.1** | Verify centralized access control | ✓ | ✓ | SC-13 | Single enforcement point |
| **8.1.2** | Verify enforcement on trusted layer | ✓ | ✓ | SC-04 | Never trust client-side |
| **8.1.3** | Verify deny by default | ✓ | ✓ | SC-13 | Explicit permissions only |
| **8.1.4** | Verify role-based access control (RBAC) | ✓ | ✓ | SC-13 | Manageable permissions |

## V8.2 Permission Management

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **8.2.1** | Verify least privilege | ✓ | ✓ | SC-13 | Minimum necessary access |
| **8.2.2** | Verify separation of duties | | ✓ | SC-13 | Prevents fraud |
| **8.2.3** | Verify time-based access controls | ✓ | ✓ | SC-12 | Context-aware access is standard for PHI |
| **8.2.4** | Verify location-based access controls | ✓ | ✓ | SC-12 | Geographic access control for PHI |

## V8.3 Data Access Control

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **8.3.1** | Verify row-level security | ✓ | ✓ | SC-13, HIPAA | User data isolation |
| **8.3.2** | Verify field-level access control | | ✓ | SC-13 | PHI field protection |
| **8.3.3** | Verify API access control | ✓ | ✓ | SC-13 | No unauthorized endpoints |

## V8.4 Emergency Access (Break-Glass)

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **8.4.1** | Verify emergency access (break-glass) procedures exist | ✓ | ✓ | HIPAA 164.312(a)(2)(ii) | Medical emergency access |
| **8.4.2** | Verify emergency access requires dual authorization | ✓ | ✓ | HIPAA 164.312(a)(2)(ii) | Prevents abuse |
| **8.4.3** | Verify all emergency access is logged with justification | ✓ | ✓ | HIPAA 164.312(b) | Audit trail |
| **8.4.4** | Verify automatic notification to security on emergency access | ✓ | ✓ | SC-07 | Immediate awareness |
| **8.4.5** | Verify post-emergency access review within 24 hours | | ✓ | HIPAA 164.312(a)(2)(ii) | Compliance validation |

**Rationale for Deviation**: ASVS does not include break-glass requirements. Vibrant requires this because:
- 988 Lifeline operates 24/7/365 crisis services where normal authentication may block life-saving access
- HIPAA mandates emergency access procedures for workforce members providing patient care [45 CFR 164.312(a)(2)(ii)](https://www.ecfr.gov/current/title-45/subtitle-A/subchapter-C/part-164/subpart-C/section-164.312)
- Crisis intervention requires immediate access to caller history during emergencies

## V8.5 Privilege Escalation Prevention

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **8.5.1** | Verify no IDOR vulnerabilities | ✓ | ✓ | SC-13 | Prevent unauthorized access |
| **8.5.2** | Verify horizontal access control | ✓ | ✓ | SC-13 | User data isolation |
| **8.5.3** | Verify vertical access control | ✓ | ✓ | SC-13 | Privilege boundary enforcement |

**Key Principle**: Access control must be enforced server-side with a single, centralized mechanism.

## References

- OWASP Access Control Cheat Sheet
- NIST SP 800-53 SC-12, SC-13
- NOFO FG-26-001 SC-13