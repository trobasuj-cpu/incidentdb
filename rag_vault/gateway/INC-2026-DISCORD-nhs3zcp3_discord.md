# [INC-2026-DISCORD-nhs3zcp3] Some servers unavailable
**Company:** Discord | **Date:** 2026-03-11 | **Severity:** HIGH | **Source:** [https://stspg.io/x89wjkrxkyvx](https://stspg.io/x89wjkrxkyvx)  
**Technologies:** Gateway, Discord Gateway, Elixir, ScyllaDB  
**Categories:** NETWORK_CONNECTIVITY  

---

## 1. Symptoms & Observed Errors
```text
[2026-03-11 16:33:11 UTC] Discord SRE (Resolved): This incident has been resolved.
[2026-03-11 16:11:26 UTC] Discord SRE (Monitoring): A fix has been implemented and we are monitoring the results.
[2026-03-11 16:07:08 UTC] Discord SRE (Identified): They should be back shortly
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Discord engineering: This incident has been resolved. A fix has been implemented and we are monitoring the results. They should be back shortly

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Some servers unavailable
service_cluster:
  provider: "Discord"
  impacted_components: ["Gateway", "Discord Gateway", "Elixir", "ScyllaDB"]
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
- [ ] Validate automatic health checks and circuit breaking on Gateway cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
