# [INC-2022-ATLASSIAN-01] Deletion of 775 Cloud Customer Instances via Deprecated App Uninstallation Script
**Company:** Atlassian | **Date:** 2022-04-05 | **Severity:** CRITICAL  
**Technologies:** AWS, Microservices, PostgreSQL, Identity  
**Categories:** DATA_LOSS, OPERATOR_ERROR, MULTI_TENANT_ISOLATION  

---

## 1. Symptoms & Observed Errors
```text
HTTP 404 Not Found: Site 'customer-jira.atlassian.net' does not exist
TenantManager: Received deprovisioning signal for site_id: 104819
AuditLog: Dropping RDS schema 'tenant_104819_data' and revoking IAM roles
Customer Escalation: 775 enterprise enterprise tenants reporting total workspace disappearance.
```

## 2. Root Cause Analysis
An engineering team intended to disable a deprecated legacy application (Insight - Asset Management) on a small set of customer sites. Instead of providing the specific app-entitlement ID to the deprovisioning runner, the operator provided the overarching cloud product site IDs. The runner interpreted the request as a full customer account deletion, dropping relational database schemas, object storage buckets, and access configurations across 775 customer organizations.

## 3. Breaking Configuration / Problematic Code
```
# Operator invocation of internal deprovisioning runner with ambiguous ID list:
run_tenant_cleanup --mode hard_delete --ids-file /tmp/insight_app_users.csv
# The CSV contained root tenant site IDs instead of Insight app subscription IDs
```

## 4. Remediation Patch / Corrected Configuration
```
# Separation of app-uninstallation from tenant deprovisioning APIs:
def deprovision_tenant(site_id: str, confirmation_token: str) -> None:
    assert_explicit_customer_cancellation_verified(site_id)
    # Move to 30-day soft-delete tombstone bucket before destructive schema drop
    quarantine_tenant_assets(site_id, retention_days=30)
```

## 5. Prevention & Hardening Checklist
- [ ] Enforce mandatory 30-day soft-delete tombstoning before executing any irreversible database drops
- [ ] Refactor internal administrative APIs to strictly isolate app-level removal from site-level deprovisioning
- [ ] Implement automated anomaly detection that halts execution if bulk deletion commands exceed 10 records
