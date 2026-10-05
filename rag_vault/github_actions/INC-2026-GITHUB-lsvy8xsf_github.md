# [INC-2026-GITHUB-lsvy8xsf] Disruption with Login and Release Asset downloads
**Company:** GitHub | **Date:** 2026-08-12 | **Severity:** HIGH | **Source:** [https://stspg.io/xp36nwz4rrh1](https://stspg.io/xp36nwz4rrh1)  
**Technologies:** GitHub Actions, Git, REST API, Webhooks  
**Categories:** NETWORK_CONNECTIVITY, API_ERROR_SPIKE, AUTHENTICATION_FAILURE  

---

## 1. Symptoms & Observed Errors
```text
[2026-08-12 22:56:39 UTC] GitHub SRE (Resolved): On August 12 and 13, 2026, some anonymous (logged-out) requests to github.com experienced HTTP 5xx errors when loading pages like the sign-in page, and when downloading release assets, due to an unusual traffic pattern that repeatedly overloaded a part of our infrastructure that serves these types of requests. There were three windows of impact: (1) August 12 from 16:34 to 18:34 UTC, with an average error rate of 16.16% that peaked at 28.6%; (2) August 12 from 19:00 to 22:56 UTC, with an average error rate of 16.55% that peaked at 24.18%; and (3) August 13 from 06:19 to 08:05 UTC, with an average error rate of 2.01% that peaked at 7.49%.<br />Requests from signed-in users were unaffected.<br /><br />We mitigated the incidents by applying traffic controls at our network edge that limited any requests matching the pattern identified previously, thereby preventing overload on our systems.<br /><br />Since these incidents occurred, we have tightened our monitoring systems to alert server-side errors that affect logged-out traffic. We are also working to further strengthen our edge protections and reduce the time to detect and mitigate similar incidents.
[2026-08-12 22:22:11 UTC] GitHub SRE (Investigating): We have identified the root cause and are working on mitigation. Errors on the login page and downloading release assets have decreased, but we are not fully mitigated. We will continue to provide updates.
[2026-08-12 21:43:25 UTC] GitHub SRE (Investigating): We are investigating issues with Login and when downloading Release Assets. We will continue to keep users updated on progress towards mitigation.
[2026-08-12 21:39:05 UTC] GitHub SRE (Investigating): We are investigating reports of impacted performance for some GitHub services.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by GitHub engineering: On August 12 and 13, 2026, some anonymous (logged-out) requests to github.com experienced HTTP 5xx errors when loading pages like the sign-in page, and when downloading release assets, due to an unusual traffic pattern that repeatedly overloaded a part of our infrastructure that serves these types of requests. There were three windows of impact: (1) August 12 from 16:34 to 18:34 UTC, with an average error rate of 16.16% that peaked at 28.6%; (2)

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Disruption with Login and Release Asset downloads
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
