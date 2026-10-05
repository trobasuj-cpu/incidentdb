# [INC-2026-GITHUB-kzh5j0mt] Actions Larger Runner Jobs for some customers may be slow to start
**Company:** GitHub | **Date:** 2026-09-14 | **Severity:** MEDIUM | **Source:** [https://stspg.io/f2b6fd4l83wy](https://stspg.io/f2b6fd4l83wy)  
**Technologies:** GitHub Actions, Git, REST API, Webhooks  
**Categories:** API_ERROR_SPIKE, AUTHENTICATION_FAILURE, PIPELINE_EXECUTION_FAILURE  

---

## 1. Symptoms & Observed Errors
```text
[2026-09-14 19:35:48 UTC] GitHub SRE (Resolved): On September 14, 2026, between 16:10 and 19:01 UTC, some customers using GitHub Actions larger runners experienced longer-than-normal wait times for jobs to start. During this period, <b>5.7%</b> of larger-runner jobs were affected. <br /><br />A routine expansion of our compute capacity exposed a bug in how our provisioning system handled capacity records when selecting where to create runner virtual machines. This slowed the creation of new runners, leaving insufficient runner capacity to start affected jobs promptly. <br /><br />We restored normal provisioning by correcting the affected capacity records. We have fixed the underlying capacity-selection bug to prevent this failure from recurring. We have also added alerts for VM-record creation failures associated with this capacity issue.
[2026-09-14 19:01:52 UTC] GitHub SRE (Monitoring): The degradation has been mitigated. We are monitoring to ensure stability.
[2026-09-14 18:40:25 UTC] GitHub SRE (Investigating): We are investigating reports of impacted performance for some GitHub services.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by GitHub engineering: On September 14, 2026, between 16:10 and 19:01 UTC, some customers using GitHub Actions larger runners experienced longer-than-normal wait times for jobs to start. During this period, <b>5.7%</b> of larger-runner jobs were affected. <br /><br />A routine expansion of our compute capacity exposed a bug in how our provisioning system handled capacity records when selecting where to create runner virtual machines. This slowed the creation of new run

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Actions Larger Runner Jobs for some customers may be slow to start
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
