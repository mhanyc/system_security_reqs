# V4 API and Web Service

## Control Objective

Secure REST APIs and web services against injection, authentication bypass, and data exposure. APIs are the primary attack surface for modern applications.

**Regulations**: SC-04, SC-12, SC-13 (NIST 800-53), HIPAA 164.312(e)

## V4.1 General API Security

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **4.1.1** | Verify all APIs require authentication | ✓ | ✓ | SC-13 | No unauthenticated access |
| **4.1.2** | Verify API rate limiting | ✓ | ✓ | SC-04 | Prevents abuse and DoS |
| **4.1.3** | Verify API versioning | ✓ | ✓ | SC-04 | Allows secure deprecation |
| **4.1.4** | Verify API response headers | ✓ | ✓ | SC-04 | Security headers required |

## V4.2 REST API Security

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **4.2.1** | Verify proper HTTP method enforcement | ✓ | ✓ | SC-04 | GET for read, POST for write |
| **4.2.2** | Verify input validation on all endpoints | ✓ | ✓ | SC-04 | Every input must be validated |
| **4.2.3** | Verify output encoding for all responses | ✓ | ✓ | SC-04 | Context-specific encoding |
| **4.2.4** | Verify pagination on list endpoints | ✓ | ✓ | SC-04 | Prevents data exposure |

## V4.3 Web Service Security

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **4.3.1** | Verify SOAP/XML security | ✓ | ✓ | SC-04 | XXE prevention |
| **4.3.2** | Verify JSON security | ✓ | ✓ | SC-04 | JSON injection prevention |
| **4.3.3** | Verify WSDL exposure control | ✓ | ✓ | SC-04 | Don't expose service definitions |

## V4.4 GraphQL Security (if applicable)

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **4.4.1** | Verify query complexity limits | ✓ | ✓ | SC-04 | Prevents DoS via complex queries |
| **4.4.2** | Verify depth limiting | ✓ | ✓ | SC-04 | Prevents nested query attacks |
| **4.4.3** | Verify introspection controls | | ✓ | SC-04 | Limit in production |

**Key Principle**: APIs must validate all input, authenticate all requests, and encode all output.

## References

- OWASP API Security Top 10
- NIST SP 800-53 SC-4, SC-12, SC-13