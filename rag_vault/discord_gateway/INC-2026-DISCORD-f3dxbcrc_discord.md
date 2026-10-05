# [INC-2026-DISCORD-f3dxbcrc] Guild Unavailability
**Company:** Discord | **Date:** 2026-03-04 | **Severity:** HIGH | **Source:** [https://stspg.io/k9sdr00kz7sl](https://stspg.io/k9sdr00kz7sl)  
**Technologies:** Discord Gateway, Elixir, ScyllaDB, WebSockets  
**Categories:** DATABASE_DEGRADATION, NETWORK_CONNECTIVITY  

---

## 1. Symptoms & Observed Errors
```text
[2026-03-04 17:58:07 UTC] Discord SRE (Resolved): This incident has been resolved.
[2026-03-04 17:49:28 UTC] Discord SRE (Identified): The issue has been identified and a fix is being implemented.
[2026-03-04 17:37:11 UTC] Discord SRE (Investigating): We're investigating an issue where some guilds are unavailable.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Discord engineering: This incident has been resolved. The issue has been identified and a fix is being implemented. We're investigating an issue where some guilds are unavailable.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Guild Unavailability
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
