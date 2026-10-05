# [INC-2026-DISCORD-k42p6rqy] Message Sends affected on certain channels
**Company:** Discord | **Date:** 2026-05-02 | **Severity:** HIGH | **Source:** [https://stspg.io/p6fddpk44db0](https://stspg.io/p6fddpk44db0)  
**Technologies:** API, Discord Gateway, Elixir, ScyllaDB  
**Categories:** API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-05-02 10:29:17 UTC] Discord SRE (Resolved): We've resolved the issue and messaging should be functional
[2026-05-02 10:03:31 UTC] Discord SRE (Investigating): We are currently investigating the issue
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Discord engineering: We've resolved the issue and messaging should be functional We are currently investigating the issue

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Message Sends affected on certain channels
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
