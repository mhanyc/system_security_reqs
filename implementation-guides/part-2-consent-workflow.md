# 42 CFR Part 2 Consent Workflow

Implementation guide for V14.3.1 Part 2 consent requirements.

## Overview

42 CFR Part 2 requires specific consent elements for disclosing Substance Use Disorder (SUD) records. Violations carry criminal penalties. This guide covers the UI/UX implementation of compliant consent workflows.

## Requirements Mapping

| Requirement | Consent Element | UI Implementation |
| :--- | :--- | :--- |
| **14.3.1** | Patient name | Pre-populated, read-only field |
| **14.3.1** | Disclosing program name | Dropdown of authorized programs |
| **14.3.1** | Information description | Checkbox list of record types |
| **14.3.1** | Recipient name/address | Free text with validation |
| **14.3.1** | Purpose of disclosure | Dropdown + free text option |
| **14.3.1** | Expiration (event/date) | Date picker or event selection |
| **14.3.1** | Patient signature | Electronic signature capture |
| **14.3.1a** | Re-disclosure prohibition notice | Required checkbox acknowledgment |
| **14.3.1b** | Expiration type validation | Event-based vs. time-limited logic |

## Consent Form Design

### Wireframe Structure

```
┌─────────────────────────────────────────────────────────────┐
│           42 CFR Part 2 Consent Authorization               │
├─────────────────────────────────────────────────────────────┤
│ Patient: [Jane Doe]          Date: [03/19/2026]            │
├─────────────────────────────────────────────────────────────┤
│ 1. DISCLOSING PROGRAM                                        │
│    [Dropdown: Vibrant Crisis Center ▼]                       │
├─────────────────────────────────────────────────────────────┤
│ 2. INFORMATION TO BE DISCLOSED                              │
│    [✓] Assessment records                                  │
│    [✓] Crisis intervention notes                           │
│    [ ] Referral history                                    │
│    [ ] Treatment plan                                      │
├─────────────────────────────────────────────────────────────┤
│ 3. RECIPIENT                                                │
│    Name:    [________________________]                     │
│    Address: [________________________]                     │
├─────────────────────────────────────────────────────────────┤
│ 4. PURPOSE                                                  │
│    [Dropdown: Treatment coordination ▼]                      │
│    Additional details: [________________________]          │
├─────────────────────────────────────────────────────────────┤
│ 5. EXPIRATION (select one)                                  │
│    ( ) Specific date: [__/__/____]                          │
│    ( ) Event-based:   [Dropdown ▼]                          │
├─────────────────────────────────────────────────────────────┤
│ ⚠️ RE-DISCLOSURE PROHIBITION                                │
│ Federal regulations (42 CFR Part 2) prohibit the recipient  │
│ from re-disclosing this information without your specific   │
│ written consent. Violations may result in criminal         │
│ prosecution.                                                 │
│                                                              │
│ [✓] I understand this prohibition applies to the recipient │
├─────────────────────────────────────────────────────────────┤
│ [Review Consent]              [Clear Signature]             │
├─────────────────────────────────────────────────────────────┤
│ Signature: _____________________ Date: ___________         │
└─────────────────────────────────────────────────────────────┘
```

## Validation Rules

### Required Field Validation

```javascript
const part2ConsentValidation = {
  patientName: { required: true, readOnly: true },
  disclosingProgram: { required: true },
  informationTypes: { required: true, min: 1 },
  recipientName: { required: true, minLength: 2 },
  recipientAddress: { required: true, minLength: 10 },
  purpose: { required: true },
  expiration: { required: true, type: 'date|event' },
  redisclosureAcknowledgment: { required: true, mustBeChecked: true },
  patientSignature: { required: true, minLength: 3 },
  signatureDate: { required: true, notFuture: true }
};
```

### Expiration Logic

```python
def validate_expiration(consent):
    """Validate expiration is event-based or time-limited per 42 CFR 2.31"""
    if consent.expiration_type == 'DATE':
        assert consent.expiration_date <= today() + timedelta(days=365), \
            "Date-based consent cannot exceed 1 year"
    elif consent.expiration_type == 'EVENT':
        assert consent.expiration_event in VALID_EVENTS, \
            f"Event must be one of: {VALID_EVENTS}"
    else:
        raise ValidationError("Expiration must be date-based or event-based")
```

## API Schema

### Consent Record Structure

