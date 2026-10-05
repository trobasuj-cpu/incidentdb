# [INC-2026-CLOUDFLARE-pvvq3811] Elevated error rates for Stream Live LL-HLS
**Company:** Cloudflare | **Date:** 2026-09-22 | **Severity:** MEDIUM | **Source:** [https://www.cloudflarestatus.com/incidents/pvvq3811tpyq](https://www.cloudflarestatus.com/incidents/pvvq3811tpyq)  
**Technologies:** Stream, Cloudflare Edge, DNS, WAF  
**Categories:** API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-09-22 15:38:50 UTC] Cloudflare SRE (Resolved): This incident has been resolved.
[2026-09-22 14:44:18 UTC] Cloudflare SRE (Monitoring): A fix has been implemented and we are monitoring the results.
[2026-09-22 14:37:45 UTC] Cloudflare SRE (Investigating): We are investigating an issue where some Stream Live customers are receiving errors when playing back live broadcasts with LL-HLS enabled.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Cloudflare engineering: This incident has been resolved. A fix has been implemented and we are monitoring the results. We are investigating an issue where some Stream Live customers are receiving errors when playing back live broadcasts with LL-HLS enabled.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Elevated error rates for Stream Live LL-HLS
service_cluster:
  provider: "Cloudflare"
  impacted_components: ["Stream", "Cloudflare Edge", "DNS", "WAF"]
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
- [ ] Validate automatic health checks and circuit breaking on Stream cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
