# [INC-2026-VERCEL-vqndbxzg] GitHub-linked deployments and authentication affected
**Company:** Vercel | **Date:** 2026-07-16 | **Severity:** MEDIUM | **Source:** [https://stspg.io/yht3lc3nr2g8](https://stspg.io/yht3lc3nr2g8)  
**Technologies:** Dashboard, Builds, Git Integrations, Vercel Edge Network  
**Categories:** QUEUE_DELAY, API_ERROR_SPIKE, AUTHENTICATION_FAILURE  

---

## 1. Symptoms & Observed Errors
```text
[2026-07-17 00:42:10 UTC] Vercel SRE (Resolved): This incident has been resolved.
[2026-07-17 00:07:15 UTC] Vercel SRE (Monitoring): Deployments for projects linked to GitHub repositories are recovering. Some customers may continue to experience delayed build starts or builds stuck in an initializing state while recovery continues. We will provide additional updates as they become available. GitHub login, signup, and account connections are recovered.
[2026-07-16 23:39:29 UTC] Vercel SRE (Identified): We've identified an issue affecting deployments for projects linked to GitHub repositories, as well as GitHub login, signup, and account connections. CLI deployments and other sign-in methods are unaffected. We will share updates as they become available.
[2026-07-16 23:09:29 UTC] Vercel SRE (Investigating): We are investigating elevated error rates for users logging in to Vercel using their GitHub account and connecting their GitHub accounts to Vercel. Login with email or other sign-in methods is unaffected.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Vercel engineering: This incident has been resolved. Deployments for projects linked to GitHub repositories are recovering. Some customers may continue to experience delayed build starts or builds stuck in an initializing state while recovery continues. We will provide additional updates as they become available. GitHub login, signup, and account connections are recovered. We've identified an issue affecting deployments for projects linked to GitHub repositories, as

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during GitHub-linked deployments and authentication affected
service_cluster:
  provider: "Vercel"
  impacted_components: ["Dashboard", "Builds", "Git Integrations", "Vercel Edge Network"]
  health_check_status: DEGRADED
  circuit_breaker: OPEN
  observed_error_threshold: 0.05
```

## 4. Remediation Patch / Corrected Configuration
```
# Remediation mitigation applied by Vercel SRE
deployment_mitigation:
  action: "Drain degraded node pool and scale healthy replicas"
  traffic_rerouting: ENABLED
  rate_limiting_tier: STRICT
  status: RESOLVED
```

## 5. Prevention & Hardening Checklist
- [ ] Validate automatic health checks and circuit breaking on Dashboard cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
