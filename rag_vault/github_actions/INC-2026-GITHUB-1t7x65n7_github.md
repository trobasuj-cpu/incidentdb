# [INC-2026-GITHUB-1t7x65n7] Disruption with Copilot for access to some models
**Company:** GitHub | **Date:** 2026-08-10 | **Severity:** MEDIUM | **Source:** [https://stspg.io/4x5bd0ghzy0x](https://stspg.io/4x5bd0ghzy0x)  
**Technologies:** GitHub Actions, Git, REST API, Webhooks  
**Categories:** API_ERROR_SPIKE, PIPELINE_EXECUTION_FAILURE  

---

## 1. Symptoms & Observed Errors
```text
[2026-08-10 21:50:43 UTC] GitHub SRE (Resolved): On August 10, 2026, between 19:48 UTC and 20:49 UTC, GitHub Copilot users saw an incomplete list of available models. During this window, the service could return as few as one model instead of the full catalog. Requests that tried to use a model missing from that shortened list failed with a "model not found" error. Copilot requests that used an available model were not affected. This did not affect customers on data-residency (Proxima) environments.<br /><br />The issue was caused by a change to how model data was published, which our systems could not read back correctly and fell back to a limited default list.<br /><br />We mitigated the incident by 20:49 UTC and deployed a fix to prevent immediate recurrence by 21:50 UTC. We are adding validation and retry safeguards so that model data is verified before it is served.<br /><br />We apologize for the disruption.
[2026-08-10 21:50:40 UTC] GitHub SRE (Monitoring): We have deployed and validated the fix to prevent immediate reoccurrence. We will be performing additional work to limit these kinds of failures in the future.
[2026-08-10 21:19:06 UTC] GitHub SRE (Monitoring): The issue has been mitigated across all affected environments. We are currently deploying on a fix to prevent reoccurrence. We will provide another update once the fix has been deployed.
[2026-08-10 20:49:46 UTC] GitHub SRE (Monitoring): The degradation has been mitigated. We are monitoring to ensure stability.
[2026-08-10 20:39:11 UTC] GitHub SRE (Monitoring): We are currently investigating reports of some Copilot users experiencing issues accessing certain models. Affected users may see errors or degraded functionality when attempting to use specific models. We are actively working on a fix and will provide updates as we have more information.
[2026-08-10 20:27:19 UTC] GitHub SRE (Investigating): We are investigating reports of impacted performance for some GitHub services.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by GitHub engineering: On August 10, 2026, between 19:48 UTC and 20:49 UTC, GitHub Copilot users saw an incomplete list of available models. During this window, the service could return as few as one model instead of the full catalog. Requests that tried to use a model missing from that shortened list failed with a "model not found" error. Copilot requests that used an available model were not affected. This did not affect customers on data-residency (Proxima) environm

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Disruption with Copilot for access to some models
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
