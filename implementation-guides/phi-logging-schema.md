# PHI Audit Log Schema

Implementation guide for V16.1.5 PHI Access Logging requirements.

## Overview

HIPAA 164.312(b) requires audit controls that record who accessed PHI and when. This guide defines the PHI-specific audit log schema for Vibrant's 988 Lifeline systems.

## Requirements Mapping

| Requirement | Log Element | Purpose |
| :--- | :--- | :--- |
| **16.1.5** | Create, read, update, delete (CRUD) operations | Complete PHI lifecycle tracking |
| **16.1.6** | User ID, patient ID, action, timestamp, data elements | Forensic attribution |
| **16.1.7** | Failed access attempts | Unauthorized access detection |

## Log Schema

### Core PHI Access Event

```json
{
  "event_metadata": {
    "event_id": "evt_abc123",
    "event_version": "1.0",
    "timestamp": "2026-03-19T14:30:00.000Z",
    "event_type": "PHI_ACCESS",
    "severity": "INFO"
  },
  "actor": {
    "user_id": "counselor_789",
    "user_type": "CRISIS_COUNSELOR",
    "session_id": "sess_def456",
    "ip_address": "10.0.1.100",
    "user_agent": "Mozilla/5.0...",
    "mfa_verified": true
  },
  "patient": {
    "patient_id": "patient_123",
    "mrn": "****5678",
    "phi_classification": ["988_CALLER", "SUD_RECORD"]
  },
  "action": {
    "operation": "READ",
    "resource_type": "CRISIS_NOTE",
    "resource_id": "note_ghi789",
    "data_elements_accessed": [
      "call_timestamp",
      "risk_assessment",
      "safety_plan"
    ],
    "justification": "Crisis callback follow-up"
  },
  "context": {
    "application": "crisis-line-web",
    "endpoint": "/api/patients/123/notes",
    "method": "GET",
    "query_params": {"include_safety_plan": "true"},
    "response_status": 200
  },
  "result": {
    "success": true,
    "records_returned": 5,
    "execution_time_ms": 45
  }
}
```

### Failed Access Event

```json
{
  "event_metadata": {
    "event_id": "evt_fail789",
    "event_version": "1.0",
    "timestamp": "2026-03-19T14:32:15.000Z",
    "event_type": "PHI_ACCESS_DENIED",
    "severity": "WARNING"
  },
  "actor": {
    "user_id": "counselor_999",
    "user_type": "CRISIS_COUNSELOR",
    "session_id": "sess_unauth001",
    "ip_address": "203.0.113.50",
    "user_agent": "Mozilla/5.0...",
    "mfa_verified": true
  },
  "patient": {
    "patient_id": "patient_456",
    "mrn": "****9012",
    "phi_classification": ["988_CALLER"]
  },
  "action": {
    "operation": "READ",
    "resource_type": "PATIENT_RECORD",
    "resource_id": "record_jkl012",
    "attempted_access": "full_history"
  },
  "denial": {
    "reason": "INSUFFICIENT_PRIVILEGES",
    "policy_violated": "patient_assignment",
    "user_assigned_patients": ["patient_123", "patient_789"],
    "attempted_patient": "patient_456"
  },
  "context": {
    "application": "crisis-line-web",
    "endpoint": "/api/patients/456/history",
    "method": "GET",
    "response_status": 403
  }
}
```

## Event Types

| Event Type | Description | Example |
| :--- | :--- | :--- |
| `PHI_CREATE` | New PHI record created | Crisis note created |
| `PHI_READ` | PHI accessed/viewed | Counselor views patient history |
| `PHI_UPDATE` | PHI modified | Safety plan updated |
| `PHI_DELETE` | PHI marked deleted | Soft delete of note |
| `PHI_EXPORT` | PHI exported/downloaded | Report generation |
| `PHI_ACCESS_DENIED` | Unauthorized access attempt | Wrong patient accessed |
| `PHI_BULK_ACCESS` | Batch PHI access | List query returning multiple patients |
| `PHI_EMERGENCY_ACCESS` | Break-glass access | Emergency override used |

## Data Elements Reference

### PHI Classification Tags

```python
PHI_CLASSIFICATIONS = [
    "988_CALLER",        # 988 Lifeline caller data
    "SUD_RECORD",        # 42 CFR Part 2 protected
    "PSYCHOTHERAPY",     # Psychotherapy notes (separate consent)
    "MINOR",             # Patient under 18
    "HIGH_RISK",         # Suicide/self-harm risk flagged
    "RESEARCH_SUBJECT",  # Enrolled in research study
]
```

### Operation Types

```python
PHI_OPERATIONS = [
    "CREATE",
    "READ",
    "UPDATE",
    "DELETE",
    "EXPORT",
    "SEARCH",
    "SHARE",      # Disclosure to third party
    "PRINT",
    "DOWNLOAD"
]
```

### Denial Reasons

```python
ACCESS_DENIED_REASONS = [
    "UNAUTHENTICATED",
    "INSUFFICIENT_PRIVILEGES",
    "PATIENT_NOT_ASSIGNED",
    "TIME_RESTRICTED",
    "LOCATION_RESTRICTED",
    "EMERGENCY_ONLY",
    "SUD_CONSENT_REQUIRED",
    "SESSION_EXPIRED"
]
```

## Implementation

### Logging Middleware (Python/FastAPI)

