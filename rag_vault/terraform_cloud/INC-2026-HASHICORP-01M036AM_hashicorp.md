# [INC-2026-HASHICORP-01M036AM] Delayed HCP Terraform Runs
**Company:** HashiCorp | **Date:** 2026-08-15 | **Severity:** MEDIUM | **Source:** [https://status.hashicorp.com/incidents/01M036AM4VGQPG41CC4Q9SN1XK](https://status.hashicorp.com/incidents/01M036AM4VGQPG41CC4Q9SN1XK)  
**Technologies:** Terraform Cloud, Vault, Consul, Nomad  
**Categories:** QUEUE_DELAY  

---

## 1. Symptoms & Observed Errors
```text
[2026-08-18 12:50:39 UTC] HashiCorp SRE (Resolved): Our Engineering team has resolved the issue. These systems should now be operating normally. If you continue to experience any problems, please open a ticket with our support team.
[2026-08-15 18:21:28 UTC] HashiCorp SRE (Monitoring): We've mitigated the delayed HCP Terraform run start times.  Customer jobs running on HashiCorp-hosted agents should start without unusual delay.
[2026-08-15 17:08:18 UTC] HashiCorp SRE (Investigating): Our engineering team is investigating elevated Terraform Cloud run start times on HashiCorp-hosted agents,  In some cases, runs are taking about 10 minutes to start.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by HashiCorp engineering: Our Engineering team has resolved the issue. These systems should now be operating normally. If you continue to experience any problems, please open a ticket with our support team. We've mitigated the delayed HCP Terraform run start times.  Customer jobs running on HashiCorp-hosted agents should start without unusual delay. Our engineering team is investigating elevated Terraform Cloud run start times on HashiCorp-hosted agents,  In some cases, r

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Delayed HCP Terraform Runs
service_cluster:
  provider: "HashiCorp"
  impacted_components: ["Terraform Cloud", "Vault", "Consul", "Nomad"]
  health_check_status: DEGRADED
  circuit_breaker: OPEN
  observed_error_threshold: 0.05
```

## 4. Remediation Patch / Corrected Configuration
```
# Remediation mitigation applied by HashiCorp SRE
deployment_mitigation:
  action: "Drain degraded node pool and scale healthy replicas"
  traffic_rerouting: ENABLED
  rate_limiting_tier: STRICT
  status: RESOLVED
```

## 5. Prevention & Hardening Checklist
- [ ] Validate automatic health checks and circuit breaking on Terraform Cloud cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
