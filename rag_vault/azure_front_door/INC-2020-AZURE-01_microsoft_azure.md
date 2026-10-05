# [INC-2020-AZURE-01] Front Door Global Routing Outage via Expired Internal TLS Management Certificate
**Company:** Microsoft Azure | **Date:** 2020-03-03 | **Severity:** CRITICAL  
**Technologies:** Azure Front Door, TLS, PKI, Edge  
**Categories:** CERTIFICATE_EXPIRATION, EDGE_INGRESS, AUTOMATION_DEFECT  

---

## 1. Symptoms & Observed Errors
```text
SSL_ERROR_EXPIRED_CERT_ALERT
Sec_Error_Expired_Issuer_Certificate
Failed to establish TLS handshake with edge proxy: certificate expired 2020-03-03 08:30:00 UTC
HTTP 503 across Azure Portal, Teams, and downstream customer services.
```

## 2. Root Cause Analysis
An internal automated service responsible for rotating SSL/TLS certificates across Azure Front Door edge nodes encountered a silent permissions error during key renewal. Because monitoring scripts only alerted when certificates were missing rather than when expiration timestamps approached zero, the management certificate lapsed, invalidating secure communication between edge reverse proxies and core routing services.

## 3. Breaking Configuration / Problematic Code
```
# Inadequate certificate health monitor checking presence only:
def check_cert_health(cert_path: str) -> bool:
    # Defect: Only checks file existence, ignoring validity timestamps!
    return os.path.exists(cert_path) and os.path.getsize(cert_path) > 0
```

## 4. Remediation Patch / Corrected Configuration
```
# Hardened certificate monitor evaluating cryptographic expiration date
def check_cert_health(cert_bytes: bytes, threshold_days: int = 30) -> bool:
    cert = x509.load_pem_x509_certificate(cert_bytes)
    remaining_days = (cert.not_valid_after - datetime.utcnow()).days
    if remaining_days <= threshold_days:
        alert_oncall_engineer(f"Certificate expires in {remaining_days} days!")
        return False
    return True
```

## 5. Prevention & Hardening Checklist
- [ ] Implement proactive alerts alerting on certificate expiration starting at 45, 30, and 14 days before deadline
- [ ] Automate end-to-end synthetic TLS handshakes against all edge domains every 60 seconds
- [ ] Build automated fallback to secondary root CA trust chains to prevent hard failure on single cert lapse
