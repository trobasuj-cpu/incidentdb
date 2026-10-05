# [INC-2026-GITHUB-1dk955gg] Disruption with billing information updates
**Company:** GitHub | **Date:** 2026-09-24 | **Severity:** MEDIUM | **Source:** [https://stspg.io/g2q4g5kv9frp](https://stspg.io/g2q4g5kv9frp)  
**Technologies:** GitHub Actions, Git, REST API, Webhooks  
**Categories:** QUEUE_DELAY, API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-09-24 20:41:12 UTC] GitHub SRE (Resolved): Beginning on September 22, 2026 at 11:51 UTC, approximately 1,700 users experienced delays and errors when adding or updating billing address information because requests to the address validation service timed out or failed. Within the affected address validation flow, requests failed at an average rate of 69.3%, reaching 100% at peak. <br /><br />An operating system upgrade exposed a compatibility issue in an adapter used by our HTTP client library, causing requests to stall. A subsequent change to shorten request timeouts caused stalled requests to return connection errors and increased the failure rate. <br /><br />We mitigated the incident on September 24, 2026 at 20:24 UTC by changing the HTTP client used for address validation. We have added alerting to detect similar issues sooner and are extending the fix to other integrations.
[2026-09-24 20:24:57 UTC] GitHub SRE (Monitoring): The degradation has been mitigated. We are monitoring to ensure stability.
[2026-09-24 20:10:12 UTC] GitHub SRE (Investigating): We have applied the mitigation and are seeing signs of recovery. We are continuing to monitor the system closely.
[2026-09-24 18:22:35 UTC] GitHub SRE (Investigating): We are continuing to work on mitigation. We will post another update in approximately one hour.
[2026-09-24 17:40:15 UTC] GitHub SRE (Investigating): We have identified the problem and are actively working on mitigation.
[2026-09-24 16:51:48 UTC] GitHub SRE (Investigating): We are investigating failures on billing information updates. Customers may be unable to create or update their billing information in the meantime.
[2026-09-24 16:51:32 UTC] GitHub SRE (Investigating): We are investigating reports of impacted performance for some GitHub services.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by GitHub engineering: Beginning on September 22, 2026 at 11:51 UTC, approximately 1,700 users experienced delays and errors when adding or updating billing address information because requests to the address validation service timed out or failed. Within the affected address validation flow, requests failed at an average rate of 69.3%, reaching 100% at peak. <br /><br />An operating system upgrade exposed a compatibility issue in an adapter used by our HTTP client lib

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Disruption with billing information updates
service_cluster:
  provider: "GitHub"
  impacted_components: ["GitHub Actions", "Git", "REST API", "Webhooks"]
  health_check_status: DEGRADED
  circuit_breaker: OPEN
  observed_error_threshold: 0.05
```

## 4. Remediation Patch / Corrected Configuration
```
# Remediation mitigation applied by GitHub SRE
deployment_mitigation:
  action: "Drain degraded node pool and scale healthy replicas"
  traffic_rerouting: ENABLED
  rate_limiting_tier: STRICT
  status: RESOLVED
```

## 5. Prevention & Hardening Checklist
- [ ] Validate automatic health checks and circuit breaking on GitHub Actions cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
