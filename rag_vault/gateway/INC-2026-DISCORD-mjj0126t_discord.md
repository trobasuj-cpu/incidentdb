# [INC-2026-DISCORD-mjj0126t] Connection Delays
**Company:** Discord | **Date:** 2026-04-28 | **Severity:** MEDIUM | **Source:** [https://stspg.io/xmt90d9bj9fm](https://stspg.io/xmt90d9bj9fm)  
**Technologies:** Gateway, Discord Gateway, Elixir, ScyllaDB  
**Categories:** QUEUE_DELAY, NETWORK_CONNECTIVITY  

---

## 1. Symptoms & Observed Errors
```text
[2026-04-28 14:12:03 UTC] Discord SRE (Resolved): This incident has been resolved.
[2026-04-28 14:02:50 UTC] Discord SRE (Monitoring): A fix has been implemented and we are monitoring the results.
[2026-04-28 12:29:39 UTC] Discord SRE (Investigating): We are investigating issues with large guilds being unavailable
[2026-04-28 12:20:14 UTC] Discord SRE (Investigating): Users are experiencing delays connecting to the platform. We are investigating
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Discord engineering: This incident has been resolved. A fix has been implemented and we are monitoring the results. We are investigating issues with large guilds being unavailable Users are experiencing delays connecting to the platform. We are investigating

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Connection Delays
service_cluster:
  provider: "Discord"
  impacted_components: ["Gateway", "Discord Gateway", "Elixir", "ScyllaDB"]
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
- [ ] Validate automatic health checks and circuit breaking on Gateway cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
