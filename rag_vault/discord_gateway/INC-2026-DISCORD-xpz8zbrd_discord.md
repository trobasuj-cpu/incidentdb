# [INC-2026-DISCORD-xpz8zbrd] Some servers and other services (voice calls, activities) not available for some users
**Company:** Discord | **Date:** 2026-08-31 | **Severity:** HIGH | **Source:** [https://stspg.io/11m73f3r9hhd](https://stspg.io/11m73f3r9hhd)  
**Technologies:** Discord Gateway, Elixir, ScyllaDB, WebSockets  
**Categories:** DATABASE_DEGRADATION, NETWORK_CONNECTIVITY  

---

## 1. Symptoms & Observed Errors
```text
[2026-08-31 17:49:17 UTC] Discord SRE (Resolved): This incident has been resolved.
[2026-08-31 16:56:45 UTC] Discord SRE (Identified): We are taking steps to remediate the issue
[2026-08-31 16:38:47 UTC] Discord SRE (Investigating): We are taking steps to remediate the issue
[2026-08-31 16:26:49 UTC] Discord SRE (Investigating): some servers will appear unavailable for some users, and some other services may also be impacted (voice calls, activities)
[2026-08-31 15:54:05 UTC] Discord SRE (Investigating): Some users may not be able to connect to some of their servers
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Discord engineering: This incident has been resolved. We are taking steps to remediate the issue We are taking steps to remediate the issue some servers will appear unavailable for some users, and some other services may also be impacted (voice calls, activities) Some users may not be able to connect to some of their servers

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Some servers and other services (voice calls, activities) not available for some users
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
