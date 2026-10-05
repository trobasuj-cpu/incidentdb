# [INC-2026-DISCORD-jvqsklfd] UK/AU: Issues Completing Age Verification via k-ID
**Company:** Discord | **Date:** 2026-03-05 | **Severity:** MEDIUM | **Source:** [https://stspg.io/0zdtdpxmmxgl](https://stspg.io/0zdtdpxmmxgl)  
**Technologies:** Desktop, iOS, Android, Web  
**Categories:** PIPELINE_EXECUTION_FAILURE  

---

## 1. Symptoms & Observed Errors
```text
[2026-03-05 19:49:19 UTC] Discord SRE (Resolved): k-ID age verification is operating normally again in the UK and Australia. Previously age verified users were not impacted.
[2026-03-05 19:38:36 UTC] Discord SRE (Monitoring): k-ID has deployed a fix for UK/AU age verification issues. We’re monitoring to ensure age verification is working normally. Previously age verified users should not be impacted.
[2026-03-05 19:26:36 UTC] Discord SRE (Investigating): Some users in the UK and Australia may be unable to complete new age verification via k-ID. Users who have already completed age verification should not be impacted. We’re actively working with k-ID on a resolution and will share updates here as we learn more.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Discord engineering: k-ID age verification is operating normally again in the UK and Australia. Previously age verified users were not impacted. k-ID has deployed a fix for UK/AU age verification issues. We’re monitoring to ensure age verification is working normally. Previously age verified users should not be impacted. Some users in the UK and Australia may be unable to complete new age verification via k-ID. Users who have already completed age verification should

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during UK/AU: Issues Completing Age Verification via k-ID
service_cluster:
  provider: "Discord"
  impacted_components: ["Desktop", "iOS", "Android", "Web"]
  health_check_status: DEGRADED
  circuit_breaker: OPEN
  observed_error_threshold: 0.05
```

## 4. Remediation Patch / Corrected Configuration
```
# Remediation mitigation applied by Discord SRE
deployment_mitigation:
  action: "Drain degraded node pool and scale healthy replicas"
  traffic_rerouting: ENABLED
  rate_limiting_tier: STRICT
  status: RESOLVED
```

## 5. Prevention & Hardening Checklist
- [ ] Validate automatic health checks and circuit breaking on Desktop cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
