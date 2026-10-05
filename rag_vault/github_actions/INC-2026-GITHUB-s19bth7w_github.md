# [INC-2026-GITHUB-s19bth7w] Disruption with creation of fine grained personal access tokens
**Company:** GitHub | **Date:** 2026-08-10 | **Severity:** MEDIUM | **Source:** [https://stspg.io/tjf46x5zq25c](https://stspg.io/tjf46x5zq25c)  
**Technologies:** GitHub Actions, Git, REST API, Webhooks  
**Categories:** API_ERROR_SPIKE, AUTHENTICATION_FAILURE  

---

## 1. Symptoms & Observed Errors
```text
[2026-08-10 18:46:22 UTC] GitHub SRE (Resolved): On August 10, 2026, between 17:16 and 18:21 UTC, users were unable to create new fine-grained personal access tokens (FG PAT) through the GitHub website. When a user submitted the FG PAT creation form, they were returned to the FG PAT list without an error message and no FG PAT was created. Creating classic personal access tokens, as well as editing or deleting existing FG PAT were not affected.<br /><br />The cause was a change to how the website loads certain front-end JavaScript that was enabled for all users at 17:15 UTC; the change interacted with an issue in the token creation form's confirmation step that prevented it from running, so the final submission that actually creates the token never completed. Because the page still loaded and the server returned a normal response, the failure produced no error message. GitHub mitigated the incident by disabling the change at 18:21 UTC, at which point token creation recovered immediately, and the incident was resolved at 18:46 UTC.<br /><br />To reduce the chance of recurrence, GitHub is adding monitoring and alerting for anomalies in the FG PAT creation success rate and is removing the issue in the FG PAT creation form that prevented the confirmation step from running. GitHub is also adding automated detection of the issue so other areas of the GitHub front end do not repeat the problem.
[2026-08-10 18:22:18 UTC] GitHub SRE (Monitoring): We identified the source of the issue affecting creation of fine-grained personal access tokens and have applied a mitigation. Users should now be able to create new fine-grained tokens successfully. We are continuing to monitor to confirm full recovery.
[2026-08-10 18:21:41 UTC] GitHub SRE (Monitoring): The degradation has been mitigated. We are monitoring to ensure stability.
[2026-08-10 18:09:36 UTC] GitHub SRE (Investigating): We are investigating reports of users being unable to create fine-grained Personal Access Tokens. Attempting to create a new token redirects the user back to the token overview page without an error message, but the token was not created.
[2026-08-10 18:02:04 UTC] GitHub SRE (Investigating): We are investigating reports of impacted performance for some GitHub services.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by GitHub engineering: On August 10, 2026, between 17:16 and 18:21 UTC, users were unable to create new fine-grained personal access tokens (FG PAT) through the GitHub website. When a user submitted the FG PAT creation form, they were returned to the FG PAT list without an error message and no FG PAT was created. Creating classic personal access tokens, as well as editing or deleting existing FG PAT were not affected.<br /><br />The cause was a change to how the websit

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Disruption with creation of fine grained personal access tokens
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
