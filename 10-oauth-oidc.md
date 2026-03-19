# V10 OAuth and OIDC

## Control Objective

Securely implement OAuth 2.0 and OpenID Connect for authorization and federated identity. Protect against redirect manipulation and token theft.

**Regulations**: SC-12 (NIST 800-53)

## V10.1 OAuth 2.0 Security

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **10.1.1** | Verify redirect_uri validation | ✓ | ✓ | SC-12 | Prevents redirect hijacking |
| **10.1.2** | Verify state parameter use | ✓ | ✓ | SC-12 | CSRF prevention |
| **10.1.3** | Verify PKCE implementation | ✓ | ✓ | SC-12 | Authorization code theft prevention |
| **10.1.4** | Verify code flow only (no implicit) | ✓ | ✓ | SC-12 | Modern security |

## V10.2 Token Security

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **10.2.1** | Verify short-lived access tokens | ✓ | ✓ | SC-12 | Limits token exposure |
| **10.2.2** | Verify secure token storage | ✓ | ✓ | SC-12 | Prevents theft |
| **10.2.3** | Verify refresh token rotation | | ✓ | SC-12 | Replay prevention |

## V10.3 OpenID Connect

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **10.3.1** | Verify ID token validation | ✓ | ✓ | SC-12 | Issuer, audience, signature |
| **10.3.2** | Verify nonce for ID token | ✓ | ✓ | SC-12 | Replay prevention |
| **10.3.3** | Verify userinfo endpoint security | ✓ | ✓ | SC-12 | HTTPS required |
| **10.3.4** | Verify identity provider validation | ✓ | ✓ | SC-12 | Trusted IdP only |

## V10.4 Provider Security

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **10.4.1** | Verify scopes are limited | ✓ | ✓ | SC-12 | Minimum necessary |
| **10.4.2** | Verify token revocation | ✓ | ✓ | SC-12 | Logout functionality |
| **10.4.3** | Verify session management | ✓ | ✓ | SC-12 | Federated logout |

**Key Principle**: OAuth/OIDC simplifies authentication but requires careful implementation to prevent token theft and redirect attacks.

## References

- OAuth 2.0 Security Best Current Practice
- NIST SP 800-63
- OWASP OAuth Cheat Sheet