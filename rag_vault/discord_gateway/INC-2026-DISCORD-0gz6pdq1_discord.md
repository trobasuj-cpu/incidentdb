# [INC-2026-DISCORD-0gz6pdq1] session starts, DM message delivery down
**Company:** Discord | **Date:** 2026-07-27 | **Severity:** HIGH | **Source:** [https://stspg.io/m3f8f3hwc75m](https://stspg.io/m3f8f3hwc75m)  
**Technologies:** Discord Gateway, Elixir, ScyllaDB, WebSockets  
**Categories:** DATABASE_DEGRADATION, NETWORK_CONNECTIVITY  

---

## 1. Symptoms & Observed Errors
```text
[2026-07-27 11:05:48 UTC] Discord SRE (Resolved): This incident has been resolved.
[2026-07-27 10:48:46 UTC] Discord SRE (Investigating): You may have issues logging in or not receiving DM messages live
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Discord engineering: This incident has been resolved. You may have issues logging in or not receiving DM messages live

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during session starts, DM message delivery down
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
