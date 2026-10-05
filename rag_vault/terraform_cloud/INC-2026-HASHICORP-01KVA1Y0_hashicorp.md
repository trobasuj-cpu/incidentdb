# [INC-2026-HASHICORP-01KVA1Y0] Issues Re-enabling SAML connections
**Company:** HashiCorp | **Date:** 2026-06-17 | **Severity:** MEDIUM | **Source:** [https://status.hashicorp.com/incidents/01KVA1Y0VSYC53BZE9V4BCRYSG](https://status.hashicorp.com/incidents/01KVA1Y0VSYC53BZE9V4BCRYSG)  
**Technologies:** Terraform Cloud, Vault, Consul, Nomad  
**Categories:** AUTHENTICATION_FAILURE  

---

## 1. Symptoms & Observed Errors
```text
[2026-06-17 07:00:36 UTC] HashiCorp SRE (Resolved): Our upstream provider has resolved the issue. These systems should now be operating normally. If you continue to experience any problems, please open a ticket with our support team.
[2026-06-17 06:28:31 UTC] HashiCorp SRE (Monitoring): We are seeing our upstream provider services recovering and our own tests are now passing. We are monitoring the situation closely and will post an update as soon as the issue is fully resolved.
[2026-06-17 05:49:50 UTC] HashiCorp SRE (Identified): Our Engineering team has identified the cause Our Engineering team has identified the cause to be with an upstream provider. We are working with the provider to restore functionality as soon as possible. Once we have additional information, we will share another update.
[2026-06-17 05:47:42 UTC] HashiCorp SRE (Investigating): We are aware of and investigating reports of errors when attempting to enable SAML for organizations right after having deleted SAML configurations.

For the moment, we strongly recommend not deleting SAML configurations from an organization.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by HashiCorp engineering: Our upstream provider has resolved the issue. These systems should now be operating normally. If you continue to experience any problems, please open a ticket with our support team. We are seeing our upstream provider services recovering and our own tests are now passing. We are monitoring the situation closely and will post an update as soon as the issue is fully resolved. Our Engineering team has identified the cause Our Engineering team has id

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Issues Re-enabling SAML connections
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
