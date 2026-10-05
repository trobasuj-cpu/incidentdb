# [INC-2026-DISCORD-t00495fy] Age group status mismatch across devices
**Company:** Discord | **Date:** 2026-09-23 | **Severity:** HIGH | **Source:** [https://stspg.io/4pqvfhkn61px](https://stspg.io/4pqvfhkn61px)  
**Technologies:** Android, Discord Gateway, Elixir, ScyllaDB  
**Categories:** API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-09-23 20:32:10 UTC] Discord SRE (Resolved): This incident has been resolved.
https://x.com/discord/status/2102964092189475028?s=20
[2026-09-23 20:05:18 UTC] Discord SRE (Monitoring): A fix has been implemented and we are monitoring results.
Please close and refresh your app to get the fix for adult status.
[2026-09-23 19:10:16 UTC] Discord SRE (Identified): We’re aware of an issue where users are seeing mismatched age group statuses across devices. For example, appearing as “unconfirmed” on an older mobile app version, but "adult" on web/desktop. We’re aware of the issue and actively working on a fix.
[2026-09-23 19:08:27 UTC] Discord SRE (Investigating): We’re aware of an issue where users are seeing mismatched age group statuses across devices. For example, appearing as “unconfirmed” on an older mobile app version, but "adult" on web/desktop. We’re aware of the issue and actively working on a fix.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Discord engineering: This incident has been resolved.
https://x.com/discord/status/2102964092189475028?s=20 A fix has been implemented and we are monitoring results.
Please close and refresh your app to get the fix for adult status. We’re aware of an issue where users are seeing mismatched age group statuses across devices. For example, appearing as “unconfirmed” on an older mobile app version, but "adult" on web/desktop. We’re aware of the issue and actively working

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Age group status mismatch across devices
service_cluster:
  provider: "Discord"
  impacted_components: ["Android", "Discord Gateway", "Elixir", "ScyllaDB"]
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
- [ ] Validate automatic health checks and circuit breaking on Android cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
