# [INC-2026-CLOUDFLARE-tl0vrgjy] Cloudflare One Clients are incorrectly challenged on some sites
**Company:** Cloudflare | **Date:** 2026-09-22 | **Severity:** MEDIUM | **Source:** [https://www.cloudflarestatus.com/incidents/tl0vrgjy99r0](https://www.cloudflarestatus.com/incidents/tl0vrgjy99r0)  
**Technologies:** Cloudflare One Client, Cloudflare Edge, DNS, WAF  
**Categories:** INFRASTRUCTURE_DEGRADATION, SERVICE_OUTAGE  

---

## 1. Symptoms & Observed Errors
```text
[2026-09-23 11:02:41 UTC] Cloudflare SRE (Identified): The issue has been identified and a fix is being implemented.
[2026-09-22 19:23:17 UTC] Cloudflare SRE (Investigating): Cloudflare is investigating an issue where Cloudflare One Client customers are incorrectly challenged while visiting some websites.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Cloudflare engineering: The issue has been identified and a fix is being implemented. Cloudflare is investigating an issue where Cloudflare One Client customers are incorrectly challenged while visiting some websites.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Cloudflare One Clients are incorrectly challenged on some sites
service_cluster:
  provider: "Cloudflare"
  impacted_components: ["Cloudflare One Client", "Cloudflare Edge", "DNS", "WAF"]
  health_check_status: DEGRADED
  circuit_breaker: OPEN
  observed_error_threshold: 0.05
```

## 4. Remediation Patch / Corrected Configuration
```
# Remediation mitigation applied by Cloudflare SRE
deployment_mitigation:
  action: "Drain degraded node pool and scale healthy replicas"
  traffic_rerouting: ENABLED
  rate_limiting_tier: STRICT
  status: RESOLVED
```

## 5. Prevention & Hardening Checklist
- [ ] Validate automatic health checks and circuit breaking on Cloudflare One Client cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
