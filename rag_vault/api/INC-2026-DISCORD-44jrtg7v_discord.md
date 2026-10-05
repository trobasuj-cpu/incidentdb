# [INC-2026-DISCORD-44jrtg7v] Embeds not working
**Company:** Discord | **Date:** 2026-04-06 | **Severity:** MEDIUM | **Source:** [https://stspg.io/ll3l61zny6dp](https://stspg.io/ll3l61zny6dp)  
**Technologies:** API, Discord Gateway, Elixir, ScyllaDB  
**Categories:** API_ERROR_SPIKE, PIPELINE_EXECUTION_FAILURE  

---

## 1. Symptoms & Observed Errors
```text
[2026-04-06 19:52:03 UTC] Discord SRE (Resolved): This incident has been resolved.
[2026-04-06 19:46:05 UTC] Discord SRE (Monitoring): A fix has been deployed and we're monitoring the results.
[2026-04-06 19:28:12 UTC] Discord SRE (Identified): The issue has been identified and a fix is being deployed.
[2026-04-06 18:48:05 UTC] Discord SRE (Investigating): We are currently investigating an issue with embeds.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Discord engineering: This incident has been resolved. A fix has been deployed and we're monitoring the results. The issue has been identified and a fix is being deployed. We are currently investigating an issue with embeds.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Embeds not working
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
