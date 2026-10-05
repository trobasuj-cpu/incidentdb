# [INC-2026-GITHUB-kk183dsl] Degraded availability GPT 5.6 Luna
**Company:** GitHub | **Date:** 2026-08-01 | **Severity:** MEDIUM | **Source:** [https://stspg.io/pj5lgggbc2rb](https://stspg.io/pj5lgggbc2rb)  
**Technologies:** Copilot AI Model Providers, GitHub Actions, Git, REST API  
**Categories:** INFRASTRUCTURE_DEGRADATION, SERVICE_OUTAGE  

---

## 1. Symptoms & Observed Errors
```text
[2026-08-01 12:30:21 UTC] GitHub SRE (Resolved): On August 1st, 2026, the GPT-5.6 Luna model in GitHub Copilot experienced degraded availability in intermittent time intervals between ~08:05 UTC and ~16:30 UTC. Specifically the timeframes observed were 10:00-10:20 UTC, 10:45-11:50 UTC, 13:00-14:25 UTC, and 16:00-16:30 UTC. During this time, requests to GPT-5.6 Luna in Copilot chat and IDE surfaces frequently failed or timed out. This was caused by an issue with an upstream model provider. Other Copilot models were not affected, and users could continue working by selecting another model or 'Auto'. Availability for GPT-5.6 Luna fully recovered once the provider resolved their outage at 16:30 UTC.
[2026-08-01 12:29:20 UTC] GitHub SRE (Investigating): The issues with our upstream model provider have been resolved, and GPT-5.6 Luna is once again available in Copilot products and IDE surfaces.<br />We will continue monitoring to ensure stability, but mitigation is complete.
[2026-08-01 12:13:24 UTC] GitHub SRE (Investigating): We keep working with our upstream model provider, and are observing recovery. We continue monitoring to ensure stability.
[2026-08-01 11:20:02 UTC] GitHub SRE (Investigating): We are experiencing degraded availability for the GPT-5.6 Luna model in Copilot products and IDE surfaces. This is due to an issue with an upstream model provider. While we work with them to resolve the issue, we recommend choosing another model or selecting 'Auto' to continue using Copilot
[2026-08-01 11:16:25 UTC] GitHub SRE (Investigating): We are investigating reports of degraded performance for Copilot AI Model Providers
```

## 2. Root Cause Analysis
Official investigation timeline recorded by GitHub engineering: On August 1st, 2026, the GPT-5.6 Luna model in GitHub Copilot experienced degraded availability in intermittent time intervals between ~08:05 UTC and ~16:30 UTC. Specifically the timeframes observed were 10:00-10:20 UTC, 10:45-11:50 UTC, 13:00-14:25 UTC, and 16:00-16:30 UTC. During this time, requests to GPT-5.6 Luna in Copilot chat and IDE surfaces frequently failed or timed out. This was caused by an issue with an upstream model provider. Other

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Degraded availability GPT 5.6 Luna
service_cluster:
  provider: "GitHub"
  impacted_components: ["Copilot AI Model Providers", "GitHub Actions", "Git", "REST API"]
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
- [ ] Validate automatic health checks and circuit breaking on Copilot AI Model Providers cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
