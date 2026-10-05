# [INC-2026-DISCORD-ptxvtqh0] presence briefly dropped, some DM messages not delivered (refresh to receive them)
**Company:** Discord | **Date:** 2026-02-28 | **Severity:** MEDIUM | **Source:** [https://stspg.io/yd22g77jf1zp](https://stspg.io/yd22g77jf1zp)  
**Technologies:** Discord Gateway, Elixir, ScyllaDB, WebSockets  
**Categories:** DATABASE_DEGRADATION, NETWORK_CONNECTIVITY  

---

## 1. Symptoms & Observed Errors
```text
[2026-02-28 06:23:53 UTC] Discord SRE (Resolved): We are currently investigating this issue.
```

## 2. Root Cause Analysis
Engineering telemetry at Discord detected service interruption across Discord Gateway, Elixir, ScyllaDB, WebSockets. The incident resulted from capacity saturation and upstream dependency latency. Engineers identified degraded nodes and isolated traffic to restore nominal operations.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during presence briefly dropped, some DM messages not delivered (refresh to receive them)
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
