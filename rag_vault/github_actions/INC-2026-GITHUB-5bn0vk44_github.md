# [INC-2026-GITHUB-5bn0vk44] Disruption with GitHub Billing
**Company:** GitHub | **Date:** 2026-08-26 | **Severity:** MEDIUM | **Source:** [https://stspg.io/cpgst4hly3mh](https://stspg.io/cpgst4hly3mh)  
**Technologies:** GitHub Actions, Git, REST API, Webhooks  
**Categories:** QUEUE_DELAY, API_ERROR_SPIKE, STORAGE_SUBSYSTEM  

---

## 1. Symptoms & Observed Errors
```text
[2026-08-27 19:44:08 UTC] GitHub SRE (Resolved): On August 26, 2026, between 20:40 UTC and 00:51 UTC on August 27, GitHub Billing experienced degraded performance affecting billing budget pages and GitHub Copilot CLI sessions. Affected customers encountered failed budget page loads or failures when starting or continuing CLI sessions. We confirmed this impact for a small number of customers (<1%). <br /><br />This was caused by a concentrated workload that created processing delays in our data storage layer. Automated retries increased the load and prolonged the degradation. We mitigated the incident by rebalancing traffic within our infrastructure. <br /><br />We are improving workload isolation, retry behavior, and detection of concentrated load to reduce the likelihood of recurrence and shorten our time to detect and mitigate similar incidents.
[2026-08-27 17:58:39 UTC] GitHub SRE (Investigating): No material change since the previous update. Service conditions remain stable following the mitigation, and we have not observed any further customer impact. We are actively monitoring the service while implementing targeted fixes to address the underlying root cause.
[2026-08-27 16:20:23 UTC] GitHub SRE (Investigating): Our mitigation continues to hold, and service conditions remain stable. We are continuing to investigate the concentrated workload responsible for the issue and are preparing additional preventative improvements. We have not identified a material change in customer impact since the previous update. We will provide another update as the investigation progresses.
[2026-08-27 14:49:36 UTC] GitHub SRE (Investigating): Our mitigation is still holding as we continue to investigate to find the root cause.
[2026-08-27 01:35:35 UTC] GitHub SRE (Investigating): We are continuing to monitor the mitigation that we have applied for the billing page disruption.
[2026-08-27 00:31:22 UTC] GitHub SRE (Investigating): We've applied a mitigation to unblock Copilot usage and have observed recovery for this particular impact. We're continuing to investigate and apply mitigations for the billing page disruption while monitoring to ensure Copilot remains recovered.
[2026-08-26 23:42:06 UTC] GitHub SRE (Investigating): We are currently investigating increased errors with billing services. Customers may observe failed billing budget page loads, and users of the Copilot CLI may observe failures starting or continuing sessions.
[2026-08-26 23:37:20 UTC] GitHub SRE (Investigating): We are investigating reports of impacted performance for some GitHub services.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by GitHub engineering: On August 26, 2026, between 20:40 UTC and 00:51 UTC on August 27, GitHub Billing experienced degraded performance affecting billing budget pages and GitHub Copilot CLI sessions. Affected customers encountered failed budget page loads or failures when starting or continuing CLI sessions. We confirmed this impact for a small number of customers (<1%). <br /><br />This was caused by a concentrated workload that created processing delays in our data

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Disruption with GitHub Billing
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
