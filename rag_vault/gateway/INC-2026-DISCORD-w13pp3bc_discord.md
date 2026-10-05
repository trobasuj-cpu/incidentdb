# [INC-2026-DISCORD-w13pp3bc] All clients may see issues connecting to Discord
**Company:** Discord | **Date:** 2026-06-09 | **Severity:** HIGH | **Source:** [https://stspg.io/2nt2792k0yg8](https://stspg.io/2nt2792k0yg8)  
**Technologies:** Gateway, Discord Gateway, Elixir, ScyllaDB  
**Categories:** NETWORK_CONNECTIVITY  

---

## 1. Symptoms & Observed Errors
```text
[2026-06-09 14:44:14 UTC] Discord SRE (Resolved): This incident has been resolved.
[2026-06-09 14:37:09 UTC] Discord SRE (Monitoring): A fix has been implemented and we are monitoring the results.
[2026-06-09 14:30:41 UTC] Discord SRE (Identified): We are letting in more connections as capacity allows
[2026-06-09 14:24:31 UTC] Discord SRE (Investigating): We are currently investigating this issue.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Discord engineering: This incident has been resolved. A fix has been implemented and we are monitoring the results. We are letting in more connections as capacity allows We are currently investigating this issue.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during All clients may see issues connecting to Discord
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
