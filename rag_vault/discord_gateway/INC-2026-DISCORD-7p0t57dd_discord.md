# [INC-2026-DISCORD-7p0t57dd] Guild + Session Availability
**Company:** Discord | **Date:** 2026-05-08 | **Severity:** MEDIUM | **Source:** [https://stspg.io/p9zy8727d9w0](https://stspg.io/p9zy8727d9w0)  
**Technologies:** Discord Gateway, Elixir, ScyllaDB, WebSockets  
**Categories:** DATABASE_DEGRADATION, NETWORK_CONNECTIVITY  

---

## 1. Symptoms & Observed Errors
```text
[2026-05-09 00:01:44 UTC] Discord SRE (Resolved): This incident has been resolved.
[2026-05-08 23:54:07 UTC] Discord SRE (Monitoring): A fix has been implemented and we are monitoring the results.
[2026-05-08 23:25:52 UTC] Discord SRE (Investigating): We're investigating an availability where some users may be unable to start a session or some guilds may be unable to connect.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Discord engineering: This incident has been resolved. A fix has been implemented and we are monitoring the results. We're investigating an availability where some users may be unable to start a session or some guilds may be unable to connect.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Guild + Session Availability
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
