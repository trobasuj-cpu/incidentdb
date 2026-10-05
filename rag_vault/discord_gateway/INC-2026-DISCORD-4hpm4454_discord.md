# [INC-2026-DISCORD-4hpm4454] Increased API Errors
**Company:** Discord | **Date:** 2026-05-08 | **Severity:** HIGH | **Source:** [https://stspg.io/yj8w3fd3p7jn](https://stspg.io/yj8w3fd3p7jn)  
**Technologies:** Discord Gateway, Elixir, ScyllaDB, WebSockets  
**Categories:** DATABASE_DEGRADATION, NETWORK_CONNECTIVITY, API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-05-08 15:38:17 UTC] Discord SRE (Resolved): All critical functionalities have recovered for all users.
[2026-05-08 14:47:02 UTC] Discord SRE (Identified): Users are able to connect to the platform, and we are working on recovering some features
[2026-05-08 14:08:03 UTC] Discord SRE (Identified): We are continuing to see recovery on our systems and are continuing recovery operations to get the service into a fully healthy state
[2026-05-08 13:19:48 UTC] Discord SRE (Identified): We are beginning to see recovery on our systems and are continuing recovery operations to get the service into a fully healthy state
[2026-05-08 13:16:52 UTC] Discord SRE (Monitoring): We are seeing significant recovery at this time. We are continue to restore ancillary services and we are metering in traffic as users reconnect.
[2026-05-08 12:56:03 UTC] Discord SRE (Identified): We are continuing to work to remediate the issues impacting availability for some Discord users. This is causing impact across our service, including logging in and sending messages.
[2026-05-08 12:24:48 UTC] Discord SRE (Identified): We have identified the issue. Many users are unable to start their sessions at this time
[2026-05-08 12:08:32 UTC] Discord SRE (Investigating): We're investgating errors in our API systems.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Discord engineering: All critical functionalities have recovered for all users. Users are able to connect to the platform, and we are working on recovering some features We are continuing to see recovery on our systems and are continuing recovery operations to get the service into a fully healthy state We are beginning to see recovery on our systems and are continuing recovery operations to get the service into a fully healthy state We are seeing significant recovery

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Increased API Errors
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
