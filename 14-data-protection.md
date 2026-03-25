# V14 Data Protection

## Control Objective

Protect personally identifiable information (PII) and protected health information (PHI) throughout its lifecycle. Ensure compliance with HIPAA and 42 CFR Part 2.

**Regulations**: SC-04, SC-13, SC-14 (NIST 800-53), HIPAA 164.312, 42 CFR Part 2, NOFO SC-15

## V14.1 Data Classification

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **14.1.1** | Verify data classification policy | ✓ | ✓ | SC-04 | Identify sensitive data |
| **14.1.2** | Verify PHI identification | ✓ | ✓ | HIPAA 164.302 | HIPAA requirement |
| **14.1.3** | Verify SUD record identification | ✓ | ✓ | 42 CFR Part 2 | Stricter protections |
| **14.1.4** | Verify 988 caller data protections | ✓ | ✓ | NOFO SC-15 | Crisis hotline specific |

## V14.2 Data Handling

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **14.2.1** | Verify data minimization | ✓ | ✓ | SC-04 | Collect only necessary |
| **14.2.2** | Verify purpose limitation | ✓ | ✓ | SC-04 | Use for stated purpose only |
| **14.2.3** | Verify retention policies | ✓ | ✓ | SC-04 | Delete when no longer needed |
| **14.2.4** | Verify secure deletion | | ✓ | SC-04 | Data destruction |

## V14.3 Privacy Controls

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **14.3.1** | Verify Part 2 consent includes: patient name, disclosing program, information description, recipient, purpose, expiration, patient signature | ✓ | ✓ | 42 CFR 2.31 | Federal law requirement |
| **14.3.1a** | Verify Part 2 consent includes re-disclosure prohibition notice to recipient | ✓ | ✓ | 42 CFR 2.32 | Required notice |
| **14.3.1b** | Verify Part 2 consent expiration is event-based or time-limited | ✓ | ✓ | 42 CFR 2.31 | Limits disclosure scope |

**Rationale for Deviation**: ASVS has generic "consent management." Vibrant requires this because:
- 42 CFR Part 2 imposes criminal penalties (not just civil fines) for unauthorized SUD disclosure [42 CFR 2.4](https://www.ecfr.gov/current/title-42/chapter-I/subchapter-A/part-2/subpart-A/section-2.4)
- Part 2 consent must include 8 specific elements; generic consent is invalid
- Re-disclosure prohibition notice is mandatory for all SUD record recipients
| **14.3.2** | Verify data export capability | ✓ | ✓ | HIPAA 164.524 | Patient access |
| **14.3.3** | Verify data correction capability | ✓ | ✓ | HIPAA 164.526 | Patient rights |
| **14.3.4** | Verify accounting of disclosures | ✓ | ✓ | HIPAA 164.528 | HIPAA §164.528 requires accounting |

## V14.4 42 CFR Part 2 Specific (SUD Records)

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **14.4.1** | Verify SUD records marked | ✓ | ✓ | 42 CFR 2.31 | Identification |
| **14.4.2** | Verify research protections | ✓ | ✓ | 42 CFR 2.52 | Research use |
| **14.4.3** | Verify disclosure logging | ✓ | ✓ | 42 CFR 2.31 | Audit requirements |

## V14.5 Emergency Disclosures (42 CFR Part 2)

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **14.5.1** | Verify medical emergency disclosure procedures without prior consent | ✓ | ✓ | 42 CFR 2.51 | Medical emergency exception |
| **14.5.2** | Verify emergency disclosure limited to treating professionals | ✓ | ✓ | 42 CFR 2.51 | Limits scope |
| **14.5.3** | Verify emergency disclosure documented with medical necessity | ✓ | ✓ | 42 CFR 2.51 | Audit requirement |
| **14.5.4** | Verify patient notified of emergency disclosure post-event | | ✓ | 42 CFR 2.51 | Patient rights |

**Rationale for Deviation**: ASVS has no emergency disclosure concept. Vibrant requires this because:
- 988 Lifeline is a medical/crisis service where immediate disclosure may save lives
- Part 2 permits disclosure to medical personnel in bona fide emergencies
- Lack of documented emergency procedures could delay crisis intervention

## V14.6 Data Tokenization

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **14.6.1** | Verify tokenization available for PHI where appropriate | | ✓ | SC-13 | Reduce exposure scope |
| **14.6.2** | Verify token vault separate from application databases | | ✓ | SC-13 | Isolation |
| **14.6.3** | Verify token-to-data mapping protected with encryption | | ✓ | SC-13 | Confidentiality |

**Rationale**: Tokenization reduces data exposure for analytics and testing environments.

**Key Principle**: 988 crisis data requires heightened protections under both HIPAA and 42 CFR Part 2.

## References

- HIPAA Security Rule (45 CFR 164.312)
- 42 CFR Part 2
- NOFO FG-26-001 SC-15