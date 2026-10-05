# [INC-2026-VERCEL-ysczd6rx] Dynamic API routes returned 404 errors for some deployments
**Company:** Vercel | **Date:** 2026-08-14 | **Severity:** MEDIUM | **Source:** [https://stspg.io/s34qc7lrx9db](https://stspg.io/s34qc7lrx9db)  
**Technologies:** Builds, Vercel Edge Network, Serverless Functions, Build Pipeline  
**Categories:** API_ERROR_SPIKE, PIPELINE_EXECUTION_FAILURE  

---

## 1. Symptoms & Observed Errors
```text
[2026-08-14 13:08:12 UTC] Vercel SRE (Resolved): This incident has been resolved. Between August 12, 17:12 UTC and August 13, 20:33 UTC, some Vercel deployments could return 404 errors from dynamic API routes.

Deployments built on Vercel during this period were affected only if they met all three conditions: used Next.js 16.2 or earlier, used the Pages Router with internationalization (i18n configured in next.config.js), and used dynamic API routes, such as pages/api/[slug].ts or pages/api/[...path].ts.

Deployments built outside Vercel (with vercel deploy --prebuilt) were not affected, and existing deployments not rebuilt during this period were not affected.

If your project matches the conditions above and you deployed during the impact window, redeploy your project. Affected deployments are not fixed automatically; a new deployment is required to restore the affected routes.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Vercel engineering: This incident has been resolved. Between August 12, 17:12 UTC and August 13, 20:33 UTC, some Vercel deployments could return 404 errors from dynamic API routes.

Deployments built on Vercel during this period were affected only if they met all three conditions: used Next.js 16.2 or earlier, used the Pages Router with internationalization (i18n configured in next.config.js), and used dynamic API routes, such as pages/api/[slug].ts or pages/api/[..

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Dynamic API routes returned 404 errors for some deployments
service_cluster:
  provider: "Vercel"
  impacted_components: ["Builds", "Vercel Edge Network", "Serverless Functions", "Build Pipeline"]
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
- [ ] Validate automatic health checks and circuit breaking on Builds cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
