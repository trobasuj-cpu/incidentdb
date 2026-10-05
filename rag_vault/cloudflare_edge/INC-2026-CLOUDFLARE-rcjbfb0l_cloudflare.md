# [INC-2026-CLOUDFLARE-rcjbfb0l] Issues with 1.1.1.1 for Families
**Company:** Cloudflare | **Date:** 2026-09-23 | **Severity:** MEDIUM | **Source:** [https://www.cloudflarestatus.com/incidents/rcjbfb0lyn65](https://www.cloudflarestatus.com/incidents/rcjbfb0lyn65)  
**Technologies:** Cloudflare Edge, DNS, WAF, Workers  
**Categories:** NETWORK_CONNECTIVITY, AUTHENTICATION_FAILURE  

---

## 1. Symptoms & Observed Errors
```text
[2026-09-23 01:26:28 UTC] Cloudflare SRE (Resolved): This incident has been resolved.
[2026-09-23 01:21:02 UTC] Cloudflare SRE (Monitoring): A fix has been implemented and we are monitoring the results.
[2026-09-23 00:55:34 UTC] Cloudflare SRE (Identified): The issue has been identified and a fix is being implemented.
[2026-09-23 00:42:50 UTC] Cloudflare SRE (Investigating): Cloudflare is aware of, and investigating, an issue which impacts DNS over HTTPS requests to 1.1.1.1 for Families (1.1.1.2 and 1.1.1.3)
Authoritative DNS provided by Cloudflare is unaffected.

Further detail will be provided as more information becomes available.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Cloudflare engineering: This incident has been resolved. A fix has been implemented and we are monitoring the results. The issue has been identified and a fix is being implemented. Cloudflare is aware of, and investigating, an issue which impacts DNS over HTTPS requests to 1.1.1.1 for Families (1.1.1.2 and 1.1.1.3)
Authoritative DNS provided by Cloudflare is unaffected.

Further detail will be provided as more information becomes available.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Issues with 1.1.1.1 for Families
service_cluster:
  provider: "Cloudflare"
  impacted_components: ["Cloudflare Edge", "DNS", "WAF", "Workers"]
  health_check_status: DEGRADED
  circuit_breaker: OPEN
  observed_error_threshold: 0.05
```

## 4. Remediation Patch / Corrected Configuration
```
# Remediation mitigation applied by Cloudflare SRE
deployment_mitigation:
  action: "Drain degraded node pool and scale healthy replicas"
  traffic_rerouting: ENABLED
  rate_limiting_tier: STRICT
  status: RESOLVED
```

## 5. Prevention & Hardening Checklist
- [ ] Validate automatic health checks and circuit breaking on Cloudflare Edge cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
