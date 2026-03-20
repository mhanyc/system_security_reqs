# V1 Encoding and Sanitization

## Control Objective

Prevent injection attacks by properly validating input and encoding output. Input validation and output encoding are the most critical defenses against XSS, SQL injection, and other injection vulnerabilities.

**Regulations**: SC-04 (NIST 800-53), HIPAA 164.312(a)

## V1.1 Input Validation

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **1.1.1** | Verify defense against HTTP parameter pollution | ✓ | ✓ | SC-04 | Prevents attackers from manipulating parameters to bypass validation |
| **1.1.2** | Verify protection against mass parameter assignment | ✓ | ✓ | SC-04 | Prevents attackers from setting protected fields via API |
| **1.1.3** | Verify all input uses positive validation (allow lists) | ✓ | ✓ | SC-04, HIPAA 164.312(a) | Blocks unknown malicious input before processing |
| **1.1.4** | Verify structured data is strongly typed and validated | ✓ | ✓ | SC-04 | Ensures data conforms to expected format |
| **1.1.5** | Verify URL redirects use allow lists | ✓ | ✓ | SC-04 | Prevents open redirect attacks |

## V1.2 Sanitization and Sandboxing

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **1.2.1** | Verify untrusted HTML input is sanitized | ✓ | ✓ | SC-04 | Prevents XSS via WYSIWYG editors |
| **1.2.2** | Verify unstructured data sanitization | ✓ | ✓ | SC-04 | Protects against malicious file contents |
| **1.2.3** | Verify mail input sanitization | ✓ | ✓ | SC-04 | Prevents SMTP injection |
| **1.2.4** | Verify avoidance of eval() and dynamic code execution | ✓ | ✓ | SC-04 | Prevents code injection |
| **1.2.5** | Verify template injection protection | ✓ | ✓ | SC-04 | Prevents server-side template injection |
| **1.2.6** | Verify SSRF protection | ✓ | ✓ | SC-04 | Prevents server-side request forgery |
| **1.2.7** | Verify SVG sanitization | ✓ | ✓ | SC-04 | Prevents XSS via SVG files |
| **1.2.8** | Verify template language sandboxing | ✓ | ✓ | SC-04 | Prevents injection via Markdown, CSS |

## V1.3 Output Encoding

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **1.3.1** | Verify context-specific output encoding | ✓ | ✓ | SC-04 | Different contexts need different escaping |
| **1.3.2** | Verify Unicode and character set handling | ✓ | ✓ | SC-04 | Prevents encoding-based attacks |
| **1.3.3** | Verify protection against XSS | ✓ | ✓ | SC-04 | Critical for PHI protection |
| **1.3.4** | Verify use of parameterized queries | ✓ | ✓ | SC-04, HIPAA 164.312(e) | Prevents SQL injection |
| **1.3.5** | Verify context-specific SQL escaping | ✓ | ✓ | SC-04 | Defense in depth |
| **1.3.6** | Verify JSON injection protection | ✓ | ✓ | SC-04 | Prevents JSON-based attacks |
| **1.3.7** | Verify LDAP injection protection | ✓ | ✓ | SC-04 | Prevents directory injection |
| **1.3.8** | Verify OS command injection protection | ✓ | ✓ | SC-04 | Prevents command execution |

## V1.5 Secure Deserialization

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **1.5.1** | Verify secure deserialization of untrusted data (JSON, XML, YAML) | ✓ | ✓ | [ASVS 5.0 V1.5](https://github.com/OWASP/ASVS/blob/master/5.0/en/0x11-V1-Architecture.md) | Prevents RCE via deserialization |
| **1.5.2** | Verify XML parsers disable DTDs and external entities (XXE prevention) | ✓ | ✓ | [ASVS 5.0 V1.5.2](https://github.com/OWASP/ASVS/blob/master/5.0/en/0x11-V1-Architecture.md) | Prevents XXE attacks |
| **1.5.3** | Verify type checking on deserialized objects | ✓ | ✓ | ASVS 5.0 V1.5.3 | Prevents type confusion |
| **1.5.4** | Verify no insecure deserialization functions (pickle, ObjectInputStream) | ✓ | ✓ | [OWASP Top 10:2021 A08](https://owasp.org/Top10/A08_2021-Software_and_Data_Integrity_Failures/) | Known vulnerability |

**References**:
- [CWE-502: Deserialization of Untrusted Data](https://cwe.mitre.org/data/definitions/502.html)
- [OWASP Deserialization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Deserialization_Cheat_Sheet.html)

**Rationale**: ASVS L1 requirement. Deserialization attacks achieve RCE with single request; 988 systems are high-value targets.

**Key Principle**: Output encoding is context-specific—HTML, JavaScript, SQL, URL, and OS commands each require different encoding.

**Examples**:
```javascript
// Allow list validation
const isValid = /^[a-zA-Z0-9_]{3,20}$/.test(input);

// Parameterized query
db.prepare('SELECT * FROM users WHERE name = ?').run(name);

// Context-specific encoding
element.textContent = userInput; // HTML context
```

## References

- OWASP Input Validation Cheat Sheet
- NIST SP 800-53 SC-4