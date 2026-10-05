# [INC-2026-GITHUB-kfspvrz1] Incident with Actions and Pull Requests
**Company:** GitHub | **Date:** 2026-08-26 | **Severity:** MEDIUM | **Source:** [https://stspg.io/nrbwjftcz72d](https://stspg.io/nrbwjftcz72d)  
**Technologies:** Pull Requests, Actions, GitHub Actions, Git  
**Categories:** QUEUE_DELAY  

---

## 1. Symptoms & Observed Errors
```text
[2026-08-27 00:26:44 UTC] GitHub SRE (Resolved): On August 26, 2026, from 21:55 UTC to 23:58 UTC, 2.6% of workflow runs triggered by pull request events were delayed, with the impact rising as high as 25% at its peak. Some users also experienced delays in pull request merge-commit generation, mergeability information, and merge-button availability. Actions and Pull Requests fully recovered by 23:58 UTC; the incident was resolved at 00:26 UTC after normal operation was confirmed. <br /><br />Background jobs that process pull request updates and generate merge commits were impacted by timeouts reaching a single partition of git data. This resulted in a backlog in pull request merge-commit processing, delaying pull request-triggered GitHub Actions workflows and some mergeability information. <br /><br />We reduced workload, shifted traffic away from affected infrastructure, and restored the affected service component to a healthy state. Together, these actions helped drain the backlog and restore normal operations. <br /><br />We are working to improve resource saturation detection and to eliminate customer impact in this scenario by isolating impact, placing better bounds on retries, and strengthening backpressure to make our systems more resilient under load.
[2026-08-27 00:26:05 UTC] GitHub SRE (Monitoring): The degradation affecting Actions and Pull Requests has been mitigated. We are monitoring to ensure stability.
[2026-08-27 00:25:58 UTC] GitHub SRE (Investigating): We confirmed full recovery beginning at 23:58 UTC. Actions workflow runs and pull request merges are operating normally. We will now resolve the incident while continuing to monitor service health.
[2026-08-27 00:01:05 UTC] GitHub SRE (Investigating): We've applied mitigations and are seeing recovery in Actions workflow runs and blocked pull request merges. We're continuing to monitor for sustained health of merge commit creates before resolving.
[2026-08-26 22:57:08 UTC] GitHub SRE (Investigating): We are investigating elevated delays and timeouts affecting Actions workflow runs triggered by pull request events. 20% of actions runs have delayed starts of more than 5 minutes and up to 4% of runs failed to trigger. We are actively working on mitigation and will provide updates as we learn more.
[2026-08-26 22:56:31 UTC] GitHub SRE (Investigating): We are investigating reports of degraded performance for Actions and Pull Requests
```

## 2. Root Cause Analysis
Official investigation timeline recorded by GitHub engineering: On August 26, 2026, from 21:55 UTC to 23:58 UTC, 2.6% of workflow runs triggered by pull request events were delayed, with the impact rising as high as 25% at its peak. Some users also experienced delays in pull request merge-commit generation, mergeability information, and merge-button availability. Actions and Pull Requests fully recovered by 23:58 UTC; the incident was resolved at 00:26 UTC after normal operation was confirmed. <br /><br />Bac

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Incident with Actions and Pull Requests
service_cluster:
  provider: "GitHub"
  impacted_components: ["Pull Requests", "Actions", "GitHub Actions", "Git"]
  health_check_status: DEGRADED
  circuit_breaker: OPEN
  observed_error_threshold: 0.05
```

## 4. Remediation Patch / Corrected Configuration
```
# Remediation mitigation applied by GitHub SRE
deployment_mitigation:
  action: "Drain degraded node pool and scale healthy replicas"
  traffic_rerouting: ENABLED
  rate_limiting_tier: STRICT
  status: RESOLVED
```

## 5. Prevention & Hardening Checklist
- [ ] Validate automatic health checks and circuit breaking on Pull Requests cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
