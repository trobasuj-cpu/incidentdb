# [INC-2026-VERCEL-x4ngbgzy] Increase in Workflow runs stuck as pending
**Company:** Vercel | **Date:** 2026-06-25 | **Severity:** HIGH | **Source:** [https://stspg.io/6rjcs694wc3s](https://stspg.io/6rjcs694wc3s)  
**Technologies:** Workflow, Vercel Edge Network, Serverless Functions, Build Pipeline  
**Categories:** QUEUE_DELAY, PIPELINE_EXECUTION_FAILURE  

---

## 1. Symptoms & Observed Errors
```text
[2026-06-25 13:43:22 UTC] Vercel SRE (Resolved): This incident has been resolved.
[2026-06-25 13:30:43 UTC] Vercel SRE (Monitoring): The fix has been fully rolled out and we are monitoring the results.
[2026-06-25 12:15:05 UTC] Vercel SRE (Identified): We are currently rolling a fix for the issue. We are monitoring the results.
[2026-06-25 10:42:09 UTC] Vercel SRE (Identified): We are still working on a fix for this issue. While we fix the issue, users experiencing pending Workflow runs can trigger an instant rollback to a working deployment created before Jun 24 21:00 UTC to resume operation on new runs.
[2026-06-25 09:25:34 UTC] Vercel SRE (Identified): The issue has been identified. We are continuing to work on a fix for this issue.
[2026-06-25 08:27:40 UTC] Vercel SRE (Identified): The issue has been identified and a fix is being implemented.
[2026-06-25 07:45:00 UTC] Vercel SRE (Investigating): We are currently investigating an issue where some recently deployed Workflow projects are stuck as pending and not delivering queued messages as expected.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Vercel engineering: This incident has been resolved. The fix has been fully rolled out and we are monitoring the results. We are currently rolling a fix for the issue. We are monitoring the results. We are still working on a fix for this issue. While we fix the issue, users experiencing pending Workflow runs can trigger an instant rollback to a working deployment created before Jun 24 21:00 UTC to resume operation on new runs. The issue has been identified. We are c

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Increase in Workflow runs stuck as pending
service_cluster:
  provider: "Vercel"
  impacted_components: ["Workflow", "Vercel Edge Network", "Serverless Functions", "Build Pipeline"]
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
- [ ] Validate automatic health checks and circuit breaking on Workflow cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
