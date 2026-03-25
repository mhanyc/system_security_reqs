# V3 Web Frontend Security

## Control Objective

Secure the client-side presentation layer against XSS, DOM manipulation, and other client-side attacks. Client-side security is critical for protecting user data and preventing account compromise.

**Regulations**: SC-04 (NIST 800-53)

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
| **3.3.1** | Verify CSP uses `script-src` with nonces or hashes (no `'unsafe-inline'`) | ✓ | ✓ | SC-04 | XSS prevention |
| **3.3.1a** | Verify CSP includes `frame-ancestors 'none'` | ✓ | ✓ | SC-04 | Clickjacking prevention |
| **3.3.1b** | Verify CSP includes `object-src 'none'` | ✓ | ✓ | SC-04 | Flash/plugin prevention |
| **3.3.1c** | Verify CSP includes `base-uri 'self'` | ✓ | ✓ | SC-04 | Base tag hijacking |
| **3.3.2** | Verify X-Content-Type-Options header | ✓ | ✓ | SC-04 | Prevents MIME sniffing |
| **3.3.3** | Verify X-Frame-Options header | ✓ | ✓ | SC-04 | Prevents clickjacking |
| **3.3.4** | Verify Referrer-Policy header | ✓ | ✓ | SC-04 | Controls referrer leakage |

## V3.4 Accessible Security

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **3.4.1** | Verify security controls are accessible | ✓ | ✓ | SC-04 | Security must not impede accessibility |
| **3.4.2** | Verify error messages are accessible | ✓ | ✓ | SC-04 | Screen reader compatible |

## V3.7 External Resource Integrity

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **3.7.1** | Verify Subresource Integrity (SRI) for external scripts/stylesheets | | ✓ | SC-04 | CDN compromise protection |
| **3.7.2** | Verify external resources loaded from approved domains only | ✓ | ✓ | SC-04 | Limits trust boundary |
| **3.7.3** | Verify CSP includes integrity hash verification | | ✓ | SC-04 | Defense in depth |

**Rationale for Deviation**: ASVS 5.0 V3.6 covers this. Vibrant marks Enhanced because:
- SRI requires build pipeline changes (hash generation)
- CDN compromise affects Enhanced systems handling PHI
- Baseline systems may use bundled resources only

**Key Principle**: Never trust client-side data for security decisions—all validation must be repeated server-side.

## References

- OWASP DOM Security Cheat Sheet
- NIST SP 800-53 SC-4, DS-04