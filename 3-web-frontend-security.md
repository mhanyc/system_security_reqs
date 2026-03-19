# V3 Web Frontend Security

## Control Objective

Secure the client-side presentation layer against XSS, DOM manipulation, and other client-side attacks. Client-side security is critical for protecting user data and preventing account compromise.

**Regulations**: SC-04 (NIST 800-53), DS-04 (21st Century IDEA Act - accessibility)

## V3.1 Client-Side Input Handling

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **3.1.1** | Verify client-side input validation is not trusted | ✓ | ✓ | SC-04 | Client validation is for UX only |
| **3.1.2** | Verify sensitive data not exposed in client-side code | ✓ | ✓ | SC-04, SC-13 | Prevents credential leakage |
| **3.1.3** | Verify no secrets in browser storage | ✓ | ✓ | SC-04, SC-13 | Tokens/secrets must not be in localStorage |

## V3.2 DOM Security

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **3.2.1** | Verify safe DOM manipulation APIs | ✓ | ✓ | SC-04 | Use textContent, not innerHTML |
| **3.2.2** | Verify URL parameters are sanitized | ✓ | ✓ | SC-04 | Prevents DOM-based XSS |
| **3.2.3** | Verify postMessage source validation | ✓ | ✓ | SC-04 | Prevents cross-origin attacks |
| **3.2.4** | Verify iframe sandboxing | ✓ | ✓ | SC-04 | Limits iframe capabilities |

## V3.3 Content Security

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **3.3.1** | Verify Content Security Policy (CSP) header | ✓ | ✓ | SC-04 | Mitigates XSS attacks |
| **3.3.2** | Verify X-Content-Type-Options header | ✓ | ✓ | SC-04 | Prevents MIME sniffing |
| **3.3.3** | Verify X-Frame-Options header | ✓ | ✓ | SC-04 | Prevents clickjacking |
| **3.3.4** | Verify Referrer-Policy header | ✓ | ✓ | SC-04 | Controls referrer leakage |

## V3.4 Accessible Security

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **3.4.1** | Verify security controls are accessible | ✓ | ✓ | DS-04 | Security must not impede accessibility |
| **3.4.2** | Verify error messages are accessible | ✓ | ✓ | DS-04 | Screen reader compatible |

**Key Principle**: Never trust client-side data for security decisions—all validation must be repeated server-side.

## References

- OWASP DOM Security Cheat Sheet
- NIST SP 800-53 SC-4, DS-04