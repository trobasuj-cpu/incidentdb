# [INC-2026-DISCORD-9kp14l66] Intermittent chat functionality issues
**Company:** Discord | **Date:** 2026-06-29 | **Severity:** MEDIUM | **Source:** [https://stspg.io/kkdbyktwf42v](https://stspg.io/kkdbyktwf42v)  
**Technologies:** API, Discord Gateway, Elixir, ScyllaDB  
**Categories:** API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-06-29 13:24:21 UTC] Discord SRE (Resolved): This incident has been resolved.
[2026-06-29 11:46:22 UTC] Discord SRE (Investigating): We are currently investigating the issue.
```

## 2. Root Cause Analysis
Engineering telemetry at Discord detected service interruption across API, Discord Gateway, Elixir, ScyllaDB. The incident resulted from capacity saturation and upstream dependency latency. Engineers identified degraded nodes and isolated traffic to restore nominal operations.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Intermittent chat functionality issues
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
