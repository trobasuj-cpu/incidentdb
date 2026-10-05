# [INC-2017-AWS-01] US-EAST-1 Regional S3 Outage via Unbounded Index Server Decommissioning Script
**Company:** Amazon Web Services | **Date:** 2017-02-28 | **Severity:** CRITICAL  
**Technologies:** AWS S3, Placement Group, DNS, C++  
**Categories:** OPERATOR_ERROR, STORAGE_SUBSYSTEM, INDEX_CORRUPTION  

---

## 1. Symptoms & Observed Errors
```text
503 Service Unavailable: Slow Down
The server is currently unable to handle the request due to a temporary overloading or maintenance of the server.
S3 API Error: InternalError - We encountered an internal error. Please try again.
DNS SERVFAIL: s3.us-east-1.amazonaws.com does not resolve
CloudWatch Alarm: S3High5xxErrorCount in us-east-1 triggered (Value: 99.8%)
```

## 2. Root Cause Analysis
An authorized operator executing an operational playbook to decommission a small number of billing index servers entered an unintended parameter argument. The decommission utility took down a massive fraction of the primary index servers and placement subsystem for the entire us-east-1 region. The subsystems required a full cold restart, during which they had to replay massive transaction journals before accepting write or read requests.

## 3. Breaking Configuration / Problematic Code
```
# Operator intended to remove servers matching billing partition:
./admin_tool --decommission-index-servers --pool billing --count 4
# Subsystem CLI parser error executed unbounded deletion across primary storage tier:
./admin_tool --decommission-index-servers --pool primary --count 400
```

## 4. Remediation Patch / Corrected Configuration
```
# Implementation of mandatory capacity percentage limits in admin utility
def decommission_servers(pool: str, count: int, max_pct: float = 0.05) -> None:
    current_capacity = get_pool_size(pool)
    max_allowed = int(current_capacity * max_pct)
    if count > max_allowed:
        raise ValueError(f"Decommission request {count} exceeds safety cap of {max_allowed} (5% of pool)")
    execute_safe_decommission(pool, count)
```

## 5. Prevention & Hardening Checklist
- [ ] Constrain all administrative tooling with hard caps preventing more than 5% capacity decommission
- [ ] Implement two-operator dual-authorization validation for infrastructure modifications
- [ ] Re-architect storage index restart procedures into parallelized decoupled partitions
