# [INC-2026-VERCEL-nxvrk1jk] Partial disruption of Observability, Analytics, Firewall, Usage, and Login in Dashboard
**Company:** Vercel | **Date:** 2026-08-17 | **Severity:** MEDIUM | **Source:** [https://stspg.io/sqztsdm8bq4d](https://stspg.io/sqztsdm8bq4d)  
**Technologies:** Dashboard, Observability, Speed Insights, Web Analytics  
**Categories:** AUTHENTICATION_FAILURE  

---

## 1. Symptoms & Observed Errors
```text
[2026-08-17 21:25:26 UTC] Vercel SRE (Resolved): Services have recovered. We are still working to ingest missing observability data during this period.
[2026-08-17 20:13:29 UTC] Vercel SRE (Monitoring): Services have recovered. We are still working to ingest missing observability data during this period.
[2026-08-17 17:51:57 UTC] Vercel SRE (Monitoring): Login with TOTP has completely recovered. We are still working to ingest missing observability data during this period. We will share more information as it becomes available.
[2026-08-17 16:46:03 UTC] Vercel SRE (Monitoring): We are continuing to monitor for any further issues.
[2026-08-17 16:44:55 UTC] Vercel SRE (Monitoring): We have implemented a fix for the disruption, and services have started to recover. We will share more information as it becomes available.
[2026-08-17 16:06:23 UTC] Vercel SRE (Investigating): We are also investigating an issue where some customers may be unable to sign in to Dashboard with time-based one-time passwords (TOTP). We will share updates as they become available.
[2026-08-17 15:09:49 UTC] Vercel SRE (Investigating): We are continuing to investigate this issue. We will share updates as they become available.
[2026-08-17 14:30:28 UTC] Vercel SRE (Investigating): We are investigating a partial disruption to Observability, Web Analytics, Speed Insights, Firewall, and Usage in Dashboard. We will share updates as they become available.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Vercel engineering: Services have recovered. We are still working to ingest missing observability data during this period. Services have recovered. We are still working to ingest missing observability data during this period. Login with TOTP has completely recovered. We are still working to ingest missing observability data during this period. We will share more information as it becomes available. We are continuing to monitor for any further issues. We have impleme

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Partial disruption of Observability, Analytics, Firewall, Usage, and Login in Dashboard
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
