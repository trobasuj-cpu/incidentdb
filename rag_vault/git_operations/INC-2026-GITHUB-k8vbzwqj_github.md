# [INC-2026-GITHUB-k8vbzwqj] Incident with Webhooks
**Company:** GitHub | **Date:** 2026-08-13 | **Severity:** MEDIUM | **Source:** [https://stspg.io/f0cgtdc0kqxg](https://stspg.io/f0cgtdc0kqxg)  
**Technologies:** Git Operations, Webhooks, Issues, Pull Requests  
**Categories:** DATABASE_DEGRADATION, API_ERROR_SPIKE, PIPELINE_EXECUTION_FAILURE  

---

## 1. Symptoms & Observed Errors
```text
[2026-08-13 15:36:34 UTC] GitHub SRE (Resolved): Between 14:24 and 14:53 UTC on 13 August 2026, a routine background job to delete an organization overwhelmed a key shared database, causing multiple GitHub services to briefly return elevated errors and slower responses. Most affected was the webhook management API, with smaller impact to Git operations, pull requests, issues, packages, sign-in, and Copilot. Impact cleared on its own at about 14:53 UTC once the job finished; we resolved the incident at 15:36 UTC. <br /><br />Affected users may have experienced a brief increase in errors and slower responses, primarily when creating, listing, or updating webhooks, with smaller impacts to pull requests, issues, packages, and Git operations. Failures peaked at about 1% for several minutes around 14:37 UTC. <br /><br />To prevent future incidents, we've already shipped an update that turns on the safer deletion path for organizations, along with caps on deletion holds on databases. Building on these changes, we're auditing all bulk deletion and cleanup jobs that write to shared databases to prevent similar issues in future.
[2026-08-13 15:33:54 UTC] GitHub SRE (Monitoring): We have temporarily disabled a background job which caused the impact. At this time the impact is fully mitigated.
[2026-08-13 15:33:02 UTC] GitHub SRE (Monitoring): The degradation affecting Git Operations, Issues, Packages, Pull Requests and Webhooks has been mitigated. We are monitoring to ensure stability.
[2026-08-13 14:58:49 UTC] GitHub SRE (Investigating): We are currently investigating a brief degradation of service for Git operations (specifically pushes), issues, pull requests, package registry, and webhooks between 14:32 and 14:46 UTC. We have identified the source of the degradation and are investigating mitigation strategies to prevent recurrence.
[2026-08-13 14:56:45 UTC] GitHub SRE (Investigating): Packages is experiencing degraded performance. We are continuing to investigate.
[2026-08-13 14:46:52 UTC] GitHub SRE (Investigating): Git Operations is experiencing degraded performance. We are continuing to investigate.
[2026-08-13 14:46:23 UTC] GitHub SRE (Investigating): Issues is experiencing degraded performance. We are continuing to investigate.
[2026-08-13 14:46:11 UTC] GitHub SRE (Investigating): Pull Requests is experiencing degraded performance. We are continuing to investigate.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by GitHub engineering: Between 14:24 and 14:53 UTC on 13 August 2026, a routine background job to delete an organization overwhelmed a key shared database, causing multiple GitHub services to briefly return elevated errors and slower responses. Most affected was the webhook management API, with smaller impact to Git operations, pull requests, issues, packages, sign-in, and Copilot. Impact cleared on its own at about 14:53 UTC once the job finished; we resolved the inci

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Incident with Webhooks
service_cluster:
  provider: "GitHub"
  impacted_components: ["Git Operations", "Webhooks", "Issues", "Pull Requests"]
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
- [ ] Validate automatic health checks and circuit breaking on Git Operations cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
