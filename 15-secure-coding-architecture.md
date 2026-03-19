# V15 Secure Coding and Architecture

## Control Objective

Integrate security throughout the software development lifecycle. Build security into architecture, design, and implementation.

**Regulations**: SC-04 (NIST 800-53), NOFO SC-10

## V15.1 Security Architecture

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **15.1.1** | Verify threat modeling | ✓ | ✓ | SC-04 | Identify threats early |
| **15.1.2** | Verify security requirements | ✓ | ✓ | SC-04 | Define security needs |
| **15.1.3** | Verify trust boundaries defined | ✓ | ✓ | SC-04 | Architecture clarity |
| **15.1.4** | Verify centralized security controls | ✓ | ✓ | SC-04 | Consistent enforcement |

## V15.2 Secure Development

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **15.2.1** | Verify secure coding standards | ✓ | ✓ | SC-04 | Developer guidance |
| **15.2.2** | Verify security testing in CI/CD | ✓ | ✓ | SC-04 | NOFO SC-10 requirement |
| **15.2.3** | Verify code review process | ✓ | ✓ | SC-04 | Peer validation |
| **15.2.4** | Verify static analysis | ✓ | ✓ | SC-04 | Automated scanning |

## V15.3 Security Testing

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **15.3.1** | Verify penetration testing | | ✓ | SC-04 | NOFO SC-10 requirement |
| **15.3.2** | Verify vulnerability scanning | ✓ | ✓ | SC-04 | Ongoing assessment |
| **15.3.3** | Verify dependency scanning | ✓ | ✓ | SC-04 | Known vulnerabilities |

## V15.4 Incident Response Preparation

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **15.4.1** | Verify logging for forensics | ✓ | ✓ | SC-04, SC-08 | Incident investigation |
| **15.4.2** | Verify secure backup | ✓ | ✓ | SC-04 | Business continuity |
| **15.4.3** | Verify incident response plan | | ✓ | SC-04 | NOFO SC-10 requirement |

**Key Principle**: Security must be built in, not bolted on. Start security from architecture through deployment.

## References

- OWASP SAMM
- NIST SP 800-53 SC-04
- NOFO FG-26-001 SC-10