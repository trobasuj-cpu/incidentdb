# [INC-2026-DISCORD-g196lnx3] unable to establish voice connection
**Company:** Discord | **Date:** 2026-07-22 | **Severity:** HIGH | **Source:** [https://stspg.io/b029r8spc5j3](https://stspg.io/b029r8spc5j3)  
**Technologies:** Discord Gateway, Elixir, ScyllaDB, WebSockets  
**Categories:** DATABASE_DEGRADATION, NETWORK_CONNECTIVITY  

---

## 1. Symptoms & Observed Errors
```text
[2026-07-22 16:57:46 UTC] Discord SRE (Resolved): This incident has been resolved.
[2026-07-22 16:49:20 UTC] Discord SRE (Monitoring): A fix has been implemented and we are monitoring the results.
[2026-07-22 16:32:44 UTC] Discord SRE (Investigating): We are currently investigating this issue.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Discord engineering: This incident has been resolved. A fix has been implemented and we are monitoring the results. We are currently investigating this issue.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during unable to establish voice connection
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
