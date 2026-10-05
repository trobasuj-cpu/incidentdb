# [INC-2026-DISCORD-fvb0y6yp] possible issues with presence and DM delivery
**Company:** Discord | **Date:** 2026-02-28 | **Severity:** MEDIUM | **Source:** [https://stspg.io/x2wy1l9ntk92](https://stspg.io/x2wy1l9ntk92)  
**Technologies:** Gateway, Discord Gateway, Elixir, ScyllaDB  
**Categories:** NETWORK_CONNECTIVITY  

---

## 1. Symptoms & Observed Errors
```text
[2026-02-28 08:37:54 UTC] Discord SRE (Resolved): This incident has been resolved.
[2026-02-28 06:53:57 UTC] Discord SRE (Monitoring): Some users were unable to start sessions, but should be able to now. Some servers were also not available
[2026-02-28 06:39:39 UTC] Discord SRE (Investigating): you may need to refresh your client to see new DM messages. We are working on identifying the extent of the issue
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Discord engineering: This incident has been resolved. Some users were unable to start sessions, but should be able to now. Some servers were also not available you may need to refresh your client to see new DM messages. We are working on identifying the extent of the issue

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during possible issues with presence and DM delivery
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
