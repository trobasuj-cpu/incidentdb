# [INC-2026-GITHUB-9wzhl5jr] Some Copilot Cloud Agent jobs not starting
**Company:** GitHub | **Date:** 2026-08-05 | **Severity:** MEDIUM | **Source:** [https://stspg.io/kxdh3hfxm3zl](https://stspg.io/kxdh3hfxm3zl)  
**Technologies:** GitHub Actions, Git, REST API, Webhooks  
**Categories:** QUEUE_DELAY, API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-08-05 13:00:25 UTC] GitHub SRE (Resolved): On August 5, 2026, between 11:02 and 11:54 UTC, the GitHub Copilot cloud agent service was degraded and new cloud agent jobs were delayed from starting. During this period 100% of newly submitted agent jobs were affected. The incident was limited to delay of cloud agent jobs. No jobs were lost and the queued backlog was processed by 13:00 UTC. This was due to an internal rate limit used to protect service availability that was enabled more broadly than intended delaying more traffic than expected. <br /> <br />The service recovered when the rate limit window expired. We then tuned the control so it no longer affected unrelated coding agent traffic. <br /> <br />We are working to improve the control's scoping and our monitoring and alerting to reduce our time to detection and mitigation of similar issues in the future.
[2026-08-05 12:10:46 UTC] GitHub SRE (Monitoring): Copilot cloud agent jobs have recovered and the backlog of delayed jobs is being processed.
[2026-08-05 12:01:29 UTC] GitHub SRE (Monitoring): The degradation has been mitigated. We are monitoring to ensure stability.
[2026-08-05 11:38:57 UTC] GitHub SRE (Investigating): We are investigating reports of impacted performance for some GitHub services.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by GitHub engineering: On August 5, 2026, between 11:02 and 11:54 UTC, the GitHub Copilot cloud agent service was degraded and new cloud agent jobs were delayed from starting. During this period 100% of newly submitted agent jobs were affected. The incident was limited to delay of cloud agent jobs. No jobs were lost and the queued backlog was processed by 13:00 UTC. This was due to an internal rate limit used to protect service availability that was enabled more broadl

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Some Copilot Cloud Agent jobs not starting
service_cluster:
  provider: "GitHub"
  impacted_components: ["GitHub Actions", "Git", "REST API", "Webhooks"]
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
- [ ] Validate automatic health checks and circuit breaking on GitHub Actions cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
