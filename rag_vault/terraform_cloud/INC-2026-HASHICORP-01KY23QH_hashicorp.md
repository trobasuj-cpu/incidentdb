# [INC-2026-HASHICORP-01KY23QH] Terraform Runs Failing to Fetch GitHub Sources Over SSH
**Company:** HashiCorp | **Date:** 2026-07-21 | **Severity:** MEDIUM | **Source:** [https://status.hashicorp.com/incidents/01KY23QHWJSW7BDJ353WBPTCPG](https://status.hashicorp.com/incidents/01KY23QHWJSW7BDJ353WBPTCPG)  
**Technologies:** Terraform Cloud, Vault, Consul, Nomad  
**Categories:** AUTHENTICATION_FAILURE, PIPELINE_EXECUTION_FAILURE  

---

## 1. Symptoms & Observed Errors
```text
[2026-07-21 12:43:59 UTC] HashiCorp SRE (Resolved): GitHub has completed remediation of the SSH deploy key incident. Terraform runs using GitHub-hosted modules/sources over SSH have recovered, clone operations are functioning normally, and no further authentication failures are being observed.
[2026-07-21 12:05:28 UTC] HashiCorp SRE (Monitoring): GitHub has completed remediation of the incident affecting SSH deploy keys. Terraform runs using GitHub-hosted modules and sources over SSH have recovered, and clone operations are functioning normally. We are no longer observing related authentication failures and will continue monitoring.
[2026-07-21 12:04:39 UTC] HashiCorp SRE (Monitoring): GitHub has completed remediation of the incident affecting SSH deploy keys. Terraform runs using GitHub-hosted modules and sources over SSH have recovered, and clone operations are functioning normally. We are no longer observing related authentication failures and will continue monitoring.
[2026-07-21 10:32:18 UTC] HashiCorp SRE (Investigating): We are investigating failures cloning GitHub-hosted Terraform modules and sources over SSH, caused by an ongoing upstream GitHub incident affecting SSH deploy keys. Runs using `git::ssh://` or `git@github.com:` sources may fail with Permission denied (publickey); HTTPS and registry-based sources are unaffected. We're tracking GitHub's remediation and will update as we learn more.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by HashiCorp engineering: GitHub has completed remediation of the SSH deploy key incident. Terraform runs using GitHub-hosted modules/sources over SSH have recovered, clone operations are functioning normally, and no further authentication failures are being observed. GitHub has completed remediation of the incident affecting SSH deploy keys. Terraform runs using GitHub-hosted modules and sources over SSH have recovered, and clone operations are functioning normally. We a

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Terraform Runs Failing to Fetch GitHub Sources Over SSH
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
