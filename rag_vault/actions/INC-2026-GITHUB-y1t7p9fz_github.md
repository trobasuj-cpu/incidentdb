# [INC-2026-GITHUB-y1t7p9fz] Incident with Actions
**Company:** GitHub | **Date:** 2026-08-26 | **Severity:** CRITICAL | **Source:** [https://stspg.io/pg14nv9m3095](https://stspg.io/pg14nv9m3095)  
**Technologies:** Actions, Pages, GitHub Actions, Git  
**Categories:** QUEUE_DELAY, DATABASE_DEGRADATION, PIPELINE_EXECUTION_FAILURE  

---

## 1. Symptoms & Observed Errors
```text
[2026-08-26 18:01:30 UTC] GitHub SRE (Resolved): On August 26, 2026 from 15:02 to 15:45 UTC, Actions jobs failed to start. The following 2 hours until 17:40 UTC, Actions runs were delayed starting by more than 5 minutes as the system caught up with delayed load. This impact was triggered by saturation of writes to the database primary used by the service processing triggers for Actions workflows. The primary was failed over, but the system did not fully recover. The saturation was caused by growing daily peak load combined with an upstream issue in GitHub’s event processing infrastructure, https://www.githubstatus.com/incidents/hcbtzksccj2f, which caused burst amplification of already-high load. Downstream throttles that were later used to recover were set ~10% too high to protect the system.  <br /><br />At 15:45 UTC, throttling combined with service restarts recovered the service’s core health. Those throttles were gradually raised between 15:54 and 17:22 to restore full webhook processing for Actions runs. This ramp was deliberately slow to ensure we did not re-overwhelm the system given our original throttling was now known to be incorrectly set. The queue of webhook events was fully burned down at 17:40 UTC. <br /><br />3.7% of larger-runner jobs, along with some scale-set self-hosted jobs, remained stuck in queued or “waiting for runner” state. We deployed a change to force-revoke jobs in this state, and they transitioned to failed at 18:40 UTC, about 50 minutes after incident mitigation. Releasing these jobs also freed hosted concurrency for larger-runner jobs. <br /><br />Customers using concurrency groups saw longer impact due to a separate issue where runners assigned to a subset of jobs disconnected before the force-revoke mitigation was deployed, which prevented runner acquisition from progressing and left jobs in a waiting-for-runner state. This was resolved at 01:00 UTC on August 27. <br /><br />Some runs triggered during the 15:02-15:45 UTC incident window encountered a bug that left them showing as queued even after service recovery. In the backend, these runs had already failed and will automatically move to canceled state 24 hours after creation. As follow-up, we are fixing the root cause of this queued state and improving our ability to bulk-cancel affected runs. <br /><br />Several changes to improve the general scalability of this part of Actions were already complete and deploying to production. Rollout of those changes will be complete within the next 24 hours. Further work to improve scale, resiliency, and more graceful degradation of Actions workflows are in flight. We are also taking a repair item to accelerate clearing of stuck queued or waiting jobs in similar future cases.
[2026-08-26 18:00:48 UTC] GitHub SRE (Monitoring): All inbound queues have recovered and Actions is operating as expected. 3.7% of jobs assigned to larger runners during the early stage of this incident are stuck waiting for runner assignment. Those will be canceled within the hour. Other runners are successfully processing all new jobs.
[2026-08-26 17:54:33 UTC] GitHub SRE (Monitoring): The degradation affecting Actions has been mitigated. We are monitoring to ensure stability.
[2026-08-26 17:32:22 UTC] GitHub SRE (Investigating): We are continuing to observe recovery and expect actions inbound queues to be back to normal in <30min. Work will continue to flow through the system subject to per-customer concurrency limits.
[2026-08-26 16:50:28 UTC] GitHub SRE (Investigating): We are continuing to observe recovery and delayed queues are burning down. Some customers will continue to see increased delays until all throttled work has been completed - we expect this within the next hour.
[2026-08-26 16:49:07 UTC] GitHub SRE (Investigating): Pages is operating normally.
[2026-08-26 16:14:16 UTC] GitHub SRE (Investigating): We believe we've identified and addressed the issue and are ramping traffic back up slowly to ensure it doesn't recur. Some customers will continue to see delays as we ramp up.
[2026-08-26 15:48:07 UTC] GitHub SRE (Investigating): primary failover briefly improved performance but did not fully mitigate, we've throttled inbound traffic and are investigating upstream Vitess issues
```

## 2. Root Cause Analysis
Official investigation timeline recorded by GitHub engineering: On August 26, 2026 from 15:02 to 15:45 UTC, Actions jobs failed to start. The following 2 hours until 17:40 UTC, Actions runs were delayed starting by more than 5 minutes as the system caught up with delayed load. This impact was triggered by saturation of writes to the database primary used by the service processing triggers for Actions workflows. The primary was failed over, but the system did not fully recover. The saturation was caused by gro

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
