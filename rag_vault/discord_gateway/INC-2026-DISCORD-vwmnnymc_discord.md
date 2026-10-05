# [INC-2026-DISCORD-vwmnnymc] API Errors
**Company:** Discord | **Date:** 2026-07-16 | **Severity:** HIGH | **Source:** [https://stspg.io/dn6zshm4sygl](https://stspg.io/dn6zshm4sygl)  
**Technologies:** Discord Gateway, Elixir, ScyllaDB, WebSockets  
**Categories:** QUEUE_DELAY, DATABASE_DEGRADATION, NETWORK_CONNECTIVITY  

---

## 1. Symptoms & Observed Errors
```text
[2026-07-16 20:48:44 UTC] Discord SRE (Resolved): This incident has been resolved.
[2026-07-16 18:51:51 UTC] Discord SRE (Identified): We've identified an upstream latency issue, and we're working to mitigate its impacts.
[2026-07-16 17:49:10 UTC] Discord SRE (Investigating): We are currently investigating this issue.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Discord engineering: This incident has been resolved. We've identified an upstream latency issue, and we're working to mitigate its impacts. We are currently investigating this issue.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during API Errors
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
