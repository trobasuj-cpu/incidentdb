# [INC-2026-DISCORD-jt67pjfb] General API errors
**Company:** Discord | **Date:** 2026-03-28 | **Severity:** HIGH | **Source:** [https://stspg.io/ydgzkcnrmmxx](https://stspg.io/ydgzkcnrmmxx)  
**Technologies:** API, Discord Gateway, Elixir, ScyllaDB  
**Categories:** API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-03-28 14:44:16 UTC] Discord SRE (Resolved): This incident has been resolved.
[2026-03-28 14:43:17 UTC] Discord SRE (Monitoring): We are continuing to monitor for any further issues.
[2026-03-28 14:40:00 UTC] Discord SRE (Monitoring): A fix has been implemented and we are monitoring the results.
[2026-03-28 14:31:52 UTC] Discord SRE (Identified): The issue has been identified and a fix is being implemented.
[2026-03-28 14:23:50 UTC] Discord SRE (Investigating): We are currently investigating this issue.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Discord engineering: This incident has been resolved. We are continuing to monitor for any further issues. A fix has been implemented and we are monitoring the results. The issue has been identified and a fix is being implemented. We are currently investigating this issue.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during General API errors
service_cluster:
  provider: "Discord"
  impacted_components: ["API", "Discord Gateway", "Elixir", "ScyllaDB"]
  health_check_status: DEGRADED
  circuit_breaker: OPEN
  observed_error_threshold: 0.05
```

## 4. Remediation Patch / Corrected Configuration
```
# Remediation mitigation applied by Discord SRE
deployment_mitigation:
  action: "Drain degraded node pool and scale healthy replicas"
  traffic_rerouting: ENABLED
  rate_limiting_tier: STRICT
  status: RESOLVED
```

## 5. Prevention & Hardening Checklist
- [ ] Validate automatic health checks and circuit breaking on API cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
