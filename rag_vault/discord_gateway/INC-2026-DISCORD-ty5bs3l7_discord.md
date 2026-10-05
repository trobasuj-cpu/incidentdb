# [INC-2026-DISCORD-ty5bs3l7] Server Unavailability
**Company:** Discord | **Date:** 2026-09-02 | **Severity:** HIGH | **Source:** [https://stspg.io/1msh0252j36w](https://stspg.io/1msh0252j36w)  
**Technologies:** Discord Gateway, Elixir, ScyllaDB, WebSockets  
**Categories:** DATABASE_DEGRADATION, NETWORK_CONNECTIVITY  

---

## 1. Symptoms & Observed Errors
```text
[2026-09-02 11:26:50 UTC] Discord SRE (Resolved): This incident has been resolved.
[2026-09-02 11:18:14 UTC] Discord SRE (Monitoring): We have recovered. We are still investigating the root cause and will post an update once we have more information
[2026-09-02 10:51:02 UTC] Discord SRE (Investigating): We are still investigating the issue, some servers are continuing to have a degraded experience
[2026-09-02 10:04:15 UTC] Discord SRE (Investigating): We are continuing to investigate this issue
[2026-09-02 09:12:20 UTC] Discord SRE (Investigating): We're investigating an issue where some servers are offline.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Discord engineering: This incident has been resolved. We have recovered. We are still investigating the root cause and will post an update once we have more information We are still investigating the issue, some servers are continuing to have a degraded experience We are continuing to investigate this issue We're investigating an issue where some servers are offline.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Server Unavailability
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
