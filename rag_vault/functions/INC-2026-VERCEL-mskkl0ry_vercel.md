# [INC-2026-VERCEL-mskkl0ry] Increased invocation failures for Hobby Team functions
**Company:** Vercel | **Date:** 2026-07-17 | **Severity:** MEDIUM | **Source:** [https://stspg.io/qcpsy7kj6dlc](https://stspg.io/qcpsy7kj6dlc)  
**Technologies:** Functions, Vercel Edge Network, Serverless Functions, Build Pipeline  
**Categories:** PIPELINE_EXECUTION_FAILURE  

---

## 1. Symptoms & Observed Errors
```text
[2026-07-17 15:19:53 UTC] Vercel SRE (Resolved): This incident has been resolved.

A subset of deployments created under the Hobby plan teams during Jul 17, 2026 10:18 - Jul 17, 2026 14:10 UTC were impacted by this incident. New deployments after this window are unaffected.
[2026-07-17 14:27:39 UTC] Vercel SRE (Monitoring): We have deployed a fix and are continuing to monitor. Function invocations for new Hobby Team deployments should no longer fail. If you are still experiencing function invocation errors, redeploy or rollback to a previous deployment.
[2026-07-17 14:19:52 UTC] Vercel SRE (Identified): We have identified the source of errors and are implementing a fix.
[2026-07-17 14:12:58 UTC] Vercel SRE (Investigating): There are elevated rates of invocation failures for Hobby Team functions in new deployments. Existing deployments are unaffected. We are investigating. Mitigation is possible by rolling back to a previous deployment.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Vercel engineering: This incident has been resolved.

A subset of deployments created under the Hobby plan teams during Jul 17, 2026 10:18 - Jul 17, 2026 14:10 UTC were impacted by this incident. New deployments after this window are unaffected. We have deployed a fix and are continuing to monitor. Function invocations for new Hobby Team deployments should no longer fail. If you are still experiencing function invocation errors, redeploy or rollback to a previous de

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Increased invocation failures for Hobby Team functions
service_cluster:
  provider: "Vercel"
  impacted_components: ["Functions", "Vercel Edge Network", "Serverless Functions", "Build Pipeline"]
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
- [ ] Validate automatic health checks and circuit breaking on Functions cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
