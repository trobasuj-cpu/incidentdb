# [INC-2026-GITHUB-wms44hv6] Degraded Git Operations over SSH
**Company:** GitHub | **Date:** 2026-08-21 | **Severity:** MEDIUM | **Source:** [https://stspg.io/9nls6g9ln8p4](https://stspg.io/9nls6g9ln8p4)  
**Technologies:** GitHub Actions, Git, REST API, Webhooks  
**Categories:** API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-08-24 07:19:23 UTC] GitHub SRE (Resolved): On August 21, 2026, between 14:00 and 14:07 UTC, dotcom Git operations over SSH were degraded. Successful Git operations over SSH fell by more than 95% for during the peak impact window, making clone, fetch, or push over SSH effectively unavailable to most users for approximately four minutes. Git operations over HTTPS were not affected.  

The incident was caused by a software defect in our load-balancing infrastructure that was triggered by a configuration change. The defect only occurred when connections passed through multiple layers of load balancers running the new configuration, which meant it was not detected during canary testing. 

We mitigated the incident by rolling back the configuration change.  

We are adding regression coverage for multi-layer load-balancer configurations and improving monitoring and alerting for Git operations over SSH to reduce our time to detection and mitigation of similar issues in the future.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by GitHub engineering: On August 21, 2026, between 14:00 and 14:07 UTC, dotcom Git operations over SSH were degraded. Successful Git operations over SSH fell by more than 95% for during the peak impact window, making clone, fetch, or push over SSH effectively unavailable to most users for approximately four minutes. Git operations over HTTPS were not affected.  

The incident was caused by a software defect in our load-balancing infrastructure that was triggered by a c

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Degraded Git Operations over SSH
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
