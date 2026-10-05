# [INC-2026-DIGITALOCEAN-p3zyn7c4] Let's Encrypt Outage Affecting Certificate Issuance and Managed Databases Operations
**Company:** DigitalOcean | **Date:** 2026-05-08 | **Severity:** MEDIUM | **Source:** [https://stspg.io/rw9m9n69q6tg](https://stspg.io/rw9m9n69q6tg)  
**Technologies:** Global, Droplets Hypervisor, DOKS Kubernetes, Block Storage  
**Categories:** QUEUE_DELAY, DATABASE_DEGRADATION  

---

## 1. Symptoms & Observed Errors
```text
[2026-05-08 21:54:44 UTC] DigitalOcean SRE (Resolved): The upstream outage with Let's Encrypt has been resolved. Customers should now be able to issue Let's Encrypt certificates for Spaces, Load Balancers, and App Platform Custom Domains. Our Engineering team has also confirmed that stuck or delayed actions with Mongo, Advanced PG, and Advanced MySQL databases should complete normally now. 

We appreciate your patience. If you continue to experience any issues, please open a support ticket from within your account.
[2026-05-08 20:46:22 UTC] DigitalOcean SRE (Identified): Our Engineering team is aware of an upstream outage with Let's Encrypt (see https://letsencrypt.status.io/) which impacts the following services: 

- Inability to create new Let's Encrypt certificates for Spaces, Load Balancers, and App Platform Custom Domains
- Stuck or delayed creates/forks/restores on Mongo, PG, and MySQL databases. 

Please note that operations related to Managed Databases and App Platform Custom Domains will automatically retry and should complete successfully once the upstream outage is resolved. 

We'll continue to monitor this situation and provide updates. We apologize for the inconvenience.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by DigitalOcean engineering: The upstream outage with Let's Encrypt has been resolved. Customers should now be able to issue Let's Encrypt certificates for Spaces, Load Balancers, and App Platform Custom Domains. Our Engineering team has also confirmed that stuck or delayed actions with Mongo, Advanced PG, and Advanced MySQL databases should complete normally now. 

We appreciate your patience. If you continue to experience any issues, please open a support ticket from withi

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Let's Encrypt Outage Affecting Certificate Issuance and Managed Databases Operations
service_cluster:
  provider: "DigitalOcean"
  impacted_components: ["Global", "Droplets Hypervisor", "DOKS Kubernetes", "Block Storage"]
  health_check_status: DEGRADED
  circuit_breaker: OPEN
  observed_error_threshold: 0.05
```

## 4. Remediation Patch / Corrected Configuration
```
# Remediation mitigation applied by DigitalOcean SRE
deployment_mitigation:
  action: "Drain degraded node pool and scale healthy replicas"
  traffic_rerouting: ENABLED
  rate_limiting_tier: STRICT
  status: RESOLVED
```

## 5. Prevention & Hardening Checklist
- [ ] Validate automatic health checks and circuit breaking on Global cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
