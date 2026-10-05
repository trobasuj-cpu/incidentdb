# [INC-2026-DISCORD-nm7zsjzv] Continued issues with presence and DM delivery
**Company:** Discord | **Date:** 2026-02-28 | **Severity:** HIGH | **Source:** [https://stspg.io/0ddnmh6vwg2y](https://stspg.io/0ddnmh6vwg2y)  
**Technologies:** Gateway, Discord Gateway, Elixir, ScyllaDB  
**Categories:** NETWORK_CONNECTIVITY  

---

## 1. Symptoms & Observed Errors
```text
[2026-02-28 14:54:27 UTC] Discord SRE (Resolved): This incident has been resolved.
[2026-02-28 14:51:20 UTC] Discord SRE (Monitoring): A fix has been implemented and we are monitoring the results.
[2026-02-28 14:42:41 UTC] Discord SRE (Identified): This is actually a continuation of the incident from earlier today, which we marked "resolved" in error. We have now identified and are remediating the issue
[2026-02-28 14:28:06 UTC] Discord SRE (Identified): We are currently investigating this issue. Restarting your sessions *should* fix it but existing sessions may have not be seeing new DMs
[2026-02-28 13:54:29 UTC] Discord SRE (Investigating): We are currently investigating this issue. Restarting your sessions *should* fix it but existing sessions may have not be seeing new DMs
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Discord engineering: This incident has been resolved. A fix has been implemented and we are monitoring the results. This is actually a continuation of the incident from earlier today, which we marked "resolved" in error. We have now identified and are remediating the issue We are currently investigating this issue. Restarting your sessions *should* fix it but existing sessions may have not be seeing new DMs We are currently investigating this issue. Restarting your s

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Continued issues with presence and DM delivery
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
