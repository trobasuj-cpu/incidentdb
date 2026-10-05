# [INC-2017-GITLAB-01] Catastrophic Database Directory Deletion & Secondary Replication Stagnation
**Company:** GitLab | **Date:** 2017-01-31 | **Severity:** CRITICAL  
**Technologies:** PostgreSQL, PgBouncer, LVM, Linux  
**Categories:** DATA_LOSS, REPLICATION_FAILURE, OPERATOR_ERROR, DATABASE  

---

## 1. Symptoms & Observed Errors
```text
FATAL: database system was not properly shut down; automatic recovery in progress
ERROR: could not locate a valid checkpoint record
LOG: invalid record length at 0/1A0000B8: wanted 24, got 0
FATAL: could not start WAL streaming from primary server: ERROR: replication slot 'db2' does not exist
pg_basebackup: could not connect to server: Connection refused
```

## 2. Root Cause Analysis
During elevated load and spammed issue creation, PostgreSQL replica servers lagged severely. An on-call engineer attempting to wipe a stale replica data directory manually executed 'rm -rf' while connected to the primary production database host (db1.staging versus db1.cluster). Compounding the disaster, existing automated backup cron jobs had silently failed due to version mismatches, requiring 18 hours of forensic disk recovery from temporary LVM snapshots.

## 3. Breaking Configuration / Problematic Code
```
# Dangerous manual shell session on db1 production host:
db1.cluster.gitlab.com:~$ sudo rm -rvf /var/opt/gitlab/postgresql/data/*
# Command stripped the active WAL and cluster data directory on primary node
```

## 4. Remediation Patch / Corrected Configuration
```
# 1. Enforce strict terminal prompts with production environment guards
export PS1='\[\033[01;31m\][PRODUCTION-DATABASE-CRITICAL] \u@\h:\w\$ \[\033[00m\]'
# 2. Apply immutable system flags to database data directories
chattr +i /var/opt/gitlab/postgresql/data
# 3. Configure automated daily WAL archiving with verifiable S3 heartbeat
archive_mode = on
archive_command = 'wal-g wal-push %p'
archive_timeout = 60
```

## 5. Prevention & Hardening Checklist
- [ ] Implement automated backup restoration dry-runs every 24 hours in CI sandbox
- [ ] Revoke direct interactive root SSH shell access to primary relational database nodes
- [ ] Use physical database replication safeguards and strict destructive command confirmation wrappers
