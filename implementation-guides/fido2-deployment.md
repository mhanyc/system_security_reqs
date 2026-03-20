# FIDO2/WebAuthn Deployment Guide

Implementation guide for V6.5 Phishing-Resistant Authentication requirements.

## Overview

FIDO2/WebAuthn provides phishing-resistant authentication by using public key cryptography with credentials bound to the origin. This guide covers deployment for Vibrant's 988 Lifeline privileged accounts.

## Requirements Mapping

| Requirement | Implementation | Verification |
| :--- | :--- | :--- |
| **6.5.1** | Support FIDO2/WebAuthn | User can register security key |
| **6.5.2** | Hardware-backed credentials | Resident key on YubiKey/BioPass |
| **6.5.3** | Required for privileged accounts | Admin roles require FIDO2 |

## Architecture

```
┌──────────────────┐         ┌──────────────────┐         ┌──────────────────┐
│  User Browser    │────────▶│   WebAuthn API   │────────▶│  FIDO2 Server    │
│  (Client)        │         │   (navigator.    │         │  (Relying Party) │
│                  │         │   credentials)   │         │                  │
└──────────────────┘         └──────────────────┘         └──────────────────┘
         │                                                        │
         │                                                        │
         ▼                                                        ▼
┌──────────────────┐                                     ┌──────────────────┐
│  Security Key    │                                     │  User Database   │
│  (YubiKey/etc)   │                                     │  (Public Keys)   │
└──────────────────┘                                     └──────────────────┘
```

## Deployment Phases

### Phase 1: Pilot (Weeks 1-2)

**Scope**: InfoSec team and IT administrators

```yaml
# pilot-config.yaml
fido2:
  enabled: true
  required_for_roles: []  # Optional during pilot
  allowed_authenticators:
    - "yubikey_5_nfc"
    - "yubikey_5ci"
    - "biopass_fido2"
  excluded_authenticators:
    - "software_key"  # No software authenticators
  attestation_required: true
  resident_key_preferred: true
```

### Phase 2: Privileged Accounts (Weeks 3-4)

**Scope**: System administrators, database admins, security team

```yaml
# privileged-config.yaml
fido2:
  enabled: true
  required_for_roles:
    - "SYSTEM_ADMIN"
    - "DATABASE_ADMIN"
    - "SECURITY_TEAM"
    - "COMPLIANCE_OFFICER"
  allowed_authenticators:
    - "yubikey_5_nfc"
    - "yubikey_5ci"
    - "biopass_fido2"
  attestation_required: true
  resident_key_required: true  # Hardware-backed only
```

### Phase 3: Crisis Counselors (Weeks 5-8)

**Scope**: All crisis counselors (optional, encouraged)

```yaml
# counselor-config.yaml
fido2:
  enabled: true
  required_for_roles: []  # Optional for counselors
  encouraged_for_roles:
    - "CRISIS_COUNSELOR"
    - "SUPERVISOR"
  allowed_authenticators:
    - "yubikey_5_nfc"      # USB-C/NFC
    - "yubikey_5ci"        # Lightning/USB-C
    - "yubikey_5_nano"     # USB-A low profile
    - "biopass_fido2"      # Biometric
    - "platform_authenticator"  # Touch ID/Windows Hello (optional)
```

## Implementation

### Registration Flow

```javascript
// fido2-registration.js

async function registerFIDO2Credential(user) {
  // 1. Get challenge from server
  const options = await fetch('/api/auth/fido2/register-options', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      userId: user.id,
      userName: user.email,
      displayName: user.fullName
    })
  }).then(r => r.json());

  // 2. Create credential via WebAuthn API
  const credential = await navigator.credentials.create({
    publicKey: {
      ...options,
      challenge: base64URLToBuffer(options.challenge),
      user: {
        ...options.user,
        id: base64URLToBuffer(options.user.id)
      },
      authenticatorSelection: {
        authenticatorAttachment: "cross-platform", // Security keys
        residentKey: "required",                   // Hardware-backed
        userVerification: "required"             // PIN/biometric
      },
      attestation: "direct" // For attestation verification
    }
  });

  // 3. Send credential to server
  await fetch('/api/auth/fido2/register', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      id: credential.id,
      rawId: bufferToBase64URL(credential.rawId),
      type: credential.type,
      response: {
        clientDataJSON: bufferToBase64URL(credential.response.clientDataJSON),
        attestationObject: bufferToBase64URL(credential.response.attestationObject)
      }
    })
  });
}
```

### Authentication Flow

