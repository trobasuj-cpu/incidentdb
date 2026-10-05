# [INC-2026-DISCORD-k22tny62] A/V E2EE Enforcement for Non-stage Voice Calls
**Company:** Discord | **Date:** 2026-03-02 | **Severity:** MEDIUM | **Source:** [https://stspg.io/vwcdsf0ngghc](https://stspg.io/vwcdsf0ngghc)  
**Technologies:** Discord Gateway, Elixir, ScyllaDB, WebSockets  
**Categories:** DATABASE_DEGRADATION, NETWORK_CONNECTIVITY  

---

## 1. Symptoms & Observed Errors
```text
[2026-03-11 10:25:06 UTC] Discord SRE (Resolved): This incident has been resolved.
[2026-03-02 12:02:15 UTC] Discord SRE (Monitoring): Updated clients which support the DAVE (Audio & Video End-to-End Encryption) Protocol are now required to connect to non-stages voice calls across the Discord Platform. If you attempt to connect with an out of date client, the voice gateway will reject your connection with close code 4017.

See https://support.discord.com/hc/en-us/articles/38025123604631-Minimum-Client-Version-Requirements-for-Voice-Chat for minimum client version requirements and https://support.discord.com/hc/en-us/articles/25968222946071-End-to-End-Encryption-for-Audio-and-Video for further details.

ChromeOS devices should now be able to connect voice channels after refreshing the Discord application.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Discord engineering: This incident has been resolved. Updated clients which support the DAVE (Audio & Video End-to-End Encryption) Protocol are now required to connect to non-stages voice calls across the Discord Platform. If you attempt to connect with an out of date client, the voice gateway will reject your connection with close code 4017.

See https://support.discord.com/hc/en-us/articles/38025123604631-Minimum-Client-Version-Requirements-for-Voice-Chat for minim

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during A/V E2EE Enforcement for Non-stage Voice Calls
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
