# V2 Validation and Business Logic

## Control Objective

Ensure applications validate data consistency and enforce business logic correctly. Business logic flaws can allow attackers to manipulate workflow, bypass security controls, or cause financial loss.

**Regulations**: SC-04 (NIST 800-53), HIPAA 164.312(a)

## V2.1 Input Validation for Business Logic

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **2.1.1** | Verify business logic validates all inputs | ✓ | ✓ | SC-04 | Ensures data meets business rules |
| **2.1.2** | Verify numeric limits and ranges | ✓ | ✓ | SC-04 | Prevents overflow attacks |
| **2.1.3** | Verify date/time validation | ✓ | ✓ | SC-04 | Prevents temporal manipulation |

## V2.2 Business Logic Controls

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **2.2.1** | Verify workflows complete in correct sequence | ✓ | ✓ | SC-04 | Prevents step-skipping attacks |
| **2.2.2** | Verify no business logic bypass | ✓ | ✓ | SC-04 | All validation must occur server-side |
| **2.2.3** | Verify idempotency for critical operations | ✓ | ✓ | SC-04 | Prevents duplicate submissions |
| **2.2.4** | Verify rate limiting on business operations | ✓ | ✓ | SC-04 | Prevents business logic abuse |
| **2.2.5** | Verify limits on batch operations | ✓ | ✓ | SC-04 | Prevents resource exhaustion |

## V2.3 Data Consistency

| # | Requirement | Baseline | Enhanced | Regulations | Why It Matters |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **2.3.1** | Verify referential integrity | ✓ | ✓ | SC-04 | Prevents orphaned records |
| **2.3.2** | Verify state consistency | ✓ | ✓ | SC-04 | Prevents race conditions |
| **2.3.3** | Verify cross-field validation | ✓ | ✓ | SC-04 | Related fields must be consistent |

**Key Principle**: Business logic validation must occur server-side—client-side checks are for UX only.

## References

- OWASP Business Logic Security Cheat Sheet
- NIST SP 800-53 SC-4