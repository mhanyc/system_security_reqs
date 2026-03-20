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

## V16.1.5 PHI Access Logging

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **16.1.5** | Verify all PHI access logged (create, read, update, delete) | ✓ | ✓ | [HIPAA 164.312(b)](https://www.ecfr.gov/current/title-45/subtitle-A/subchapter-C/part-164/subpart-C/section-164.312) | Required audit controls |
| **16.1.6** | Verify PHI access logs include: user ID, patient ID, action, timestamp, data elements accessed | ✓ | ✓ | HIPAA 164.312(b) | Complete audit trail |
| **16.1.7** | Verify failed PHI access attempts logged | ✓ | ✓ | HIPAA 164.312(b) | Detect unauthorized attempts |

**Rationale for Deviation**: ASVS has "sensitive operations logged" but not PHI-specific. Vibrant requires this because:
- HIPAA explicitly requires audit controls that record who accessed PHI and when
- OCR investigations focus on PHI access audit capabilities
- Failed access attempts indicate reconnaissance or unauthorized access attempts

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
| **16.3.3** | Verify audit logs retained for 6+ years (HIPAA documentation) | ✓ | ✓ | [HIPAA 164.316(b)(1)](https://www.ecfr.gov/current/title-45/subtitle-A/subchapter-C/part-164/subpart-C/section-164.316) | Federal retention requirement |

**Rationale for Deviation**: ASVS recommends retention but doesn't specify duration. Vibrant requires this because:
- HIPAA mandates 6-year retention for security policy documentation
- OCR investigations may review historical access patterns
- Breach notification requires analysis of historical logs
| **16.3.4** | Verify centralized logging | ✓ | ✓ | SC-08 | NOFO SC-10 |

## V16.4 Log Analysis

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **16.4.1** | Verify audit logs reviewed for inappropriate access | ✓ | ✓ | [HIPAA 164.312(b)](https://www.ecfr.gov/current/title-45/subtitle-A/subchapter-C/part-164/subpart-C/section-164.312) | OCR guidance requirement |
| **16.4.2** | Verify automated analysis for anomalous patterns | | ✓ | [NIST SP 800-53 AU-6](https://csrc.nist.gov/projects/risk-management/sp800-53-controls/release-search#!/control?version=5.1&number=AU-6) | Insider threat detection |
| **16.4.3** | Verify review documented with findings and remediation | ✓ | ✓ | HIPAA 164.312(b) | Accountability |

**Rationale**: HIPAA requires audit controls, not just log collection.

## V16.5 Error Handling

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **16.5.1** | Verify no sensitive data in errors | ✓ | ✓ | SC-04 | Information leakage |
| **16.5.2** | Verify generic error messages | ✓ | ✓ | SC-04 | User enumeration prevention |
| **16.5.3** | Verify stack traces not exposed | ✓ | ✓ | SC-04 | Attack intelligence |
| **16.5.4** | Verify custom error pages | ✓ | ✓ | SC-04 | Consistent UX |

**Key Principle**: Log what's needed for security monitoring and forensics, but never log sensitive data like passwords or PHI in plain text.

## References

- OWASP Logging Cheat Sheet
- NIST SP 800-53 SC-08, AU family
- NOFO FG-26-001 SC-10