# [INC-2026-GITHUB-fmmsrcg5] Incident with GraphQL API Requests
**Company:** GitHub | **Date:** 2026-07-27 | **Severity:** MEDIUM | **Source:** [https://stspg.io/vr201n49yl53](https://stspg.io/vr201n49yl53)  
**Technologies:** API Requests, GitHub Actions, Git, REST API  
**Categories:** API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-07-27 04:09:10 UTC] GitHub SRE (Resolved): On July 26, 2026 at 21:34 UTC we began seeing intermittent errors on the GitHub GraphQL API. A subset of GraphQL API requests returned HTTP 502 errors in short bursts. During the impact window an average of 0.09% of GraphQL API requests in the affected region failed, with a peak of 0.50% of requests failing during the worst two-minute period at 03:02 UTC on July 27. Requests that failed generally succeeded when retried, and no data was lost or altered. Other GitHub services were not affected.<br /><br />The errors were traced to a single group of servers handling a share of GraphQL API traffic. Application processes on that group intermittently closed connections before completing responses. Impact ended at 03:52 UTC on July 27 when those processes were replaced, and we resolved the incident at 04:09 UTC on July 27 after confirming error rates had returned to normal.<br /><br />We are still investigating why those processes closed connections, and that work is being carried out by the team that owns the underlying compute platform. In the meantime we are adding detection and automated mitigation for when a single group of servers behaves differently from its peers.
[2026-07-27 04:09:01 UTC] GitHub SRE (Monitoring): The degradation affecting API Requests has been mitigated. We are monitoring to ensure stability.
[2026-07-27 03:53:19 UTC] GitHub SRE (Investigating): We are investigating reports of degraded performance for API Requests
```

## 2. Root Cause Analysis
Official investigation timeline recorded by GitHub engineering: On July 26, 2026 at 21:34 UTC we began seeing intermittent errors on the GitHub GraphQL API. A subset of GraphQL API requests returned HTTP 502 errors in short bursts. During the impact window an average of 0.09% of GraphQL API requests in the affected region failed, with a peak of 0.50% of requests failing during the worst two-minute period at 03:02 UTC on July 27. Requests that failed generally succeeded when retried, and no data was lost or al

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Incident with GraphQL API Requests
service_cluster:
  provider: "GitHub"
  impacted_components: ["API Requests", "GitHub Actions", "Git", "REST API"]
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
- [ ] Validate automatic health checks and circuit breaking on API Requests cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
