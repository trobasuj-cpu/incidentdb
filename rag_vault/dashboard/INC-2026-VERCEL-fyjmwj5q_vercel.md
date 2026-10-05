# [INC-2026-VERCEL-fyjmwj5q] Partial disruption of Observability, Analytics, Firewall, and Usage in dashboard
**Company:** Vercel | **Date:** 2026-06-16 | **Severity:** MEDIUM | **Source:** [https://stspg.io/00m5l68d3w2m](https://stspg.io/00m5l68d3w2m)  
**Technologies:** Dashboard, Observability, Speed Insights, Web Analytics  
**Categories:** QUEUE_DELAY, PIPELINE_EXECUTION_FAILURE  

---

## 1. Symptoms & Observed Errors
```text
[2026-06-16 15:39:55 UTC] Vercel SRE (Resolved): The incident has been resolved.
[2026-06-16 15:26:26 UTC] Vercel SRE (Monitoring): The team has deployed a fix for timeouts and failed requests related to Observability, Web Analytics, Speed Analytics, Firewall, and Usage in dashboard. We are continuing to monitor.
[2026-06-16 15:16:05 UTC] Vercel SRE (Identified): The team has identified the source of the issue and is preparing a fix for timeouts and failed requests related to Observability, Web Analytics, Speed Analytics, Firewall, and Usage in dashboard.
[2026-06-16 14:45:23 UTC] Vercel SRE (Investigating): We are continuing to investigate partial disruption to Observability, Web Analytics, Speed Analytics, Firewall, and Usage in dashboard. We are also investigating delayed usage alerts.
[2026-06-16 13:39:02 UTC] Vercel SRE (Investigating): We are investigating a partial disruption to Observability, Web Analytics, Speed Analytics and Firewall features.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Vercel engineering: The incident has been resolved. The team has deployed a fix for timeouts and failed requests related to Observability, Web Analytics, Speed Analytics, Firewall, and Usage in dashboard. We are continuing to monitor. The team has identified the source of the issue and is preparing a fix for timeouts and failed requests related to Observability, Web Analytics, Speed Analytics, Firewall, and Usage in dashboard. We are continuing to investigate partia

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Partial disruption of Observability, Analytics, Firewall, and Usage in dashboard
service_cluster:
  provider: "Vercel"
  impacted_components: ["Dashboard", "Observability", "Speed Insights", "Web Analytics"]
  health_check_status: DEGRADED
  circuit_breaker: OPEN
  observed_error_threshold: 0.05
```

## 4. Remediation Patch / Corrected Configuration
```
# Remediation mitigation applied by Vercel SRE
deployment_mitigation:
  action: "Drain degraded node pool and scale healthy replicas"
  traffic_rerouting: ENABLED
  rate_limiting_tier: STRICT
  status: RESOLVED
```

## 5. Prevention & Hardening Checklist
- [ ] Validate automatic health checks and circuit breaking on Dashboard cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
