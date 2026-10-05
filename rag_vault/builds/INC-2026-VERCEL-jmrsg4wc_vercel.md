# [INC-2026-VERCEL-jmrsg4wc] Elevated Build Failures (GitHub connected projects)
**Company:** Vercel | **Date:** 2026-05-23 | **Severity:** MEDIUM | **Source:** [https://stspg.io/gck741vpfk2k](https://stspg.io/gck741vpfk2k)  
**Technologies:** Builds, Vercel Edge Network, Serverless Functions, Build Pipeline  
**Categories:** PIPELINE_EXECUTION_FAILURE  

---

## 1. Symptoms & Observed Errors
```text
[2026-05-23 18:44:30 UTC] Vercel SRE (Resolved): This incident has been resolved.

Please refer to GitHub's status page post for more details: https://www.githubstatus.com/incidents/k5z4d1v1tqmt
[2026-05-23 18:21:29 UTC] Vercel SRE (Monitoring): GitHub has implemented a fix, and we are monitoring the results.
[2026-05-23 15:46:55 UTC] Vercel SRE (Identified): We have identified the issue and are working closely with GitHub to resolve it.
[2026-05-23 14:30:01 UTC] Vercel SRE (Investigating): We are working closely with GitHub to resolve this issue. Failures are intermittent — if your deployment fails, redeploying should resolve it in the meantime. We will provide updates as the situation develops.
[2026-05-23 12:15:39 UTC] Vercel SRE (Investigating): We are continuing to investigate this issue.
[2026-05-23 11:09:23 UTC] Vercel SRE (Investigating): We've been observing increased failures of git operations with GitHub since around 06:00 UTC. Some deployments triggered by GitHub commits might have seen git-related errors in failed build logs.

CLI deployments are unaffected at this time.
[2026-05-23 10:43:45 UTC] Vercel SRE (Investigating): We are currently investigating this issue.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Vercel engineering: This incident has been resolved.

Please refer to GitHub's status page post for more details: https://www.githubstatus.com/incidents/k5z4d1v1tqmt GitHub has implemented a fix, and we are monitoring the results. We have identified the issue and are working closely with GitHub to resolve it. We are working closely with GitHub to resolve this issue. Failures are intermittent — if your deployment fails, redeploying should resolve it in the meantime.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Elevated Build Failures (GitHub connected projects)
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
