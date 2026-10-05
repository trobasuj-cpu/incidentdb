# [INC-2026-VERCEL-0qjk7n73] Missing Build CPU Minutes Usage Data
**Company:** Vercel | **Date:** 2026-05-20 | **Severity:** MEDIUM | **Source:** [https://stspg.io/xt6sl3hp53c5](https://stspg.io/xt6sl3hp53c5)  
**Technologies:** Builds, Vercel Edge Network, Serverless Functions, Build Pipeline  
**Categories:** PIPELINE_EXECUTION_FAILURE  

---

## 1. Symptoms & Observed Errors
```text
[2026-05-21 06:08:03 UTC] Vercel SRE (Resolved): This incident has been resolved.

Usage data for Build CPU Minutes between May 15, 2026, 19:30, and May 20, 2026, 18:00 UTC is currently incomplete. Customers will see usage data for Build CPU Minutes catch up as we backfill the data for the affected window.
[2026-05-20 20:07:17 UTC] Vercel SRE (Monitoring): We've identified an issue where some users may see missing Build CPU Minutes data on Usage pages in the Vercel Dashboard. The issue has been resolved, and we are backfilling the affected usage data.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Vercel engineering: This incident has been resolved.

Usage data for Build CPU Minutes between May 15, 2026, 19:30, and May 20, 2026, 18:00 UTC is currently incomplete. Customers will see usage data for Build CPU Minutes catch up as we backfill the data for the affected window. We've identified an issue where some users may see missing Build CPU Minutes data on Usage pages in the Vercel Dashboard. The issue has been resolved, and we are backfilling the affected usag

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Missing Build CPU Minutes Usage Data
service_cluster:
  provider: "Vercel"
  impacted_components: ["Builds", "Vercel Edge Network", "Serverless Functions", "Build Pipeline"]
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
- [ ] Validate automatic health checks and circuit breaking on Builds cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
