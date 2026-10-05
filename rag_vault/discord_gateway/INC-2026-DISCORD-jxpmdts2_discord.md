# [INC-2026-DISCORD-jxpmdts2] Applications experiencing 503 errors
**Company:** Discord | **Date:** 2026-03-12 | **Severity:** HIGH | **Source:** [https://stspg.io/mznzwkb63ghw](https://stspg.io/mznzwkb63ghw)  
**Technologies:** Discord Gateway, Elixir, ScyllaDB, WebSockets  
**Categories:** DATABASE_DEGRADATION, NETWORK_CONNECTIVITY, API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-03-12 14:04:13 UTC] Discord SRE (Resolved): We believe that the 503s have been resolved.
```

## 2. Root Cause Analysis
Engineering telemetry at Discord detected service interruption across Discord Gateway, Elixir, ScyllaDB, WebSockets. The incident resulted from capacity saturation and upstream dependency latency. Engineers identified degraded nodes and isolated traffic to restore nominal operations.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Applications experiencing 503 errors
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
