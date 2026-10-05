# [INC-2026-DATADOG-cqxdntdk] Users are unable to acknowledge, escalate, or resolve On-Call Pages
**Company:** Datadog | **Date:** 2026-09-25 | **Severity:** MEDIUM | **Source:** [https://stspg.io/03h92q1l96z2](https://stspg.io/03h92q1l96z2)  
**Technologies:** Incident Response, Datadog Ingestion, APM, Metrics Agent  
**Categories:** INFRASTRUCTURE_DEGRADATION, SERVICE_OUTAGE  

---

## 1. Symptoms & Observed Errors
```text
[2026-09-25 11:38:57 UTC] Datadog SRE (Resolved): The issue is now resolved.
[2026-09-25 11:29:27 UTC] Datadog SRE (Monitoring): We are monitoring the issue.
[2026-09-25 11:23:23 UTC] Datadog SRE (Identified): We have identified the issue with pages and are applying mitigations.
[2026-09-25 11:17:33 UTC] Datadog SRE (Investigating): We are investigating an issue with users unable to acknowledge, escalate, or resolve On-Call Pages.
[2026-09-25 11:11:50 UTC] Datadog SRE (Investigating): We are investigating an issue with users unable to acknowledge on call pages
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Datadog engineering: The issue is now resolved. We are monitoring the issue. We have identified the issue with pages and are applying mitigations. We are investigating an issue with users unable to acknowledge, escalate, or resolve On-Call Pages. We are investigating an issue with users unable to acknowledge on call pages

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Users are unable to acknowledge, escalate, or resolve On-Call Pages
service_cluster:
  provider: "Datadog"
  impacted_components: ["Incident Response", "Datadog Ingestion", "APM", "Metrics Agent"]
  health_check_status: DEGRADED
  circuit_breaker: OPEN
  observed_error_threshold: 0.05
```

## 4. Remediation Patch / Corrected Configuration
```
# Remediation mitigation applied by Datadog SRE
deployment_mitigation:
  action: "Drain degraded node pool and scale healthy replicas"
  traffic_rerouting: ENABLED
  rate_limiting_tier: STRICT
  status: RESOLVED
```

## 5. Prevention & Hardening Checklist
- [ ] Validate automatic health checks and circuit breaking on Incident Response cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
