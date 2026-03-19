# V17 WebRTC

## Control Objective

Secure real-time communication using WebRTC. Protect against eavesdropping, injection, and unauthorized access to media streams.

**Regulations**: SC-04, SC-08, SC-13 (NIST 800-53)

## V17.1 Media Security

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **17.1.1** | Verify SRTP for media encryption | ✓ | ✓ | SC-13 | Media confidentiality |
| **17.1.2** | Verify DTLS for key exchange | ✓ | ✓ | SC-13 | Secure key exchange |
| **17.1.3** | Verify encrypted signaling | ✓ | ✓ | SC-08 | Control channel protection |

## V17.2 Authentication and Access Control

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **17.2.1** | Verify ICE candidate filtering | ✓ | ✓ | SC-04 | IP address leakage |
| **17.2.2** | Verify STUN/TURN authentication | ✓ | ✓ | SC-12 | Relay access control |
| **17.2.3** | Verify room/access authentication | ✓ | ✓ | SC-12 | Only authorized users |

## V17.3 Browser Security

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **17.3.1** | Verify getUserMedia permissions | ✓ | ✓ | SC-04 | User consent |
| **17.3.2** | Verify indicator for active streams | ✓ | ✓ | SC-04 | Transparency |
| **17.3.3** | Verify same-origin policy | ✓ | ✓ | SC-04 | Cross-origin restrictions |

## V17.4 TURN Server Security

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **17.4.1** | Verify TURN server authentication | ✓ | ✓ | SC-12 | Relay authorization |
| **17.4.2** | Verify TURN encryption | ✓ | ✓ | SC-08 | Relay traffic protection |
| **17.4.3** | Verify bandwidth limits | ✓ | ✓ | SC-04 | Resource management |

**Key Principle**: WebRTC media must be encrypted end-to-end using SRTP with DTLS key exchange.

## References

- WebRTC Security Architecture
- OWASP WebRTC Security Cheat Sheet
- NIST SP 800-53 SC-04, SC-08, SC-13