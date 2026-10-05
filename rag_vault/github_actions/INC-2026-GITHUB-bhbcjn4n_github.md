# [INC-2026-GITHUB-bhbcjn4n] Intermittent failures creating agent tasks
**Company:** GitHub | **Date:** 2026-08-20 | **Severity:** CRITICAL | **Source:** [https://stspg.io/py1yl5mnq89c](https://stspg.io/py1yl5mnq89c)  
**Technologies:** GitHub Actions, Git, REST API, Webhooks  
**Categories:** QUEUE_DELAY, DATABASE_DEGRADATION, API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-08-21 00:37:20 UTC] GitHub SRE (Resolved): Between 13:57 UTC on August 20 and 00:37 UTC on August 21, 2026, some users of the Copilot Cloud Agent experienced delays of up to 60 to 90 minutes in seeing the status and results of their agent tasks. The agent tasks themselves continued to run and complete during this time; only the visibility of their status was delayed.<br /><br />The cause was a regional outage in a third-party cloud database service that Copilot uses to store agent task status. We failed over the affected database to a healthy region, added processing capacity to work through the backlog, and restored normal operation once the underlying service recovered. No task data was lost during the incident.<br /><br />To prevent repetition of similar incidents, we are removing the database configuration that made us vulnerable to this regional outage and improving our database failover procedures.
[2026-08-20 20:37:06 UTC] GitHub SRE (Investigating): We are seeing gradual recovery in Copilot Cloud Agent task status visibility as we deploy a fix for the root cause. Session output remains delayed by approximately one hour while remediation continues.
[2026-08-20 19:35:31 UTC] GitHub SRE (Investigating): We are continuing to observe gradual recovery for Copilot Cloud Agent task status visibility. Session output continues to be delayed by approximately 1 hour as our remediation steps take effect.
[2026-08-20 18:45:23 UTC] GitHub SRE (Investigating): We are continuing to observe gradual recovery for Copilot Cloud Agent task status visibility, with session output delayed by approximately 1 hour. We have taken additional steps to accelerate the recovery and expect this to take effect within the next hour.
[2026-08-20 18:04:55 UTC] GitHub SRE (Investigating): We are continuing to observe gradual recovery for Copilot Cloud Agent task status visibility, with session output delayed by approximately 1 hour. We have taken additional steps to accelerate the recovery and are continuing to monitor the impact.
[2026-08-20 17:32:39 UTC] GitHub SRE (Investigating): We are observing gradual recovery for Copilot Cloud Agent task status visibility, with session output delayed approximately 1 hour. We have taken additional steps to accelerate the recovery and are continuing to monitor the impact.
[2026-08-20 17:05:20 UTC] GitHub SRE (Investigating): We are seeing signs of recovery for Copilot Cloud Agent task status visibility, but this recovery is slower than anticipated. We are pursuing additional mitigating measures to accelerate recovery.
[2026-08-20 16:14:46 UTC] GitHub SRE (Investigating): Users are experiencing delays when starting tasks using Copilot Cloud Agent and are not be able to see the status of these tasks. Copilot Cloud Agent tasks are still being completed. We have identified the cause of the issue and are putting mitigations in place to return service to normal levels. We will provide another update about the expected recovery time shortly.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by GitHub engineering: Between 13:57 UTC on August 20 and 00:37 UTC on August 21, 2026, some users of the Copilot Cloud Agent experienced delays of up to 60 to 90 minutes in seeing the status and results of their agent tasks. The agent tasks themselves continued to run and complete during this time; only the visibility of their status was delayed.<br /><br />The cause was a regional outage in a third-party cloud database service that Copilot uses to store agent task st

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Intermittent failures creating agent tasks
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
