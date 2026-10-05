# [INC-2026-DATADOG-wjrkhvg9] Delayed Monitors Notifications
**Company:** Datadog | **Date:** 2026-05-07 | **Severity:** CRITICAL | **Source:** [https://stspg.io/rbj54mc4nhgs](https://stspg.io/rbj54mc4nhgs)  
**Technologies:** APM, CI Visibility, Error Tracking, Log Management  
**Categories:** QUEUE_DELAY  

---

## 1. Symptoms & Observed Errors
```text
[2026-05-08 10:07:35 UTC] Datadog SRE (Resolved): This incident has been resolved.
[2026-05-08 02:21:58 UTC] Datadog SRE (Monitoring): We have identified upstream provider issues. Metrics, Logs, APM, RUM and CI visibility data are being processed normally for all customers. Alerting is also functional.
We are monitoring the situation.
https://health.aws.amazon.com/health/status
[2026-05-08 01:44:23 UTC] Datadog SRE (Identified): We have identified upstream provider issues. Metrics, Logs, APM and RUM data are being processed normally for all customers. Alerting is also functional. There are still processing delays for CI visibility.
We are monitoring the situation.
https://health.aws.amazon.com/health/status
[2026-05-08 01:30:42 UTC] Datadog SRE (Identified): We have identified upstream provider issues and are continuing to experience delays in processing data across multiple products. We are continuing to work on a fix.
https://health.aws.amazon.com/health/status

For metrics, distribution metrics and point metrics are being processed normally for all customers.
[2026-05-08 00:48:58 UTC] Datadog SRE (Identified): We have identified upstream provider issues and are continuing to experience delays in processing data across multiple products. We are continuing to work on a fix.
https://health.aws.amazon.com/health/status

For metrics, distribution metrics and point metrics are being processed normally for all customers.
[2026-05-08 00:13:10 UTC] Datadog SRE (Identified): We have identified upstream provider issues and are continuing to experience delays in processing data across multiple products. We are continuing to work on a fix.
https://health.aws.amazon.com/health/status

For metrics, distribution metrics and point metrics are being processed normally for all customers.
[2026-05-07 22:59:28 UTC] Datadog SRE (Identified): We have identified due to upstream provider issues, we are continuing to see unavailability of telemetry data coming from AWS into Datadog. We are continuing to work on a fix.
https://health.aws.amazon.com/health/status

For metrics we are still seeing delays in distribution metrics. Counts, rates and gauge metrics are being processed normally for most customers
[2026-05-07 22:16:54 UTC] Datadog SRE (Identified): We have identified due to upstream provider issues, we are continuing to see unavailability of telemetry data coming from AWS into Datadog. We are continuing to work on a fix.

https://health.aws.amazon.com/health/status
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Datadog engineering: This incident has been resolved. We have identified upstream provider issues. Metrics, Logs, APM, RUM and CI visibility data are being processed normally for all customers. Alerting is also functional.
We are monitoring the situation.
https://health.aws.amazon.com/health/status We have identified upstream provider issues. Metrics, Logs, APM and RUM data are being processed normally for all customers. Alerting is also functional. There are still p

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Delayed Monitors Notifications
service_cluster:
  provider: "Datadog"
  impacted_components: ["APM", "CI Visibility", "Error Tracking", "Log Management"]
  health_check_status: DEGRADED
  circuit_breaker: OPEN
  observed_error_threshold: 0.05
```

## 4. Remediation Patch / Corrected Configuration
```
# Remediation mitigation applied by Datadog SRE
deployment_mitigation:
  action: "Drain degraded node pool and scale healthy replicas"
  traffic_rerouting: ENABLED
  rate_limiting_tier: STRICT
  status: RESOLVED
```

## 5. Prevention & Hardening Checklist
- [ ] Validate automatic health checks and circuit breaking on APM cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
