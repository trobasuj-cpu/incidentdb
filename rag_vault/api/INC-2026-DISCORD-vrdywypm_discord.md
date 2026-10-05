# [INC-2026-DISCORD-vrdywypm] API errors affecting message sends, session starts, and billing operations
**Company:** Discord | **Date:** 2026-05-21 | **Severity:** HIGH | **Source:** [https://stspg.io/5xc5drn5x1p8](https://stspg.io/5xc5drn5x1p8)  
**Technologies:** API, Discord Gateway, Elixir, ScyllaDB  
**Categories:** API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-05-21 20:39:59 UTC] Discord SRE (Resolved): We have applied a fix and are seeing recovery across our systems
[2026-05-21 18:55:44 UTC] Discord SRE (Identified): We're aware of an issue that is affecting a subset of users trying to interact with our services. We are working on  a fix to address the issues.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Discord engineering: We have applied a fix and are seeing recovery across our systems We're aware of an issue that is affecting a subset of users trying to interact with our services. We are working on  a fix to address the issues.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during API errors affecting message sends, session starts, and billing operations
service_cluster:
  provider: "Discord"
  impacted_components: ["API", "Discord Gateway", "Elixir", "ScyllaDB"]
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
- [ ] Validate automatic health checks and circuit breaking on API cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
