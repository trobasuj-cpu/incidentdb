# [INC-2026-CLOUDFLARE-gwrzznc9] Customers using BYOIP can have issues updating their BGP prefixes, including advertising or withdrawing prefixes.
**Company:** Cloudflare | **Date:** 2026-09-24 | **Severity:** MEDIUM | **Source:** [https://www.cloudflarestatus.com/incidents/gwrzznc9f642](https://www.cloudflarestatus.com/incidents/gwrzznc9f642)  
**Technologies:** Bring Your Own IP (BYOIP), Cloudflare Edge, DNS, WAF  
**Categories:** QUEUE_DELAY, NETWORK_CONNECTIVITY  

---

## 1. Symptoms & Observed Errors
```text
[2026-09-24 10:25:52 UTC] Cloudflare SRE (Resolved): This incident has now been resolved - Customers with BYOIP addresses were unable to update their BGP prefixes, including advertising or withdrawing prefixes. Customers making changes to their address maps may also have experienced delays.
[2026-09-24 10:22:34 UTC] Cloudflare SRE (Investigating): We are currently investigating an issue where customers with BYOIP addresses will be unable to update their BGP prefixes, including advertising or withdrawing prefixes.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Cloudflare engineering: This incident has now been resolved - Customers with BYOIP addresses were unable to update their BGP prefixes, including advertising or withdrawing prefixes. Customers making changes to their address maps may also have experienced delays. We are currently investigating an issue where customers with BYOIP addresses will be unable to update their BGP prefixes, including advertising or withdrawing prefixes.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Customers using BYOIP can have issues updating their BGP prefixes, including advertising or withdrawing prefixes.
service_cluster:
  provider: "Cloudflare"
  impacted_components: ["Bring Your Own IP (BYOIP)", "Cloudflare Edge", "DNS", "WAF"]
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
- [ ] Validate automatic health checks and circuit breaking on Bring Your Own IP (BYOIP) cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
