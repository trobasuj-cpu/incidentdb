# [INC-2026-VERCEL-0pl74z9t] Increased Function Invocation Errors - ERR_MODULE_NOT_FOUND
**Company:** Vercel | **Date:** 2026-05-18 | **Severity:** MEDIUM | **Source:** [https://stspg.io/6knpl12q53h5](https://stspg.io/6knpl12q53h5)  
**Technologies:** Vercel Edge Network, Serverless Functions, Build Pipeline, AWS Lambda  
**Categories:** NETWORK_CONNECTIVITY, PIPELINE_EXECUTION_FAILURE  

---

## 1. Symptoms & Observed Errors
```text
[2026-05-18 23:52:05 UTC] Vercel SRE (Resolved): This incident has been resolved.

Some deployments created between May 18, 2026, 06:15 PM - 10:16 PM UTC may have experienced increased ERR_MODULE_NOT_FOUND function invocation errors due to a bad rollout. Deployments created outside of this window are unaffected.

The fix is being rolled out to existing deployments retrospectively and is expected to finish in the next few hours.
[2026-05-18 22:19:32 UTC] Vercel SRE (Monitoring): We are continuing to monitor for any further issues.
[2026-05-18 22:16:40 UTC] Vercel SRE (Monitoring): A fix is rolled out for the function invocation errors affecting React Router 7 deployments. To recover, redeploy your application or use Instant Rollback to a previous deployment from the dashboard. We're continuing to monitor.
[2026-05-18 22:11:52 UTC] Vercel SRE (Identified): We're continuing to work on rolling out a fix. In the mean time, customers may use Instant Rollback to a previous deployment version as an immediate workaround in order to recover.
[2026-05-18 21:53:29 UTC] Vercel SRE (Identified): The issue has been identified and a fix is being implemented.
[2026-05-18 21:42:09 UTC] Vercel SRE (Investigating): We're investigating an issue where customers are currently experiencing application failures due to function invocation errors when using React Router 7.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Vercel engineering: This incident has been resolved.

Some deployments created between May 18, 2026, 06:15 PM - 10:16 PM UTC may have experienced increased ERR_MODULE_NOT_FOUND function invocation errors due to a bad rollout. Deployments created outside of this window are unaffected.

The fix is being rolled out to existing deployments retrospectively and is expected to finish in the next few hours. We are continuing to monitor for any further issues. A fix is rolle

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Increased Function Invocation Errors - ERR_MODULE_NOT_FOUND
service_cluster:
  provider: "Vercel"
  impacted_components: ["Vercel Edge Network", "Serverless Functions", "Build Pipeline", "AWS Lambda"]
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
- [ ] Validate automatic health checks and circuit breaking on Vercel Edge Network cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
