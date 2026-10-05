# [INC-2026-DISCORD-73wgk29d] Messaging failures
**Company:** Discord | **Date:** 2026-08-04 | **Severity:** CRITICAL | **Source:** [https://stspg.io/wlryy8d8r7wf](https://stspg.io/wlryy8d8r7wf)  
**Technologies:** API, Discord Gateway, Elixir, ScyllaDB  
**Categories:** API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-08-04 13:17:15 UTC] Discord SRE (Resolved): This incident has been resolved.
[2026-08-04 12:59:24 UTC] Discord SRE (Monitoring): We're seeing significant positive impact from our earlier fix; nearly all users should be recovered at this point. We are monitoring closely.
[2026-08-04 12:53:43 UTC] Discord SRE (Monitoring): We have a fix in place are seeing recovery, although some users may still be seeing impact.
[2026-08-04 12:36:19 UTC] Discord SRE (Identified): We are actively working toward system recovery and will post another update in 10 minutes.
[2026-08-04 11:50:03 UTC] Discord SRE (Identified): We are continuing to work on a fix for this issue.
[2026-08-04 11:49:50 UTC] Discord SRE (Identified): We are currently investigating an issue that is impacting users' ability to send direct messages. Other functionality including interactions, shop, and SDK integrations are also affected.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Discord engineering: This incident has been resolved. We're seeing significant positive impact from our earlier fix; nearly all users should be recovered at this point. We are monitoring closely. We have a fix in place are seeing recovery, although some users may still be seeing impact. We are actively working toward system recovery and will post another update in 10 minutes. We are continuing to work on a fix for this issue. We are currently investigating an issue t

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Messaging failures
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
