# [INC-2022-POSTGRES-01] Database Outage via Automated Transaction ID Wraparound Shutdown (Emergency Vacuum Starvation)
**Company:** Fintech Global | **Date:** 2022-09-05 | **Severity:** CRITICAL  
**Technologies:** PostgreSQL, Linux, WAL, PgBouncer  
**Categories:** DATABASE, TRANSACTION_ID_WRAPAROUND, VACUUM_STARVATION  

---

## 1. Symptoms & Observed Errors
```text
WARNING: database 'trading_db' must be vacuumed within 10000000 transactions
ERROR: database is not accepting commands to avoid wraparound data loss in database 'trading_db'
HINT: Stop the postmaster and use a standalone backend to run VACUUM in database 'trading_db'.
FATAL: could not start vacuum: transaction limit exceeded
All user queries rejected with error code 57P01 (admin_shutdown).
```

## 2. Root Cause Analysis
A heavily updated order-matching ledger accumulated dead tuples faster than the default autovacuum worker could process them. Autovacuum was continuously canceled by long-running analytical queries. The database's transaction age exceeded the autovacuum_freeze_max_age ceiling (2 billion transactions). To protect against silent data loss from transaction ID wraparound, PostgreSQL executed an emergency hard shutdown into read-only freeze mode.

## 3. Breaking Configuration / Problematic Code
```
# Defective postgresql.conf with under-provisioned autovacuum:
autovacuum = on
autovacuum_max_workers = 3
autovacuum_vacuum_cost_limit = 200  # Stifled disk I/O budget throttling autovacuum progress
autovacuum_vacuum_cost_delay = 20ms
statement_timeout = 0 # Allowed analytical queries to block autovacuum for hours
```

## 4. Remediation Patch / Corrected Configuration
```
# Emergency recovery: Single-user mode freeze execution
# postgres --single -D /var/lib/postgresql/data -d 1 trading_db <<< "VACUUM FREEZE ANALYZE;"

# Tuned production configuration preventing freeze starvation:
autovacuum_max_workers = 8
autovacuum_vacuum_cost_limit = 2000 # High I/O budget for vacuuming
autovacuum_vacuum_cost_delay = 2ms
autovacuum_vacuum_scale_factor = 0.05
statement_timeout = 30000 # Terminate queries older than 30s to permit table locking
```

## 5. Prevention & Hardening Checklist
- [ ] Monitor maximum table transaction ID age with critical alerting at 1 billion transactions (50% of threshold)
- [ ] Enforce strict statement_timeout on all read replicas to prevent blocking table locks
- [ ] Tune autovacuum cost delay to allow aggressive background freezing on write-heavy tables
