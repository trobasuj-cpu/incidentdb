# [INC-2023-CIRCLECI-01] Customer Environment Secret Exposure via Compromised Engineer SSO Session Cookie
**Company:** CircleCI | **Date:** 2023-01-04 | **Severity:** CRITICAL  
**Technologies:** AWS, HashiCorp Vault, Docker, SSO  
**Categories:** SECURITY_BREACH, SECRET_EXFILTRATION, SESSION_HIJACKING  

---

## 1. Symptoms & Observed Errors
```text
AWS GuardDuty: UnauthorizedAccess:IAMUser/InstanceCredentialExfiltration
Malware detection: Trojan.Stealer on developer workstation
Vault API: Elevated read access to customer project variables from unrecognized IP range 194.26.29.11
Customers notified to rotate all AWS keys, GitHub tokens, and deployment secrets.
```

## 2. Root Cause Analysis
An engineer's personal computer was infected with infostealer malware, which harvested active browser session cookies. The exfiltrated session token allowed threat actors to impersonate the engineer and bypass multi-factor authentication (MFA). The attacker accessed production database read-replicas and encrypted customer secret stores, extracting customer environment variables, OAuth tokens, and webhook secrets.

## 3. Breaking Configuration / Problematic Code
```
# Permissive SSO session configuration with indefinite cookie validity:
session_timeout_seconds: 604800  # 7 days without re-authentication
ip_binding_enforced: false
hardware_token_required_for_vault_read: false
```

## 4. Remediation Patch / Corrected Configuration
```
# Hardened zero-trust session management and device posture verification
session_timeout_seconds: 28800   # 8 hours maximum
ip_binding_enforced: true        # Invalidate session if IP subnet changes
require_hardware_fido2_mfa: true # Phishing-resistant FIDO2 WebAuthn keys required for any database decrypt
```

## 5. Prevention & Hardening Checklist
- [ ] Enforce hardware-bound FIDO2 WebAuthn keys for all production and internal administrative access
- [ ] Bind session cookies to device posture checks and client IP ranges to prevent stolen cookie replay
- [ ] Encrypt customer secrets using customer-managed AWS KMS keys (Envelope Encryption) so platform engineers cannot decrypt
