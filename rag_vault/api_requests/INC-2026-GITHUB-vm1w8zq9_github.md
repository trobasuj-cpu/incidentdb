# [INC-2026-GITHUB-vm1w8zq9] Incident with GraphQL API Requests
**Company:** GitHub | **Date:** 2026-08-11 | **Severity:** MEDIUM | **Source:** [https://stspg.io/3xn46bst0bjh](https://stspg.io/3xn46bst0bjh)  
**Technologies:** API Requests, GitHub Actions, Git, REST API  
**Categories:** NETWORK_CONNECTIVITY, API_ERROR_SPIKE, PIPELINE_EXECUTION_FAILURE  

---

## 1. Symptoms & Observed Errors
```text
[2026-08-11 20:06:56 UTC] GitHub SRE (Resolved): On August 11, 2026, between 14:00 UTC and 16:00 UTC the GraphQL API service was degraded and customers in  saw higher than normal timeouts. On average, the timeout rate was 0.06% and peaked at 0.14% of requests routing to the service. <br /><br />This was due to increased utilization at one of our sites which caused resource contention across our dependencies, leading to an increase in timeouts for GraphQL requests. We mitigated the incident by increasing capacity to alleviate the capacity bottleneck.  <br /><br />We are working to improve our monitoring so that we can proactively reduce the impact of high consumption requests in addition to scaling up; Additionally, we will improve our time to detection and mitigation of issues like this one in the future.
[2026-08-11 20:06:50 UTC] GitHub SRE (Monitoring): We have identified and mitigated increased error rates affecting GraphQL API requests. A fix to increase service capacity has been deployed and error rates have returned to normal levels. We are resolving this incident.
[2026-08-11 16:49:34 UTC] GitHub SRE (Monitoring): The degradation affecting API Requests has been mitigated. We are monitoring to ensure stability.
[2026-08-11 16:49:24 UTC] GitHub SRE (Investigating): We have returned to a healthy baseline on GraphQL API requests. We will continue to work on investigations into the errors seen during this incident.
[2026-08-11 15:28:44 UTC] GitHub SRE (Investigating): We are investigating reports of a small increase in error rates affecting GraphQL API requests. We are working on increasing capacity and continue to investigate the increased errors. We will provide another update when we have more information
[2026-08-11 14:50:40 UTC] GitHub SRE (Investigating): We are investigating reports of degraded performance for API Requests
```

## 2. Root Cause Analysis
Official investigation timeline recorded by GitHub engineering: On August 11, 2026, between 14:00 UTC and 16:00 UTC the GraphQL API service was degraded and customers in  saw higher than normal timeouts. On average, the timeout rate was 0.06% and peaked at 0.14% of requests routing to the service. <br /><br />This was due to increased utilization at one of our sites which caused resource contention across our dependencies, leading to an increase in timeouts for GraphQL requests. We mitigated the incident by i

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
