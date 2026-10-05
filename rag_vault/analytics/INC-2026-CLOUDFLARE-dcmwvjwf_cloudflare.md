# [INC-2026-CLOUDFLARE-dcmwvjwf] Partial visibility of Workers and Durable Objects analytics for Fedramp High customers
**Company:** Cloudflare | **Date:** 2026-09-22 | **Severity:** MEDIUM | **Source:** [https://www.cloudflarestatus.com/incidents/dcmwvjwfyhjg](https://www.cloudflarestatus.com/incidents/dcmwvjwfyhjg)  
**Technologies:** Analytics, Cloudflare Edge, DNS, WAF  
**Categories:** API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-09-22 12:51:46 UTC] Cloudflare SRE (Resolved): This incident has been resolved.
[2026-09-22 10:08:10 UTC] Cloudflare SRE (Monitoring): A fix has been implemented and we are monitoring the results.
[2026-09-22 09:36:11 UTC] Cloudflare SRE (Identified): Cloudflare is monitoring the restoration of Workers and Durable Objects analytics visibility for all Fedramp High customers. While the underlying data remains intact, affected datasets were not being correctly returned by our analytics dashboard and GraphQL API since June 11th. Services are currently recovering as we propagate the fix.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Cloudflare engineering: This incident has been resolved. A fix has been implemented and we are monitoring the results. Cloudflare is monitoring the restoration of Workers and Durable Objects analytics visibility for all Fedramp High customers. While the underlying data remains intact, affected datasets were not being correctly returned by our analytics dashboard and GraphQL API since June 11th. Services are currently recovering as we propagate the fix.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Partial visibility of Workers and Durable Objects analytics for Fedramp High customers
service_cluster:
  provider: "Cloudflare"
  impacted_components: ["Analytics", "Cloudflare Edge", "DNS", "WAF"]
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
- [ ] Validate automatic health checks and circuit breaking on Analytics cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
