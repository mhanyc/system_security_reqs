# V13 Configuration

## Control Objective

Ensure secure configuration of application runtime, dependencies, and deployment. Misconfiguration is a leading cause of breaches.

**Regulations**: SC-04, SC-07 (NIST 800-53)

## V13.1 Application Configuration

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **13.1.1** | Verify production debug disabled | ✓ | ✓ | SC-04 | No information leakage |
| **13.1.2** | Verify default credentials changed | ✓ | ✓ | SC-04 | Prevent default access |
| **13.1.3** | Verify error handling configured | ✓ | ✓ | SC-04 | No stack traces |
| **13.1.4** | Verify secure headers | ✓ | ✓ | SC-04 | Defense in depth |

## V13.2 Dependency Management

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **13.2.1** | Verify dependency scanning | ✓ | ✓ | SC-04 | Known vulnerabilities |
| **13.2.2** | Verify no vulnerable components | ✓ | ✓ | SC-04 | Keep updated |
| **13.2.3** | Verify SBOM generation | | ✓ | SC-04 | Inventory management |

## V13.3 Deployment Configuration

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :---: | :---: | :---: | :---: | :--- |
| **13.3.1** | Verify secrets not in code | ✓ | ✓ | SC-04 | No credential exposure |
| **13.3.2** | Verify environment separation | ✓ | ✓ | SC-07 | Dev/prod isolation |
| **13.3.3** | Verify secure defaults | ✓ | ✓ | SC-04 | Fail-secure |
| **13.3.4** | Verify automated deployment | ✓ | ✓ | SC-04 | Repeatable process |

## V13.4 Infrastructure Configuration

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **13.4.1** | Verify cloud security posture | ✓ | ✓ | SC-07 | Cloud misconfigurations |
| **13.4.2** | Verify container security | ✓ | ✓ | SC-04 | Isolation |
| **13.4.3** | Verify IaC scanning | | ✓ | SC-04 | Infrastructure as Code |

## V13.5 Backup and Recovery Security

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **13.5.1** | Verify backup encryption at rest | ✓ | ✓ | [HIPAA 164.312(a)(2)(iv)](https://www.ecfr.gov/current/title-45/subtitle-A/subchapter-C/part-164/subpart-C/section-164.312) | PHI protection |
| **13.5.2** | Verify backup integrity verification | ✓ | ✓ | [ASVS 5.0 V13.1.4](https://github.com/OWASP/ASVS/blob/master/5.0/en/0x20-V13-Config.md) | Ransomware recovery |
| **13.5.3** | Verify offline/air-gapped backup copies | | ✓ | [NIST SP 800-61](https://csrc.nist.gov/publications/detail/sp/800-61/final) | Ransomware resilience |
| **13.5.4** | Verify backup restoration testing quarterly | ✓ | ✓ | [HIPAA 164.308(a)(7)](https://www.ecfr.gov/current/title-45/subtitle-A/subchapter-C/part-164/subpart-C/section-164.308) | Contingency planning |
| **13.5.5** | Verify backup access limited to recovery roles | ✓ | ✓ | SC-11 | Least privilege |

**Rationale for Deviation**: ASVS V13.1 mentions backups but lacks depth. Vibrant requires this because:
- Ransomware attacks on healthcare are rising 45% YoY ([HHS Ransomware Guidance](https://www.hhs.gov/hipaa/for-professionals/security/guidance/cybersecurity/index.html))
- HIPAA requires data backup plan as required safeguard [164.308(a)(7)](https://www.ecfr.gov/current/title-45/subtitle-A/subchapter-C/part-164/subpart-C/section-164.308)
- 988 Lifeline cannot afford extended downtime during crisis

**Key Principle**: Secure defaults, minimal exposure, and automated scanning for vulnerabilities.

## References

- OWASP Top 10:2021 - Security Misconfiguration
- NIST SP 800-53 SC-04, SC-07