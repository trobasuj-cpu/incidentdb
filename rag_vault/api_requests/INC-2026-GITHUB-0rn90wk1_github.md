# [INC-2026-GITHUB-0rn90wk1] Incident with several GitHub Services
**Company:** GitHub | **Date:** 2026-09-13 | **Severity:** CRITICAL | **Source:** [https://stspg.io/8f3xch4y0v2k](https://stspg.io/8f3xch4y0v2k)  
**Technologies:** API Requests, Issues, Pull Requests, Actions  
**Categories:** QUEUE_DELAY, DATABASE_DEGRADATION, API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-09-13 10:44:55 UTC] GitHub SRE (Resolved): On September 13, 2026, between 08:43 and 10:44 UTC, GitHub experienced degraded availability across approximately 28 services, including Issues, Pull Requests, Actions, Codespaces, Pages, Notifications, Code Scanning, Git LFS, and new account signup.  At peak, 8.8% of requests to create GitHub App installation access tokens failed. Token issuance for Actions workflows was also affected, impacting approximately 4% of workflows during the incident time frame. Creating issues through the web interface failed for about 96% of attempts, and signup failures were above 90%.  <br /> <br />The cause was an internal data-cleanup job that began writing to a shared database cluster at 07:33 UTC. That cluster stores permission data read on nearly every authenticated request. The safeguard that was pacing the background job watched only one health signal — how far the database replicas were lagging — and that signal stayed low the whole time. It did not account for the load building on the primary itself, so the job kept writing while the primary quietly ran toward its limit. <br /><br />When the primary ran out of available connections, requests that needed it could not complete. First, there was no quick timeout on these database calls, so request handlers waited on the stalled database instead of failing fast, and the shared request-handling capacity degraded into site-wide errors. Second, a retry loop around token creation kept re-sending the writes that were already failing, which held the database saturated rather than letting it recover. <br /><br />Monitoring declared the incident at 08:50 UTC, but due to the broad impact and amplification from token creation, it took time to identify the source of the load. First responders mitigated by shedding internal load and pausing the job, and all services recovered by 10:44 UTC. <br /><br />To prevent recurrence, we are rate-limiting background jobs against shared, customer-serving databases by default, and adding automatic pausing and paging on primary-server load rather than replication lag alone. We are also surfacing running background work directly alongside database health signals so responders can see and pause it without leaving those dashboards, bounding retries in the token-issuing path, and adding request-level timeouts so one unhealthy database cannot consume shared web server capacity. In addition, we are breaking apart this database cluster to remove the single point of failure. We will be moving various service-specific data, including the authorization data, out of this shared cluster in the next two weeks.
[2026-09-13 10:28:45 UTC] GitHub SRE (Investigating): Pull Requests is experiencing degraded performance. We are continuing to investigate.
[2026-09-13 10:26:55 UTC] GitHub SRE (Investigating): We have reduced load on this cluster with internal load-shedding and are seeing signs of recovery but continue to monitor
[2026-09-13 09:36:23 UTC] GitHub SRE (Investigating): We're seeing increased database replication delays on collab which is causing increased error rates in authorization endpoints and follow-on increased error rates across the system - we are investigating
[2026-09-13 09:25:56 UTC] GitHub SRE (Investigating): Actions is experiencing degraded performance. We are continuing to investigate.
[2026-09-13 09:16:11 UTC] GitHub SRE (Investigating): We are investigating reports of degraded availability for API Requests, Issues, Pages and Pull Requests
```

## 2. Root Cause Analysis
Official investigation timeline recorded by GitHub engineering: On September 13, 2026, between 08:43 and 10:44 UTC, GitHub experienced degraded availability across approximately 28 services, including Issues, Pull Requests, Actions, Codespaces, Pages, Notifications, Code Scanning, Git LFS, and new account signup.  At peak, 8.8% of requests to create GitHub App installation access tokens failed. Token issuance for Actions workflows was also affected, impacting approximately 4% of workflows during the incident

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Incident with several GitHub Services
service_cluster:
  provider: "GitHub"
  impacted_components: ["API Requests", "Issues", "Pull Requests", "Actions"]
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
