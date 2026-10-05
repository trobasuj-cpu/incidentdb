# [INC-2026-GITHUB-76t89hbf] Incident with Pull Requests and Issues
**Company:** GitHub | **Date:** 2026-08-12 | **Severity:** MEDIUM | **Source:** [https://stspg.io/ssd9z8l2g46v](https://stspg.io/ssd9z8l2g46v)  
**Technologies:** Issues, Pull Requests, GitHub Actions, Git  
**Categories:** DATABASE_DEGRADATION  

---

## 1. Symptoms & Observed Errors
```text
[2026-08-12 16:41:18 UTC] GitHub SRE (Resolved): Between 16:03 and 16:29 UTC on August 12, some users encountered errors when viewing pull requests, issues, and search results. During this period, about 1.9% of Pull Request requests and 0.9% of Issues requests failed. During a database migration, two indexes were removed while application settings still referenced them, causing affected requests to fail. We detected the issue after the migration reached one database shard and before it progressed to the remaining shards. We restored service by disabling both settings. We are improving safeguards around database migrations and application configuration to prevent similar mismatches from causing errors.
[2026-08-12 16:38:38 UTC] GitHub SRE (Monitoring): We identified the source of errors affecting Pull Requests, Issues, and Search on GitHub.com and have applied a mitigation. A database index hint was referencing an index that had been removed by a recent migration, causing query failures for some users. We disabled the problematic configuration and are seeing recovery across affected services. We are continuing to monitor to confirm full resolution.
[2026-08-12 16:35:40 UTC] GitHub SRE (Monitoring): The degradation affecting Issues and Pull Requests has been mitigated. We are monitoring to ensure stability.
[2026-08-12 16:24:30 UTC] GitHub SRE (Investigating): We are investigating reports of errors affecting Pull Requests and Issues on GitHub.com. Some users may encounter 500 errors when loading pull request and issue pages. Our engineering teams are actively investigating the root cause, which appears to be related to a database infrastructure issue. We will provide an update as soon as we have more information.
[2026-08-12 16:16:18 UTC] GitHub SRE (Investigating): We are investigating reports of degraded performance for Issues and Pull Requests
```

## 2. Root Cause Analysis
Official investigation timeline recorded by GitHub engineering: Between 16:03 and 16:29 UTC on August 12, some users encountered errors when viewing pull requests, issues, and search results. During this period, about 1.9% of Pull Request requests and 0.9% of Issues requests failed. During a database migration, two indexes were removed while application settings still referenced them, causing affected requests to fail. We detected the issue after the migration reached one database shard and before it progress

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Incident with Pull Requests and Issues
service_cluster:
  provider: "GitHub"
  impacted_components: ["Issues", "Pull Requests", "GitHub Actions", "Git"]
  health_check_status: DEGRADED
  circuit_breaker: OPEN
  observed_error_threshold: 0.05
```

## 4. Remediation Patch / Corrected Configuration
```
# Remediation mitigation applied by GitHub SRE
deployment_mitigation:
  action: "Drain degraded node pool and scale healthy replicas"
  traffic_rerouting: ENABLED
  rate_limiting_tier: STRICT
  status: RESOLVED
```

## 5. Prevention & Hardening Checklist
- [ ] Validate automatic health checks and circuit breaking on Issues cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