```javascript
// fido2-authentication.js

async function authenticateWithFIDO2(userId) {
  // 1. Get authentication options from server
  const options = await fetch('/api/auth/fido2/auth-options', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ userId })
  }).then(r => r.json());

  // 2. Get assertion from security key
  const assertion = await navigator.credentials.get({
    publicKey: {
      ...options,
      challenge: base64URLToBuffer(options.challenge),
      allowCredentials: options.allowCredentials.map(cred => ({
        ...cred,
        id: base64URLToBuffer(cred.id)
      })),
      userVerification: "required"
    }
  });

  // 3. Verify assertion on server
  const result = await fetch('/api/auth/fido2/verify', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      id: assertion.id,
      rawId: bufferToBase64URL(assertion.rawId),
      type: assertion.type,
      response: {
        authenticatorData: bufferToBase64URL(assertion.response.authenticatorData),
        clientDataJSON: bufferToBase64URL(assertion.response.clientDataJSON),
        signature: bufferToBase64URL(assertion.response.signature),
        userHandle: bufferToBase64URL(assertion.response.userHandle)
      }
    })
  });

  return result.ok;
}
```

### Server-Side Implementation (Python)

```python
# fido2_server.py
from webauthn import (
    generate_registration_options,
    verify_registration_response,
    generate_authentication_options,
    verify_authentication_response
)
from webauthn.helpers.structs import (
    AuthenticatorSelectionCriteria,
    ResidentKeyRequirement,
    UserVerificationRequirement
)

class FIDO2Service:
    def __init__(self, rp_id, rp_name, origin):
        self.rp_id = rp_id  # e.g., "crisis-line.vibrant.org"
        self.rp_name = rp_name  # e.g., "Vibrant Crisis Line"
        self.origin = origin  # e.g., "https://crisis-line.vibrant.org"

    def generate_registration_options(self, user_id: str, username: str):
        """Generate registration options for a new FIDO2 credential"""
        options = generate_registration_options(
            rp_id=self.rp_id,
            rp_name=self.rp_name,
            user_id=user_id.encode(),
            user_name=username,
            authenticator_selection=AuthenticatorSelectionCriteria(
                authenticator_attachment="cross-platform",
                resident_key=ResidentKeyRequirement.REQUIRED,
                user_verification=UserVerificationRequirement.REQUIRED
            ),
            attestation="direct"  # Verify authenticator type
        )
        return options

    def verify_registration(self, credential, expected_challenge, user_id: str):
        """Verify and store new credential"""
        verification = verify_registration_response(
            credential=credential,
            expected_challenge=expected_challenge,
            expected_origin=self.origin,
            expected_rp_id=self.rp_id,
            require_user_verification=True
        )

        # Store credential
        self.store_credential(
            user_id=user_id,
            credential_id=verification.credential_id,
            public_key=verification.credential_public_key,
            sign_count=verification.sign_count
        )

        return verification

    def generate_authentication_options(self, user_id: str):
        """Generate authentication challenge"""
        credentials = self.get_user_credentials(user_id)

        options = generate_authentication_options(
            rp_id=self.rp_id,
            allow_credentials=[
                {"type": "public-key", "id": cred.credential_id}
                for cred in credentials
            ],
            user_verification=UserVerificationRequirement.REQUIRED
        )
        return options

    def verify_authentication(self, credential, expected_challenge, user_id: str):
        """Verify authentication response"""
        stored_credential = self.get_credential(
            user_id=user_id,
            credential_id=credential.id
        )

        verification = verify_authentication_response(
            credential=credential,
            expected_challenge=expected_challenge,
            expected_origin=self.origin,
            expected_rp_id=self.rp_id,
            credential_public_key=stored_credential.public_key,
            credential_current_sign_count=stored_credential.sign_count,
            require_user_verification=True
        )

        # Update sign count to prevent replay
        self.update_sign_count(
            credential_id=credential.id,
            new_sign_count=verification.new_sign_count
        )

        return verification
```

## Recommended Hardware Keys

### For Privileged Accounts (Required)

| Device | Protocols | Resident Key | Price | Notes |
| :--- | :--- | :--- | :--- | :--- |
| YubiKey 5 NFC | FIDO2/U2F, NFC, USB-A/C | Yes | ~$50 | Best all-around |
| YubiKey 5C NFC | FIDO2/U2F, NFC, USB-C | Yes | ~$55 | Modern laptops |
| YubiKey 5Ci | FIDO2/U2F, Lightning, USB-C | Yes | ~$70 | iPhone/iPad compatible |
| YubiKey 5 Nano | FIDO2/U2F, USB-A (low profile) | Yes | ~$50 | Always-in port |
| BioPass FIDO2 | FIDO2, USB-A/C, Biometric | Yes | ~$60 | Fingerprint unlock |

### Deployment Kit

