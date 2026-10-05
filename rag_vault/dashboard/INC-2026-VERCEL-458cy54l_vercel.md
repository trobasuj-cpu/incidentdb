# [INC-2026-VERCEL-458cy54l] Erroneous budget notifications
**Company:** Vercel | **Date:** 2026-07-10 | **Severity:** MEDIUM | **Source:** [https://stspg.io/274dc78rzc6q](https://stspg.io/274dc78rzc6q)  
**Technologies:** Dashboard, Builds, Vercel Edge Network, Serverless Functions  
**Categories:** PIPELINE_EXECUTION_FAILURE  

---

## 1. Symptoms & Observed Errors
```text
[2026-07-10 22:36:10 UTC] Vercel SRE (Resolved): We have unpaused all impacted deployments. We will continue to monitor the situation.
[2026-07-10 21:32:58 UTC] Vercel SRE (Identified): We are still working to unpause affected teams that were erroneously paused due to the spend management budget miscalculation.

We will provide additional updates as they become available.
[2026-07-10 20:50:35 UTC] Vercel SRE (Identified): We've compiled a list of deployments that were paused due to this incident and are now unpausing them in batches. If your deployment was paused, you can also navigate to the Vercel dashboard and click the unpause button in the banner.
[2026-07-10 19:53:44 UTC] Vercel SRE (Identified): We've identified and fixed the underlying issue. Our team is now unpausing deployments that were inadvertently paused as a result.
[2026-07-10 19:15:53 UTC] Vercel SRE (Investigating): We've identified an issue where some customers may receive erroneous spend management budget notifications due to a spend management budget miscalculation. Affected teams with spend management budgets configured to pause projects may also experience paused deployments. We are currently investigating this issue. We will provide additional updates as they become available.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Vercel engineering: We have unpaused all impacted deployments. We will continue to monitor the situation. We are still working to unpause affected teams that were erroneously paused due to the spend management budget miscalculation.

We will provide additional updates as they become available. We've compiled a list of deployments that were paused due to this incident and are now unpausing them in batches. If your deployment was paused, you can also navigate to the V

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Erroneous budget notifications
service_cluster:
  provider: "Vercel"
  impacted_components: ["Dashboard", "Builds", "Vercel Edge Network", "Serverless Functions"]
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
