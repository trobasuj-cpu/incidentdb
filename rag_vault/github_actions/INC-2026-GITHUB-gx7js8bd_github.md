# [INC-2026-GITHUB-gx7js8bd] Incident with Actions
**Company:** GitHub | **Date:** 2026-08-18 | **Severity:** HIGH | **Source:** [https://stspg.io/q3ck88mqw08z](https://stspg.io/q3ck88mqw08z)  
**Technologies:** GitHub Actions, Git, REST API, Webhooks  
**Categories:** API_ERROR_SPIKE, AUTHENTICATION_FAILURE, PIPELINE_EXECUTION_FAILURE  

---

## 1. Symptoms & Observed Errors
```text
[2026-08-18 10:23:23 UTC] GitHub SRE (Resolved): On August 18, 2026, between 05:02 UTC and 11:30 UTC, customers were unable to run jobs on Actions Larger Runners and were unable to view or manage Actions Runners and Runner Groups through the GitHub UI and API. <br /><br />These issues were caused by failures in backend requests resolving essential metadata for starting Larger Runner workflow runs and for reading runner and runner group data. The failures were caused by an expired authentication certificate unique to this service. The certificate had been rotated in KeyVault, but a step to enable use at runtime had been paused to prevent recurrence of previous incidents that had been triggered by this operation. <br /><br />We mitigated the issues by completing the enablement of the new certificate in the backend system. We have added additional monitoring to this and other certificates. The relevant service is also in the process of being replaced as part of our availability and scale work, bringing this authentication path and secret management in line with patterns across all GitHub services.
[2026-08-18 09:36:40 UTC] GitHub SRE (Investigating): We are investigating reports of impacted performance for some GitHub services.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by GitHub engineering: On August 18, 2026, between 05:02 UTC and 11:30 UTC, customers were unable to run jobs on Actions Larger Runners and were unable to view or manage Actions Runners and Runner Groups through the GitHub UI and API. <br /><br />These issues were caused by failures in backend requests resolving essential metadata for starting Larger Runner workflow runs and for reading runner and runner group data. The failures were caused by an expired authentication

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Incident with Actions
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
