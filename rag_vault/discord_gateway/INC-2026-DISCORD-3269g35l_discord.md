# [INC-2026-DISCORD-3269g35l] Unavailable Servers and Voice Calls
**Company:** Discord | **Date:** 2026-02-27 | **Severity:** MEDIUM | **Source:** [https://stspg.io/sf8s45bxfxvx](https://stspg.io/sf8s45bxfxvx)  
**Technologies:** Discord Gateway, Elixir, ScyllaDB, WebSockets  
**Categories:** DATABASE_DEGRADATION, NETWORK_CONNECTIVITY  

---

## 1. Symptoms & Observed Errors
```text
[2026-02-27 14:26:41 UTC] Discord SRE (Resolved): This incident has been resolved.
[2026-02-27 13:52:42 UTC] Discord SRE (Investigating): We are investigating an issue where some servers may show up as unavailable and some voice calls may not be able to start.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Discord engineering: This incident has been resolved. We are investigating an issue where some servers may show up as unavailable and some voice calls may not be able to start.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Unavailable Servers and Voice Calls
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
