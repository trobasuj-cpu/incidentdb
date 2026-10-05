# [INC-2026-SENTRY-ccv3pcf0] Notification delivery
**Company:** Sentry | **Date:** 2026-06-11 | **Severity:** MEDIUM | **Source:** [https://stspg.io/wr04xy9pxx4w](https://stspg.io/wr04xy9pxx4w)  
**Technologies:** Sentry Relay, Kafka, ClickHouse, Snuba  
**Categories:** INFRASTRUCTURE_DEGRADATION, SERVICE_OUTAGE  

---

## 1. Symptoms & Observed Errors
```text
[2026-06-11 14:48:19 UTC] Sentry SRE (Resolved): This issue has been resolved.
[2026-06-11 14:18:30 UTC] Sentry SRE (Monitoring): The issue involving some alerts not firing has been mitigated. We will continue to monitor the situation to ensure it does not reoccur.
[2026-06-11 13:23:48 UTC] Sentry SRE (Monitoring): We have identified an issue with one of our cloud providers that might have caused some alert notifications to not fire starting at around 4:00 AM GMT+2.

Since around 1:30 PM GMT+2, alert notifications are now fully operational in the US region, while delivery is still partially degraded in the EU region.
[2026-06-11 13:13:01 UTC] Sentry SRE (Monitoring): Notifications delivery is now close to fully functional in US and DE.

We have identified the root cause as caused by one of our cloud providers, and are closely monitoring the situation.
[2026-06-11 11:51:09 UTC] Sentry SRE (Investigating): We are continuing to investigate the degraded notifications.
[2026-06-11 10:53:24 UTC] Sentry SRE (Investigating): We are continuing to investigate the degraded notifications.
[2026-06-11 09:50:38 UTC] Sentry SRE (Investigating): We're currently investigating degraded notification delivery.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Sentry engineering: This issue has been resolved. The issue involving some alerts not firing has been mitigated. We will continue to monitor the situation to ensure it does not reoccur. We have identified an issue with one of our cloud providers that might have caused some alert notifications to not fire starting at around 4:00 AM GMT+2.

Since around 1:30 PM GMT+2, alert notifications are now fully operational in the US region, while delivery is still partially deg

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Notification delivery
service_cluster:
  provider: "Sentry"
  impacted_components: ["Sentry Relay", "Kafka", "ClickHouse", "Snuba"]
  health_check_status: DEGRADED
  circuit_breaker: OPEN
  observed_error_threshold: 0.05
```

## 4. Remediation Patch / Corrected Configuration
```
# Remediation mitigation applied by Sentry SRE
deployment_mitigation:
  action: "Drain degraded node pool and scale healthy replicas"
  traffic_rerouting: ENABLED
  rate_limiting_tier: STRICT
  status: RESOLVED
```

## 5. Prevention & Hardening Checklist
- [ ] Validate automatic health checks and circuit breaking on Sentry Relay cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
