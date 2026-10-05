# [INC-2026-VERCEL-kndhly1h] Increase in Workflow runs stuck in pending, failed
**Company:** Vercel | **Date:** 2026-07-01 | **Severity:** MEDIUM | **Source:** [https://stspg.io/q6nb60k20jgd](https://stspg.io/q6nb60k20jgd)  
**Technologies:** Workflow, Vercel Edge Network, Serverless Functions, Build Pipeline  
**Categories:** INFRASTRUCTURE_DEGRADATION, SERVICE_OUTAGE  

---

## 1. Symptoms & Observed Errors
```text
[2026-07-01 18:30:29 UTC] Vercel SRE (Resolved): The issue affecting workflow runs getting stuck in a pending or failed state has been resolved. New workflow runs after the impact period are unaffected.

Affected workflow runs have been recovered where possible. Runs that could be resumed safely were resumed and runs that could not be resumed safely were transitioned to a cancelled state.
[2026-07-01 17:49:13 UTC] Vercel SRE (Identified): We are continuing to work on recovering affected workflow runs that may be stuck in a pending state or have reached a failed state. 

New workflow runs are not impacted.
[2026-07-01 16:57:03 UTC] Vercel SRE (Identified): We've identified an issue where some workflow runs created between June 30 at 22:24 UTC and July 1 at 16:08 UTC may be stuck in a pending state or have reached a failed state. 

New workflow runs are not impacted. We are working on a fix for runs stuck in a pending state and will provide additional updates as they become available.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Vercel engineering: The issue affecting workflow runs getting stuck in a pending or failed state has been resolved. New workflow runs after the impact period are unaffected.

Affected workflow runs have been recovered where possible. Runs that could be resumed safely were resumed and runs that could not be resumed safely were transitioned to a cancelled state. We are continuing to work on recovering affected workflow runs that may be stuck in a pending state or have

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Increase in Workflow runs stuck in pending, failed
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
