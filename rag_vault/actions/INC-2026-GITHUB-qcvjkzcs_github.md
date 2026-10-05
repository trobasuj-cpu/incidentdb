# [INC-2026-GITHUB-qcvjkzcs] Incident with Actions
**Company:** GitHub | **Date:** 2026-08-06 | **Severity:** CRITICAL | **Source:** [https://stspg.io/rcz3fcm83sff](https://stspg.io/rcz3fcm83sff)  
**Technologies:** Actions, Pages, GitHub Actions, Git  
**Categories:** QUEUE_DELAY, API_ERROR_SPIKE, PIPELINE_EXECUTION_FAILURE  

---

## 1. Symptoms & Observed Errors
```text
[2026-08-07 02:04:44 UTC] GitHub SRE (Resolved): On August 6, 2026, between 15:05 UTC and 00:14 UTC on August 7, GitHub Actions experienced degraded availability. During the incident, workflow runs failed or remained queued for an extended period of time. Customers using both GitHub-hosted and self-hosted runners were affected. At peak, 71% of workflow runs experienced infrastructure failures and 75% of the remaining workflow runs were delayed by more than 5 minutes. <br /><br />The incident was triggered by a routine deployment to an internal Actions service responsible for processing events and generating Actions jobs. The deployment exposed an existing capacity and concurrency weakness. As pods were replaced during the deployment, remaining capacity became saturated, causing services to crash and triggering a cascading impact across multiple clusters and downstream services. <br /><br />These services recovered at 17:00 after expanding capacity, throttling incoming webhook-triggered work to allow the system to recover, and increasing processing capacity for the backlog of affected events. <br /><br />As the incident progressed, a backlog of work accumulated across the systems responsible for assigning jobs to runners. Due to a latent bug in one of the services responsible for job assignment, runners were getting assigned jobs that were no longer valid and then getting stuck retrying those jobs, preventing them from picking up valid work. <br /><br />This second stage of impact was mitigated by deploying changes to prevent runners from repeatedly attempting to acquire invalid jobs. These mitigations allowed the accumulated queues to drain and Actions to recover to normal operation. <br /><br />Some Actions Runner Controller (ARC) runners remained stuck after the incident. A mitigation deployed during the incident inadvertently affected these runners, causing some to remain offline until they were manually recovered. We subsequently rolled back the change and are adding automatic recovery in upcoming Runner and ARC releases. <br /><br />Some jobs created during the incident were also left stuck unable to be retried or canceled.  CLI and UI solutions for customers to address these were shared at https://github.com/orgs/community/discussions/204152#discussioncomment-17946043. <br /><br />To prevent recurrence, we are making improvements to deployment and capacity safeguards for the affected services, strengthening monitoring for the conditions that preceded the incident, improving the resiliency and recovery of queued work and runner assignment, and adding automatic recovery for self-hosted runners affected by similar failure conditions. We are also making additional improvements to reduce the risk of cascading failures and accelerate recovery during large-scale Actions disruptions.
[2026-08-07 02:03:41 UTC] GitHub SRE (Monitoring): During the incident, some Actions Runner Controller (ARC) runner pods became stuck in an idle state. Affected users can delete those pods using kubectl or redeploy their Actions Runner Controller application. ARC will automatically create replacement runners.<br /><br />The next releases of Actions Runner and Actions Runner Controller will include an automatic recovery mechanism, preventing the need for these manual steps in the future.<br /><br />Some workflow-triggering events, including push and pull request events, were not processed during the incident and cannot be replayed automatically. Customers may need to repeat the triggering action by pushing a new commit, updating the pull request, or manually re-running the workflow where applicable.
[2026-08-07 00:59:01 UTC] GitHub SRE (Monitoring): We’re investigating reports that some Actions Runner Controller runners are taking longer than expected to recover. We’ll provide an update as our investigation progresses.
[2026-08-07 00:06:24 UTC] GitHub SRE (Monitoring): The degradation has been mitigated. We are monitoring to ensure stability.
[2026-08-07 00:05:05 UTC] GitHub SRE (Investigating): The degradation affecting Actions and Pages has been mitigated. We are monitoring to ensure stability.
[2026-08-07 00:01:26 UTC] GitHub SRE (Investigating): System-wide queues have been drained, and new jobs are being processed as expected. The fix for self-hosted runners not picking up jobs has been fully rolled out.<br /><br />Webhook-triggered Actions workflows have been restored to full throughput. GitHub Pages, Copilot code review, and Copilot coding agent are showing recovery. Migrations using GitHub Enterprise Importer remain paused as a precaution.<br /><br />We are monitoring all affected services for sustained recovery and will provide another update shortly.
[2026-08-07 00:01:15 UTC] GitHub SRE (Investigating): <br />System-wide queues have been drained, and new jobs are being processed as expected. The fix for self-hosted runners not picking up jobs has been fully rolled out.<br /><br />Webhook-triggered Actions workflows have been restored to full throughput. GitHub Pages, Copilot code review, and Copilot coding agent are showing recovery. Migrations using GitHub Enterprise Importer remain paused as a precaution.<br /><br />We are monitoring all affected services for sustained recovery and will provide another update shortly.
[2026-08-06 23:13:30 UTC] GitHub SRE (Investigating): We have deployed fixes that address runners being assigned invalid jobs and are taking additional steps to clear the backlog of affected jobs. Job completion rates for running workflows have improved significantly, with success rates now at 99%. Global queues for hosted runner assignment are nearly burned down and concurrency queues for customers are being processed. Another change was deployed to accelerate processing the backlog of job requests.<br /><br />We are gradually restoring throughput for webhook-triggered Actions workflows and monitoring system stability. We have deployed a fix for self-hosted runners that were not picking up jobs and are enabling it incrementally.<br /><br />GitHub Pages, Copilot code review, and Copilot coding agent may still experience intermittent failures or delays. Migrations using GitHub Enterprise Importer remain paused.<br /><br />We continue to monitor recovery across all affected services and will provide another update as conditions improve.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by GitHub engineering: On August 6, 2026, between 15:05 UTC and 00:14 UTC on August 7, GitHub Actions experienced degraded availability. During the incident, workflow runs failed or remained queued for an extended period of time. Customers using both GitHub-hosted and self-hosted runners were affected. At peak, 71% of workflow runs experienced infrastructure failures and 75% of the remaining workflow runs were delayed by more than 5 minutes. <br /><br />The incident wa

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Incident with Actions
service_cluster:
  provider: "GitHub"
  impacted_components: ["Actions", "Pages", "GitHub Actions", "Git"]
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
- [ ] Validate automatic health checks and circuit breaking on Actions cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