```python
from fastapi import Request
from datetime import datetime
import hashlib

class PHIAuditLogger:
    def __init__(self, audit_service):
        self.audit = audit_service

    async def log_phi_access(
        self,
        request: Request,
        user: User,
        patient: Patient,
        operation: str,
        resource_type: str,
        resource_id: str,
        data_elements: list[str],
        success: bool,
        denial_reason: str = None
    ):
        event = {
            "event_metadata": {
                "event_id": generate_event_id(),
                "timestamp": datetime.utcnow().isoformat(),
                "event_type": f"PHI_{operation}" if success else "PHI_ACCESS_DENIED",
                "severity": "WARNING" if not success else "INFO"
            },
            "actor": {
                "user_id": user.id,
                "user_type": user.role,
                "session_id": request.session_id,
                "ip_address": request.client.host,
                "mfa_verified": user.mfa_verified
            },
            "patient": {
                "patient_id": patient.id,
                "mrn": mask_mrn(patient.mrn),
                "phi_classification": patient.classifications
            },
            "action": {
                "operation": operation,
                "resource_type": resource_type,
                "resource_id": resource_id,
                "data_elements_accessed": data_elements
            }
        }

        if not success:
            event["denial"] = {"reason": denial_reason}

        await self.audit.publish(event)
```

### Database Trigger Example (PostgreSQL)

```sql
-- Function to log PHI access
CREATE OR REPLACE FUNCTION log_phi_access()
RETURNS TRIGGER AS $$
BEGIN
    INSERT INTO phi_audit_log (
        event_timestamp,
        event_type,
        user_id,
        patient_id,
        operation,
        table_name,
        record_id,
        ip_address
    ) VALUES (
        NOW(),
        'PHI_ACCESS',
        current_setting('app.current_user_id'),
        NEW.patient_id,
        TG_OP,
        TG_TABLE_NAME,
        NEW.id,
        current_setting('app.client_ip')
    );
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

-- Trigger on patient records table
CREATE TRIGGER patient_phi_audit
AFTER SELECT ON patient_records
FOR EACH ROW
EXECUTE FUNCTION log_phi_access();
```

## Log Retention

### Storage Tiers

| Tier | Retention | Storage | Encryption |
| :--- | :--- | :--- | :--- |
| Hot (0-90 days) | 90 days | Elasticsearch | AES-256 |
| Warm (91 days-2 years) | ~2 years | S3 Standard | AES-256 |
| Cold (2-6 years) | 6 years | S3 Glacier | AES-256 |
| Archive (6+ years) | Permanent | Glacier Deep | AES-256 |

### HIPAA Retention

```yaml
# phi-log-retention.yaml
retention_policy:
  minimum_retention_years: 6
  regulatory_basis: "HIPAA 164.316(b)(1)"
  automatic_deletion: false  # Legal hold may apply
  geographic_restriction: "US_ONLY"
  encryption_at_rest: "AES_256_GCM"
```

## Monitoring and Alerting

### Anomaly Detection Rules

```yaml
# phi-audit-alerts.yaml
alerts:
  - name: "Unusual Patient Access Pattern"
    condition: "user_unique_patients_24h > 50"
    severity: "WARNING"
    notification: "security-team@vibrant.org"

  - name: "After-Hours PHI Access"
    condition: "hour < 6 OR hour > 22"
    severity: "INFO"
    require_justification: true

  - name: "Failed Access Spike"
    condition: "user_failed_attempts_1h > 10"
    severity: "CRITICAL"
    notification: "security-team@vibrant.org"
    auto_disable_account: true

  - name: "Cross-Patient Search"
    condition: "query_returns_patients_not_assigned > 0"
    severity: "WARNING"
    notification: "supervisor@vibrant.org"
```

## Query Examples

### Forensic Queries

```sql
-- All access to a specific patient in time range
SELECT *
FROM phi_audit_log
WHERE patient_id = 'patient_123'
  AND event_timestamp BETWEEN '2026-03-01' AND '2026-03-19'
ORDER BY event_timestamp;

-- All patients accessed by a user
SELECT DISTINCT patient_id, MIN(event_timestamp) as first_access
FROM phi_audit_log
WHERE user_id = 'counselor_789'
  AND event_timestamp > NOW() - INTERVAL '30 days'
GROUP BY patient_id;

-- Failed access attempts
SELECT user_id, COUNT(*) as failed_attempts
FROM phi_audit_log
WHERE event_type = 'PHI_ACCESS_DENIED'
  AND event_timestamp > NOW() - INTERVAL '24 hours'
GROUP BY user_id
HAVING COUNT(*) > 5;
```

## Compliance Checklist

- [ ] All PHI CRUD operations logged
- [ ] Logs include user ID, patient ID, action, timestamp
- [ ] Failed access attempts logged with reason
- [ ] Data elements accessed recorded (not just table names)
- [ ] 6-year retention configured
- [ ] Log integrity protected (tamper-evident)
- [ ] Real-time alerting for anomalies
- [ ] Quarterly access review reports
- [ ] Log backups encrypted and tested

## References

- [HIPAA Security Rule Audit Controls](https://www.hhs.gov/hipaa/for-professionals/security/guidance/audit-controls/index.html)
- [NIST SP 800-53 AU-6](https://csrc.nist.gov/projects/risk-management/sp800-53-controls/release-search#!/control?version=5.1&number=AU-6)
- [OWASP Logging Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html)
