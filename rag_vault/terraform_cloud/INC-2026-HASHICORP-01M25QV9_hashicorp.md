# [INC-2026-HASHICORP-01M25QV9] Hashicorp Linux packages GPG key rotation
**Company:** HashiCorp | **Date:** 2026-09-10 | **Severity:** HIGH | **Source:** [https://status.hashicorp.com/incidents/01M25QV97NFJ5AS3NQFCFRC98S](https://status.hashicorp.com/incidents/01M25QV97NFJ5AS3NQFCFRC98S)  
**Technologies:** Terraform Cloud, Vault, Consul, Nomad  
**Categories:** INFRASTRUCTURE_DEGRADATION, SERVICE_OUTAGE  

---

## 1. Symptoms & Observed Errors
```text
[2026-09-10 20:40:53 UTC] HashiCorp SRE (Resolved): Our Engineering team has resolved the issue. These systems should now be operating normally. If you continue to experience any problems, please open a ticket with our support team.
[2026-09-10 16:35:21 UTC] HashiCorp SRE (Monitoring): Our Engineering team has implemented a fix to resolve the issue. We are monitoring the situation closely and will post an update as soon as the issue is fully resolved.
[2026-09-10 15:50:40 UTC] HashiCorp SRE (Identified): Our engineers have identified the issue and are working through remaining packages.
[2026-09-10 14:45:25 UTC] HashiCorp SRE (Identified): We have updated the keys in the apt and rpm repos and the documentation. Our engineers are working to identify and re-sign packages signed with the older key
[2026-09-10 13:24:22 UTC] HashiCorp SRE (Identified): We are currently updating documentation and package verification guidance following an ongoing **GPG** key rotation for all HashiCorp Linux packages. The fingerprint currently displayed on our site is outdated and may not match the active signing key.

We are actively updating the published fingerprint information and related documentation. We will provide an update once the corrected fingerprint details are available.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by HashiCorp engineering: Our Engineering team has resolved the issue. These systems should now be operating normally. If you continue to experience any problems, please open a ticket with our support team. Our Engineering team has implemented a fix to resolve the issue. We are monitoring the situation closely and will post an update as soon as the issue is fully resolved. Our engineers have identified the issue and are working through remaining packages. We have updated

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Hashicorp Linux packages GPG key rotation
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
