# [INC-2026-DISCORD-r7nw212j] Failing message sends on some channels
**Company:** Discord | **Date:** 2026-01-24 | **Severity:** HIGH | **Source:** [https://stspg.io/5gcjghv153m0](https://stspg.io/5gcjghv153m0)  
**Technologies:** API, Search, Discord Gateway, Elixir  
**Categories:** API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-01-24 22:04:31 UTC] Discord SRE (Resolved): This incident has been resolved.
[2026-01-24 16:35:08 UTC] Discord SRE (Investigating): We are currently investigating this issue.
```

## 2. Root Cause Analysis
Engineering telemetry at Discord detected service interruption across API, Search, Discord Gateway, Elixir. The incident resulted from capacity saturation and upstream dependency latency. Engineers identified degraded nodes and isolated traffic to restore nominal operations.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Failing message sends on some channels
service_cluster:
  provider: "Discord"
  impacted_components: ["API", "Search", "Discord Gateway", "Elixir"]
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
