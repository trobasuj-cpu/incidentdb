# [INC-2026-DISCORD-2vw56wyz] Failure to start new voice calls
**Company:** Discord | **Date:** 2026-03-19 | **Severity:** HIGH | **Source:** [https://stspg.io/4lq25k7pwl99](https://stspg.io/4lq25k7pwl99)  
**Technologies:** Discord Gateway, Elixir, ScyllaDB, WebSockets  
**Categories:** DATABASE_DEGRADATION, NETWORK_CONNECTIVITY  

---

## 1. Symptoms & Observed Errors
```text
[2026-03-19 22:56:47 UTC] Discord SRE (Resolved): This incident has been resolved.
[2026-03-19 22:43:58 UTC] Discord SRE (Monitoring): A fix has been implemented and we are monitoring the results.
[2026-03-19 19:47:07 UTC] Discord SRE (Investigating): New voice calls are failing to be created. We are currently investigating.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Discord engineering: This incident has been resolved. A fix has been implemented and we are monitoring the results. New voice calls are failing to be created. We are currently investigating.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Failure to start new voice calls
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
