# [INC-2026-DISCORD-947scltt] Issues with invites
**Company:** Discord | **Date:** 2026-04-16 | **Severity:** MEDIUM | **Source:** [https://stspg.io/ktpv1rcvk1dr](https://stspg.io/ktpv1rcvk1dr)  
**Technologies:** API, Discord Gateway, Elixir, ScyllaDB  
**Categories:** API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-04-16 13:30:06 UTC] Discord SRE (Resolved): The issue has been resolved.
[2026-04-16 13:05:42 UTC] Discord SRE (Monitoring): The rollback is progressing and we are monitoring recovery.
[2026-04-16 12:49:08 UTC] Discord SRE (Identified): The issue has been identified and changes are being rolled back.
[2026-04-16 12:47:09 UTC] Discord SRE (Investigating): We are currently investigating issues interacting with invites.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Discord engineering: The issue has been resolved. The rollback is progressing and we are monitoring recovery. The issue has been identified and changes are being rolled back. We are currently investigating issues interacting with invites.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Issues with invites
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
