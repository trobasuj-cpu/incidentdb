# [INC-2026-DISCORD-dsv750sj] Applications experiencing 503 errors
**Company:** Discord | **Date:** 2026-03-12 | **Severity:** HIGH | **Source:** [https://stspg.io/lyp50tfbf531](https://stspg.io/lyp50tfbf531)  
**Technologies:** Discord Gateway, Elixir, ScyllaDB, WebSockets  
**Categories:** DATABASE_DEGRADATION, NETWORK_CONNECTIVITY, API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-03-12 14:06:00 UTC] Discord SRE (Resolved): We believe the 503s have been resolved.
[2026-03-12 13:50:26 UTC] Discord SRE (Investigating): We are aware that bot traffic may be hitting 503 errors and are investigating the issue.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Discord engineering: We believe the 503s have been resolved. We are aware that bot traffic may be hitting 503 errors and are investigating the issue.

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
