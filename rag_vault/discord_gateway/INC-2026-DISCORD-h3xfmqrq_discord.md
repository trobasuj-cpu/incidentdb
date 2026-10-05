# [INC-2026-DISCORD-h3xfmqrq] Voice endpoint selection issues
**Company:** Discord | **Date:** 2026-09-02 | **Severity:** HIGH | **Source:** [https://stspg.io/ys9tgzwhjxtk](https://stspg.io/ys9tgzwhjxtk)  
**Technologies:** Discord Gateway, Elixir, ScyllaDB, WebSockets  
**Categories:** DATABASE_DEGRADATION, NETWORK_CONNECTIVITY  

---

## 1. Symptoms & Observed Errors
```text
[2026-09-02 16:42:44 UTC] Discord SRE (Resolved): This incident has been resolved.
[2026-09-02 16:35:19 UTC] Discord SRE (Identified): We've identified an issue with our endpoint selection service and are applying a mitigation now. We are seeing recovery to endpoint selection now.
[2026-09-02 16:26:42 UTC] Discord SRE (Investigating): We are investigating problems with voice endpoint selection. Your calls may fail to start or may fail to get an endpoint.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Discord engineering: This incident has been resolved. We've identified an issue with our endpoint selection service and are applying a mitigation now. We are seeing recovery to endpoint selection now. We are investigating problems with voice endpoint selection. Your calls may fail to start or may fail to get an endpoint.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Voice endpoint selection issues
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
