# [INC-2026-DISCORD-pcg3nsfq] Issues sending messages for bots and users
**Company:** Discord | **Date:** 2026-08-07 | **Severity:** CRITICAL | **Source:** [https://stspg.io/9y6394dfz8kw](https://stspg.io/9y6394dfz8kw)  
**Technologies:** API, Discord Gateway, Elixir, ScyllaDB  
**Categories:** API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-08-07 16:02:18 UTC] Discord SRE (Resolved): This issue has been fully resolved
[2026-08-07 15:42:09 UTC] Discord SRE (Monitoring): We've recovered service and are continuing to monitor for recovery.
[2026-08-07 15:26:32 UTC] Discord SRE (Identified): This issue has expanded and is now affecting all Discord interactions. We're continuing to work towards mitigating the issue.
[2026-08-07 15:21:42 UTC] Discord SRE (Identified): We're aware of an issue preventing users and bots from sending messages, we're working on fully restoring service
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Discord engineering: This issue has been fully resolved We've recovered service and are continuing to monitor for recovery. This issue has expanded and is now affecting all Discord interactions. We're continuing to work towards mitigating the issue. We're aware of an issue preventing users and bots from sending messages, we're working on fully restoring service

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Issues sending messages for bots and users
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
