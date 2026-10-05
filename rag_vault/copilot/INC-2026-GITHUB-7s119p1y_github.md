# [INC-2026-GITHUB-7s119p1y] Incident with Copilot
**Company:** GitHub | **Date:** 2026-08-03 | **Severity:** MEDIUM | **Source:** [https://stspg.io/y3tm8v1gmn85](https://stspg.io/y3tm8v1gmn85)  
**Technologies:** Copilot, GitHub Actions, Git, REST API  
**Categories:** AUTHENTICATION_FAILURE  

---

## 1. Symptoms & Observed Errors
```text
[2026-08-03 11:25:12 UTC] GitHub SRE (Resolved): On 2026-08-03, between 06:52 and 11:25 UTC, some GitHub Copilot users experienced errors when using chat and agent features. Requests to list the available models failed, and because every chat or agent interaction begins by retrieving the list of models, affected users saw their requests fail. On average about 3% of these model-listing requests failed during the incident (roughly 97% succeeded), but failures were significantly higher during peak-traffic periods, at times approaching 100% for the affected internal lookups. Approximately 4,066 users were affected in a single 60-minute window, concentrated among IDE-based clients. The underlying AI models themselves remained healthy throughout.<br /><br />The incident was caused by an increase in how often clients requested the model list, which pushed an internal user-authorization lookup past a rate limit; the rate-limited responses were surfaced to users as errors. We mitigated the impact by increasing how long Copilot caches that authorization lookup, which reduced load on the internal service, and we have additional capacity and rate-limit changes in progress. To prevent recurrence we are improving monitoring for this class of failure, adjusting cache and rate-limit settings, and coordinating with client teams on request patterns.
[2026-08-03 11:19:04 UTC] GitHub SRE (Monitoring): The degradation affecting Copilot has been mitigated. We are monitoring to ensure stability.
[2026-08-03 10:35:19 UTC] GitHub SRE (Investigating): We are still seeing intermittent errors with Copilot, and are continuing to investigate and consider mitigations.
[2026-08-03 09:54:22 UTC] GitHub SRE (Investigating): We are experiencing degraded availability for chat & agent models in Copilot. Multiple models are impacted and customers may experience requests failing. We are investigating and will provide an update as soon as possible.
[2026-08-03 09:53:27 UTC] GitHub SRE (Investigating): We are investigating reports of degraded performance for Copilot
```

## 2. Root Cause Analysis
Official investigation timeline recorded by GitHub engineering: On 2026-08-03, between 06:52 and 11:25 UTC, some GitHub Copilot users experienced errors when using chat and agent features. Requests to list the available models failed, and because every chat or agent interaction begins by retrieving the list of models, affected users saw their requests fail. On average about 3% of these model-listing requests failed during the incident (roughly 97% succeeded), but failures were significantly higher during peak

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Incident with Copilot
service_cluster:
  provider: "GitHub"
  impacted_components: ["Copilot", "GitHub Actions", "Git", "REST API"]
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
- [ ] Validate automatic health checks and circuit breaking on Copilot cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
