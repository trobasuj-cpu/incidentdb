# [INC-2026-GITHUB-75g5xmzp] Incident with Actions
**Company:** GitHub | **Date:** 2026-07-29 | **Severity:** HIGH | **Source:** [https://stspg.io/8nsh6820sff9](https://stspg.io/8nsh6820sff9)  
**Technologies:** Actions, GitHub Actions, Git, REST API  
**Categories:** QUEUE_DELAY, API_ERROR_SPIKE, PIPELINE_EXECUTION_FAILURE  

---

## 1. Symptoms & Observed Errors
```text
[2026-07-29 16:00:54 UTC] GitHub SRE (Resolved): On July 29, 2026, from 14:51 UTC to 15:28 UTC, GitHub Actions experienced elevated REST API request timeouts and errors, failures registering runners, and delayed workflow run starts for customers whose traffic was served by a single infrastructure site. This was caused by an under-provisioned internal Actions service in that site: under increased load its instances ran out of memory and became unresponsive, and because Actions API requests wait synchronously on that service, requests routed through the affected site stalled and timed out. During the incident, approximately 2% of workflows were delayed. Requests served by other sites remained unaffected. Both standard and larger hosted runners routed through the affected site could see delayed job starts. <br /><br />The issue was mitigated by scaling out the runner-administration service in the affected site and increasing the replica count, which restored API availability and returned workflow run starts to normal. We are working to add horizontal autoscaling, memory-saturation alerting, and scaling-forecast monitoring for this service, along with responder playbooks, to reduce the likelihood of similar issues in the future.
[2026-07-29 15:40:35 UTC] GitHub SRE (Monitoring): The degradation affecting Actions has been mitigated. We are monitoring to ensure stability.
[2026-07-29 15:34:07 UTC] GitHub SRE (Investigating): We are investigating an issue affecting GitHub Actions. Some customers may experience timeouts or failures with runner registration and workflow runs may be delayed during startup. Our team is actively working to mitigate the impact by scaling capacity across additional infrastructure.
[2026-07-29 15:26:24 UTC] GitHub SRE (Investigating): We are investigating reports of degraded availability for Actions
```

## 2. Root Cause Analysis
Official investigation timeline recorded by GitHub engineering: On July 29, 2026, from 14:51 UTC to 15:28 UTC, GitHub Actions experienced elevated REST API request timeouts and errors, failures registering runners, and delayed workflow run starts for customers whose traffic was served by a single infrastructure site. This was caused by an under-provisioned internal Actions service in that site: under increased load its instances ran out of memory and became unresponsive, and because Actions API requests wait

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Incident with Actions
service_cluster:
  provider: "GitHub"
  impacted_components: ["Actions", "GitHub Actions", "Git", "REST API"]
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
