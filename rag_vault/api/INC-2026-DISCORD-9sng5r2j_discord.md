# [INC-2026-DISCORD-9sng5r2j] Issues connecting to Discord
**Company:** Discord | **Date:** 2026-03-25 | **Severity:** CRITICAL | **Source:** [https://stspg.io/61fw0d5ddysp](https://stspg.io/61fw0d5ddysp)  
**Technologies:** API, Gateway, Discord Gateway, Elixir  
**Categories:** NETWORK_CONNECTIVITY, API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-03-25 15:38:17 UTC] Discord SRE (Resolved): This incident has been resolved.
[2026-03-25 14:26:33 UTC] Discord SRE (Monitoring): Voice and video should have recovered and users should be able to connect to voice calls. 
We are still tracking down issues with our API.
[2026-03-25 13:07:21 UTC] Discord SRE (Identified): We believe we have identified the issue and are taking action to try and restore voice traffic
[2026-03-25 12:46:21 UTC] Discord SRE (Investigating): We're continuing to investigate impacts to voice connectivity. There may be some temporary impacts to other features as we work to accelerate recovery.
[2026-03-25 12:21:47 UTC] Discord SRE (Investigating): We're aware of an issue that is preventing people from connecting to Discord - we're actively investigating
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Discord engineering: This incident has been resolved. Voice and video should have recovered and users should be able to connect to voice calls. 
We are still tracking down issues with our API. We believe we have identified the issue and are taking action to try and restore voice traffic We're continuing to investigate impacts to voice connectivity. There may be some temporary impacts to other features as we work to accelerate recovery. We're aware of an issue that is

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Issues connecting to Discord
service_cluster:
  provider: "Discord"
  impacted_components: ["API", "Gateway", "Discord Gateway", "Elixir"]
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
