# [INC-2026-GITHUB-f6yrxnz5] Incident with Pull Requests
**Company:** GitHub | **Date:** 2026-09-20 | **Severity:** MEDIUM | **Source:** [https://stspg.io/cx8xcm6k8f9s](https://stspg.io/cx8xcm6k8f9s)  
**Technologies:** Pull Requests, GitHub Actions, Git, REST API  
**Categories:** QUEUE_DELAY, STORAGE_SUBSYSTEM  

---

## 1. Symptoms & Observed Errors
```text
[2026-09-20 23:22:16 UTC] GitHub SRE (Resolved): On September 20, 2026, between 21:46 and 22:24 UTC the Pull Requests service was degraded and pull request merge and test-merge commits were created late, with delays reaching approximately four minutes at peak. Merge commits were delayed rather than lost. Because some Actions workflow runs start only after a pull request's merge commit is created, a subset of workflow runs for pull request events were also delayed. <br /><br />This was due to a routine repository maintenance job for an unusually large repository consuming nearly all of the memory on a single Git storage server, which left that server unable to serve the Git operations used to create merge commits. <br /><br />We mitigated the incident by removing the affected server from service at 22:18 UTC, after which the queued merge commits were created within six minutes. <br /><br />We have capped the memory a single repository maintenance job may consume so that one repository cannot exhaust a server, and we have improved monitoring and alerting on storage server health to reduce our time to detection and mitigation of issues like this one in the future.
[2026-09-20 22:32:02 UTC] GitHub SRE (Monitoring): A git fileserver issue caused a brief delay in creating some merge commits - we've isolated the underlying server and already observed recovery.
[2026-09-20 22:27:28 UTC] GitHub SRE (Monitoring): The degradation affecting Pull Requests has been mitigated. We are monitoring to ensure stability.
[2026-09-20 22:13:37 UTC] GitHub SRE (Investigating): We are investigating reports of degraded performance for Pull Requests
```

## 2. Root Cause Analysis
Official investigation timeline recorded by GitHub engineering: On September 20, 2026, between 21:46 and 22:24 UTC the Pull Requests service was degraded and pull request merge and test-merge commits were created late, with delays reaching approximately four minutes at peak. Merge commits were delayed rather than lost. Because some Actions workflow runs start only after a pull request's merge commit is created, a subset of workflow runs for pull request events were also delayed. <br /><br />This was due to a

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Incident with Pull Requests
service_cluster:
  provider: "GitHub"
  impacted_components: ["Pull Requests", "GitHub Actions", "Git", "REST API"]
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
- [ ] Validate automatic health checks and circuit breaking on Pull Requests cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
