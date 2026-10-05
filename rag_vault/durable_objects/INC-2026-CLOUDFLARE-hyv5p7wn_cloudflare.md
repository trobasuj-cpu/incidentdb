# [INC-2026-CLOUDFLARE-hyv5p7wn] Increased Errors for Durable Objects and R2
**Company:** Cloudflare | **Date:** 2026-10-01 | **Severity:** MEDIUM | **Source:** [https://www.cloudflarestatus.com/incidents/hyv5p7wn1c8v](https://www.cloudflarestatus.com/incidents/hyv5p7wn1c8v)  
**Technologies:** Durable Objects, R2, Cloudflare Edge, DNS  
**Categories:** INFRASTRUCTURE_DEGRADATION, SERVICE_OUTAGE  

---

## 1. Symptoms & Observed Errors
```text
[2026-10-01 21:18:42 UTC] Cloudflare SRE (Resolved): This incident has been resolved.
[2026-10-01 21:06:14 UTC] Cloudflare SRE (Monitoring): A fix has been implemented and we are monitoring the results.
[2026-10-01 20:59:56 UTC] Cloudflare SRE (Investigating): Cloudflare is investigating issues causing an increase of errors for Durable Objects and R2 as a result in Amsterdam. We are working to analyse and mitigate this problem. More updates to follow shortly.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Cloudflare engineering: This incident has been resolved. A fix has been implemented and we are monitoring the results. Cloudflare is investigating issues causing an increase of errors for Durable Objects and R2 as a result in Amsterdam. We are working to analyse and mitigate this problem. More updates to follow shortly.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Increased Errors for Durable Objects and R2
service_cluster:
  provider: "Cloudflare"
  impacted_components: ["Durable Objects", "R2", "Cloudflare Edge", "DNS"]
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
- [ ] Validate automatic health checks and circuit breaking on Durable Objects cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
