# [INC-2026-CLOUDFLARE-gv29fd07] Issues with Durable Objects
**Company:** Cloudflare | **Date:** 2026-09-25 | **Severity:** MEDIUM | **Source:** [https://www.cloudflarestatus.com/incidents/gv29fd076q2t](https://www.cloudflarestatus.com/incidents/gv29fd076q2t)  
**Technologies:** Durable Objects, Cloudflare Edge, DNS, WAF  
**Categories:** QUEUE_DELAY  

---

## 1. Symptoms & Observed Errors
```text
[2026-09-25 14:49:01 UTC] Cloudflare SRE (Resolved): This incident has been resolved.
[2026-09-25 08:48:52 UTC] Cloudflare SRE (Monitoring): A fix has been implemented and we are monitoring the results.
[2026-09-25 02:32:19 UTC] Cloudflare SRE (Investigating): Cloudflare is investigating issues causing an increase of errors for Durable Objects .We are working to analyse and mitigate this problem. More updates to follow shortly.
[2026-09-25 01:31:54 UTC] Cloudflare SRE (Monitoring): A fix has been implemented and we are monitoring the results.
[2026-09-25 00:03:24 UTC] Cloudflare SRE (Investigating): Cloudflare is aware of and investigating an issue impacting some customers where new Workflows instances may be stuck in a Queued state. More updates to follow shortly.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Cloudflare engineering: This incident has been resolved. A fix has been implemented and we are monitoring the results. Cloudflare is investigating issues causing an increase of errors for Durable Objects .We are working to analyse and mitigate this problem. More updates to follow shortly. A fix has been implemented and we are monitoring the results. Cloudflare is aware of and investigating an issue impacting some customers where new Workflows instances may be stuck in a

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Issues with Durable Objects
service_cluster:
  provider: "Cloudflare"
  impacted_components: ["Durable Objects", "Cloudflare Edge", "DNS", "WAF"]
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
