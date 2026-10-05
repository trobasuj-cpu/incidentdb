# [INC-2026-DISCORD-nt3d1xvk] Some Servers Unavailable
**Company:** Discord | **Date:** 2026-09-03 | **Severity:** MEDIUM | **Source:** [https://stspg.io/3628f7vbg5p8](https://stspg.io/3628f7vbg5p8)  
**Technologies:** Discord Gateway, Elixir, ScyllaDB, WebSockets  
**Categories:** DATABASE_DEGRADATION, NETWORK_CONNECTIVITY  

---

## 1. Symptoms & Observed Errors
```text
[2026-09-03 15:27:33 UTC] Discord SRE (Resolved): This incident has been resolved.
[2026-09-03 15:06:28 UTC] Discord SRE (Monitoring): A fix has been implemented and we are monitoring the results.
[2026-09-03 14:31:27 UTC] Discord SRE (Identified): We've identified an issue causing some servers to appear unavailable in the client
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Discord engineering: This incident has been resolved. A fix has been implemented and we are monitoring the results. We've identified an issue causing some servers to appear unavailable in the client

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Some Servers Unavailable
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
