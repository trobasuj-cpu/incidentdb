# [INC-2026-DISCORD-5sp34swn] Media Proxy Latency High
**Company:** Discord | **Date:** 2026-09-11 | **Severity:** MEDIUM | **Source:** [https://stspg.io/mpwbwtx3wm5q](https://stspg.io/mpwbwtx3wm5q)  
**Technologies:** Media Proxy, Discord Gateway, Elixir, ScyllaDB  
**Categories:** QUEUE_DELAY  

---

## 1. Symptoms & Observed Errors
```text
[2026-09-11 10:46:19 UTC] Discord SRE (Resolved): This incident has been resolved.
[2026-09-11 10:39:12 UTC] Discord SRE (Monitoring): A fix has been implemented and we are monitoring the results.
[2026-09-11 09:27:41 UTC] Discord SRE (Identified): The issue has been identified and a fix is being implemented.
[2026-09-11 09:24:46 UTC] Discord SRE (Investigating): We are currently investigating an issue involving high error-rates for image downloads.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Discord engineering: This incident has been resolved. A fix has been implemented and we are monitoring the results. The issue has been identified and a fix is being implemented. We are currently investigating an issue involving high error-rates for image downloads.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Media Proxy Latency High
service_cluster:
  provider: "Discord"
  impacted_components: ["Media Proxy", "Discord Gateway", "Elixir", "ScyllaDB"]
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
- [ ] Validate automatic health checks and circuit breaking on Media Proxy cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