```json
{
  "consent_id": "consent_abc123",
  "patient_id": "patient_456",
  "regulation": "42_CFR_PART_2",
  "elements": {
    "patient_name": "Jane Doe",
    "disclosing_program": {
      "id": "prog_vibrant_crisis",
      "name": "Vibrant Crisis Center",
      "address": "123 Main St, City, ST 12345"
    },
    "information_description": [
      "assessment_records",
      "crisis_intervention_notes"
    ],
    "recipient": {
      "name": "Dr. Sarah Smith",
      "address": "456 Medical Plaza, City, ST 12345",
      "type": "healthcare_provider"
    },
    "purpose": "treatment_coordination",
    "purpose_details": "Coordination of care following crisis intervention",
    "expiration": {
      "type": "DATE",
      "date": "2026-09-19"
    },
    "redisclosure_prohibition_notice": true,
    "signature": {
      "value": "base64_encoded_signature_image",
      "timestamp": "2026-03-19T14:30:00Z",
      "method": "electronic"
    }
  },
  "created_at": "2026-03-19T14:30:00Z",
  "created_by": "counselor_789",
  "status": "ACTIVE"
}
```

## Re-disclosure Notice Requirement

### Mandatory Notice Text

```
Federal regulations (42 CFR Part 2) protect the confidentiality of substance use disorder
patient records. The recipient of this information is prohibited from re-disclosing this
information without your specific written consent. Violations may result in criminal
prosecution under federal law.
```

### UI Implementation

```html
<div class="notice-box warning">
  <h4>⚠️ Re-disclosure Prohibition Notice</h4>
  <p>Federal regulations (42 CFR Part 2) protect the confidentiality of substance use
  disorder patient records. The recipient of this information is prohibited from
  re-disclosing this information without your specific written consent. Violations may
  result in criminal prosecution under federal law.</p>

  <label>
    <input type="checkbox" required name="redisclosure_ack" id="redisclosure_ack">
    I understand this prohibition applies to the recipient
  </label>
</div>
```

## Audit Trail

Each consent action must be logged:

```json
{
  "event_type": "PART2_CONSENT_CREATED",
  "timestamp": "2026-03-19T14:30:00Z",
  "consent_id": "consent_abc123",
  "patient_id": "patient_456",
  "counselor_id": "counselor_789",
  "elements_validated": [
    "patient_name", "disclosing_program", "information_description",
    "recipient", "purpose", "expiration", "redisclosure_notice", "signature"
  ],
  "ip_address": "10.0.1.100"
}
```

## Testing Scenarios

### Valid Consent Test

```python
def test_valid_part2_consent():
    consent = create_part2_consent(
        patient_name="Jane Doe",
        disclosing_program="Vibrant Crisis Center",
        information_types=["assessment"],
        recipient_name="Dr. Smith",
        recipient_address="123 Medical Plaza",
        purpose="treatment_coordination",
        expiration_type="DATE",
        expiration_value="2026-09-19",
        redisclosure_acknowledged=True,
        signature="Jane Doe"
    )
    assert consent.status == "ACTIVE"
    assert consent.redisclosure_notice_validated == True
```

### Missing Re-disclosure Acknowledgment

```python
def test_missing_redisclosure_acknowledgment():
    with pytest.raises(ValidationError) as exc:
        create_part2_consent(
            # ... valid fields ...
            redisclosure_acknowledged=False
        )
    assert "Re-disclosure acknowledgment required" in str(exc.value)
```

## Compliance Checklist

- [ ] All 8 Part 2 consent elements captured
- [ ] Re-disclosure prohibition notice displayed and acknowledged
- [ ] Expiration validated as event-based or time-limited
- [ ] Electronic signature meets E-SIGN Act requirements
- [ ] Audit log captures all consent actions
- [ ] Consent records retained per 42 CFR 2.16 (minimum 3 years)
- [ ] Patient provided copy of signed consent

## References

- [42 CFR 2.31 Consent Requirements](https://www.ecfr.gov/current/title-42/chapter-I/subchapter-A/part-2/subpart-C/section-2.31)
- [42 CFR 2.32 Re-disclosure Prohibition](https://www.ecfr.gov/current/title-42/chapter-I/subchapter-A/part-2/subpart-C/section-2.32)
- [SAMHSA Part 2 Guidance](https://www.samhsa.gov/sites/default/files/2024-03/part-2-faqs-2024.pdf)
