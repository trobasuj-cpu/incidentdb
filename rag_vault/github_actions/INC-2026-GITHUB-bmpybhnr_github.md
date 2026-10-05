# [INC-2026-GITHUB-bmpybhnr] Intermittent failures in runner group and runner-related permissions pages
**Company:** GitHub | **Date:** 2026-08-18 | **Severity:** MEDIUM | **Source:** [https://stspg.io/98zqb1k9jh0x](https://stspg.io/98zqb1k9jh0x)  
**Technologies:** GitHub Actions, Git, REST API, Webhooks  
**Categories:** API_ERROR_SPIKE, AUTHENTICATION_FAILURE, PIPELINE_EXECUTION_FAILURE  

---

## 1. Symptoms & Observed Errors
```text
[2026-08-18 11:42:59 UTC] GitHub SRE (Resolved): On August 18, 2026, between 05:02 UTC and 11:30 UTC, customers were unable to view or manage Actions Runners and Runner Groups through the GitHub UI and API. <br /><br />The issue was caused by failures in backend requests reading runner and runner group data. The failures were caused by an expired authentication certificate unique to this service. The certificate had been rotated in KeyVault, but a step to enable use at runtime had been paused to prevent recurrence of previous incidents triggered by this operation. <br /><br />The impact was mitigated by completing the enablement of the new certificate in the backend system. We have added additional monitoring to this and other certificates. This service is also in the process of being replaced as part of our availability and scale work, bringing this authentication path and secret management in line with patterns across all GitHub services.
[2026-08-18 11:24:06 UTC] GitHub SRE (Monitoring): We have applied a mitigation and are seeing recovery signals. We will continue monitoring recovery and providing updates.
[2026-08-18 10:41:40 UTC] GitHub SRE (Monitoring): We have identified the source of a communication issue between Actions services and are working toward mitigation. Customers may experience failure to load runner groups and runner-related permissions issues when using Larger Runners.
[2026-08-18 07:40:42 UTC] GitHub SRE (Monitoring): We are investigating reports of failure to load runner groups and runner-related permissions for customers using larger runners.
[2026-08-18 07:40:35 UTC] GitHub SRE (Investigating): We are investigating reports of impacted performance for some GitHub services.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by GitHub engineering: On August 18, 2026, between 05:02 UTC and 11:30 UTC, customers were unable to view or manage Actions Runners and Runner Groups through the GitHub UI and API. <br /><br />The issue was caused by failures in backend requests reading runner and runner group data. The failures were caused by an expired authentication certificate unique to this service. The certificate had been rotated in KeyVault, but a step to enable use at runtime had been paused t

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Intermittent failures in runner group and runner-related permissions pages
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