```yaml
# Standard deployment kit per privileged user
user_kit:
  primary_key: "YubiKey 5 NFC"  # USB-A workstation
  backup_key: "YubiKey 5C NFC"   # USB-C laptop
  spare_key: "YubiKey 5 Nano"    # Always-in backup
  documentation: "Quick Start Guide"

# For remote crisis counselors
remote_kit:
  primary_key: "YubiKey 5C NFC"  # USB-C (laptops)
  backup_key: "YubiKey 5Ci"      # Lightning (iOS backup)
```

## User Enrollment

### Enrollment Workflow

1. **Provision Keys**: IT distributes hardware keys
2. **Self-Service Registration**: User visits https://id.vibrant.org/fido2/setup
3. **Primary Key Registration**: User taps key, sets PIN
4. **Backup Key Registration**: User registers backup key
5. **Test Authentication**: Verify both keys work
6. **MFA Method Selection**: Set FIDO2 as primary MFA
7. **Old MFA Cleanup**: Optional: remove TOTP as primary

### Recovery Procedures

```yaml
# Lost/compromised key recovery
recovery:
  lost_key:
    steps:
      - "User reports lost key to IT Helpdesk"
      - "IT revokes lost key credential"
      - "User authenticates with backup key + password"
      - "User registers replacement key"
    sla: "4 business hours"

  compromised_key:
    steps:
      - "Security team revokes all sessions"
      - "IT revokes compromised key credential"
      - "Force password reset"
      - "User re-enrolls with new primary + backup keys"
    sla: "1 hour"
    incident_response: true
```

## Security Considerations

### Anti-Cloning Protection

```python
# Verify authenticator data for clone detection
def detect_credential_cloning(stored_sign_count, current_sign_count):
    """
    WebAuthn sign count should always increase.
    If it decreases, the key may be cloned.
    """
    if current_sign_count <= stored_sign_count:
        security_alert(
            type="POTENTIAL_CREDENTIAL_CLONE",
            stored_count=stored_sign_count,
            received_count=current_sign_count
        )
        return False  # Reject authentication
    return True
```

### AAGUID Validation

```python
ALLOWED_AAGUIDS = [
    "f8a011f3-8c0a-4d15-9446-0514deeac8d1",  # YubiKey 5 NFC
    "ee882879-721a-4910-8ff3-90c6604d4f0f",  # YubiKey 5C NFC
    "12ded745-4bed-47d4-abaa-e713f51d5a7f",  # YubiKey 5Ci
    "f9b7c18e-c983-41ce-8af9-5de3f0ad8592",  # BioPass FIDO2
]

def validate_authenticator(attestation_object):
    aaguid = extract_aaguid(attestation_object)
    if aaguid not in ALLOWED_AAGUIDS:
        raise SecurityError(f"Unauthorized authenticator: {aaguid}")
```

## Testing

### Automated Tests

```python
def test_fido2_registration():
    """Test credential registration"""
    options = fido2_service.generate_registration_options(
        user_id="user_123",
        username="counselor@vibrant.org"
    )
    assert options.authenticator_selection.resident_key == "required"


def test_fido2_authentication():
    """Test authentication flow"""
    result = fido2_service.verify_authentication(
        credential=test_credential,
        expected_challenge=test_challenge,
        user_id="user_123"
    )
    assert result.verified == True


def test_phishing_resistance():
    """Verify origin validation"""
    with pytest.raises(SecurityError) as exc:
        fido2_service.verify_authentication(
            credential=test_credential,
            expected_challenge=test_challenge,
            user_id="user_123",
            expected_origin="https://evil.com"  # Wrong origin
        )
    assert "origin" in str(exc.value)
```

## Rollback Plan

If FIDO2 deployment causes issues:

1. **Disable FIDO2 Requirement**: Revert to TOTP for affected roles
2. **Hybrid Mode**: Allow either FIDO2 OR TOTP temporarily
3. **Emergency Access**: Use break-glass for locked-out admins
4. **User Communication**: Email all affected users with instructions

## Compliance Notes

- **NIST 800-63B AAL3**: FIDO2 with hardware authenticator = AAL3
- **CISA MFA Guidance**: FIDO2 is "phishing-resistant MFA"
- **HIPAA**: FIDO2 meets "something you have" requirement for ePHI access

## References

- [WebAuthn Specification](https://www.w3.org/TR/webauthn-2/)
- [NIST SP 800-63B](https://pages.nist.gov/800-63-3/sp800-63b.html)
- [CISA Phishing-Resistant MFA](https://www.cisa.gov/secure-our-world/turn-mfa)
- [YubiKey Developers](https://developers.yubico.com/)
