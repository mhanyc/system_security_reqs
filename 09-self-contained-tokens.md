# V9 Self-contained Tokens

## Control Objective

Securely implement bearer tokens, JWTs, and MAC tokens. Tokens must be signed, validated, and protected against tampering.

**Regulations**: SC-12, SC-13 (NIST 800-53)

## V9.1 Token Structure

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **9.1.1** | Verify signed tokens (HMAC or cryptographic) | ✓ | ✓ | SC-13 | Prevents tampering |
| **9.1.2** | Verify algorithm is specified (not "none") | ✓ | ✓ | SC-13 | Prevents algorithm confusion |
| **9.1.3** | Verify token expiration | ✓ | ✓ | SC-12 | Limits exposure |
| **9.1.4** | Verify token audience/issuer validation | ✓ | ✓ | SC-13 | Prevents token reuse |

## V9.2 JWT Security

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **9.2.1** | Verify RS256 or HS256 (not none) | ✓ | ✓ | SC-13 | Secure signing |
| **9.2.2** | Verify claims validation (exp, iat, iss, aud) | ✓ | ✓ | SC-13 | Token integrity |
| **9.2.3** | Verify no sensitive data in JWT payload | ✓ | ✓ | SC-13 | Token can be decoded |
| **9.2.4** | Verify key management | ✓ | ✓ | SC-12 | Secure key storage |
| **9.2.5** | Verify asymmetric signatures (RS256/ES256) preferred over HMAC | ✓ | ✓ | [ASVS 5.0 V9.2.5](https://github.com/OWASP/ASVS/blob/master/5.0/en/0x14-V9-Communication.md) | Key separation |
| **9.2.6** | Verify key ID (kid) validation to prevent key confusion attacks | ✓ | ✓ | [RFC 7517](https://tools.ietf.org/html/rfc7517) | Algorithm confusion prevention |
| **9.2.7** | Verify JWT "none" algorithm rejected | ✓ | ✓ | [ASVS 5.0 V9.2.4](https://github.com/OWASP/ASVS/blob/master/5.0/en/0x14-V9-Communication.md) | Auth bypass prevention |

## V9.3 Token Storage and Transmission

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **9.3.1** | Verify tokens transmitted over HTTPS | ✓ | ✓ | SC-08 | Prevents interception |
| **9.3.2** | Verify tokens not stored in localStorage | | ✓ | SC-04 | XSS theft prevention |
| **9.3.3** | Verify short-lived access tokens | ✓ | ✓ | SC-12 | Limits breach impact |

## V9.4 Token Revocation

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **9.4.1** | Verify token blacklist/blocklist | | ✓ | SC-12 | Logout functionality |
| **9.4.2** | Verify refresh token rotation | | ✓ | SC-12 | Prevents token replay |
| **9.4.3** | Verify token reuse detection | | ✓ | SC-12 | Replay attack prevention |

**Key Principle**: Tokens are bearer credentials—anyone with the token is authorized. Secure transmission and short lifespans are critical.

## References

- OWASP JSON Web Token Cheat Sheet
- NIST SP 800-53 SC-12, SC-13