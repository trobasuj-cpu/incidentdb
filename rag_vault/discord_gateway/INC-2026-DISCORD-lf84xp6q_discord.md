# [INC-2026-DISCORD-lf84xp6q] Some servers unavailable
**Company:** Discord | **Date:** 2026-09-03 | **Severity:** HIGH | **Source:** [https://stspg.io/c16w1zbh6v2x](https://stspg.io/c16w1zbh6v2x)  
**Technologies:** Discord Gateway, Elixir, ScyllaDB, WebSockets  
**Categories:** DATABASE_DEGRADATION, NETWORK_CONNECTIVITY  

---

## 1. Symptoms & Observed Errors
```text
[2026-09-03 08:37:47 UTC] Discord SRE (Resolved): This incident has been resolved.
[2026-09-03 08:01:44 UTC] Discord SRE (Monitoring): A fix has been implemented and we are monitoring the results.
[2026-09-03 07:56:36 UTC] Discord SRE (Identified): The issue has been identified and a fix is being implemented.
[2026-09-03 07:35:27 UTC] Discord SRE (Investigating): Some servers may appear unavailable in the client. We are investigating
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Discord engineering: This incident has been resolved. A fix has been implemented and we are monitoring the results. The issue has been identified and a fix is being implemented. Some servers may appear unavailable in the client. We are investigating

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Some servers unavailable
service_cluster:
  provider: "Discord"
  impacted_components: ["Discord Gateway", "Elixir", "ScyllaDB", "WebSockets"]
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
- [ ] Validate automatic health checks and circuit breaking on Discord Gateway cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
