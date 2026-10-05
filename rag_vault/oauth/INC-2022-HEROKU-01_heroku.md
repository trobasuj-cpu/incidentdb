# [INC-2022-HEROKU-01] Exposure of Customer Source Repositories via Compromised GitHub Integration OAuth Token
**Company:** Heroku | **Date:** 2022-04-16 | **Severity:** HIGH  
**Technologies:** OAuth, PostgreSQL, GitHub API, Ruby  
**Categories:** SECURITY_BREACH, SECRET_EXFILTRATION, OAUTH_ABUSE  

---

## 1. Symptoms & Observed Errors
```text
GitHub Security Advisory: OAuth access token assigned to Heroku accessed external customer repositories
Security Audit: Internal database dump extracted from compromised developer credentials
Heroku suspended GitHub automated deployment integrations globally.
```

## 2. Root Cause Analysis
A compromised machine belonging to an internal Heroku engineer was used to extract high-privilege credentials storing customer OAuth tokens for Heroku's GitHub Integration. The attackers used these stolen OAuth tokens to clone private source code repositories from dozens of enterprise customers using Heroku pipelines.

## 3. Breaking Configuration / Problematic Code
```
# Centralized database storing unencrypted long-lived customer OAuth access tokens:
SELECT customer_id, github_oauth_token, refresh_token FROM customer_integrations;
# Tokens stored at rest without envelope encryption or scope bounding
```

## 4. Remediation Patch / Corrected Configuration
```
# Migrated to fine-grained short-lived GitHub App tokens with envelope KMS encryption
def store_oauth_token(customer_id: str, raw_token: str) -> None:
    encrypted_blob = aws_kms_encrypt(key_id="alias/customer_tokens", plaintext=raw_token)
    db.execute("INSERT INTO tokens (customer_id, ciphertext) VALUES (%s, %s)", (customer_id, encrypted_blob))
```

## 5. Prevention & Hardening Checklist
- [ ] Store all third-party access tokens with envelope encryption using unique customer encryption keys
- [ ] Transition integrations from coarse-grained OAuth user tokens to fine-grained, short-lived GitHub Apps tokens
- [ ] Implement automated anomaly alerts on unusual bulk access to token storage repositories
