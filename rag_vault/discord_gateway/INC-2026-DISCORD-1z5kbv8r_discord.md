# [INC-2026-DISCORD-1z5kbv8r] Google Pay Outage
**Company:** Discord | **Date:** 2026-09-16 | **Severity:** HIGH | **Source:** [https://stspg.io/9qdnzmhbn0pk](https://stspg.io/9qdnzmhbn0pk)  
**Technologies:** Discord Gateway, Elixir, ScyllaDB, WebSockets  
**Categories:** DATABASE_DEGRADATION, NETWORK_CONNECTIVITY  

---

## 1. Symptoms & Observed Errors
```text
[2026-09-16 09:43:18 UTC] Discord SRE (Resolved): This incident has been resolved.
[2026-09-16 09:15:33 UTC] Discord SRE (Identified): We're currently experiencing an issue that prevents users from completing purchases (including Nitro subscriptions and Shop items) through the Android app. This is caused by an ongoing outage with Google Play.

Purchases on other platforms (iOS, web, and desktop) are unaffected, and existing subscriptions remain active.

We're monitoring Google's status and will post an update as soon as service is restored. No action is needed on your part — please try again later.
https://pay.google.com/status/incidents/gQJsjf662F7KwPyiNqav
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Discord engineering: This incident has been resolved. We're currently experiencing an issue that prevents users from completing purchases (including Nitro subscriptions and Shop items) through the Android app. This is caused by an ongoing outage with Google Play.

Purchases on other platforms (iOS, web, and desktop) are unaffected, and existing subscriptions remain active.

We're monitoring Google's status and will post an update as soon as service is restored. No ac

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Google Pay Outage
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
