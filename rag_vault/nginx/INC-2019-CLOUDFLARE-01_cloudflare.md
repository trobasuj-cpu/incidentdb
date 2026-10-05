# [INC-2019-CLOUDFLARE-01] Global Edge Outage via Catastrophic Backtracking in WAF Rule PCRE Engine
**Company:** Cloudflare | **Date:** 2019-07-02 | **Severity:** CRITICAL  
**Technologies:** Nginx, Lua, PCRE, WAF  
**Categories:** CPU_EXHAUSTION, REGEX_CATASTROPHIC_BACKTRACKING, EDGE_INGRESS  

---

## 1. Symptoms & Observed Errors
```text
2019/07/02 13:42:10 [alert] 28192#0: *12049281 worker process 28195 exited on signal 9 (Killed)
2019/07/02 13:42:15 [error] 28192#0: epoll_wait() failed (4: Interrupted system call)
HTTP/1.1 502 Bad Gateway
X-Cloudflare-Ray: 4f0a9182c1992
CPU Utilization on Core 0-31: 100.0% (sys: 99.8%, user: 0.2%)
```

## 2. Root Cause Analysis
A newly deployed managed WAF rule contained a poorly anchored regular expression with nested quantifiers. When evaluated against standard HTTP request headers, the PCRE engine encountered extreme catastrophic backtracking, consuming 100% CPU on all edge Nginx worker cores simultaneously across all global points of presence, starving the event loop and dropping all proxy traffic worldwide.

## 3. Breaking Configuration / Problematic Code
```
# Broken WAF rule deployed to global edge
SecRule REQUEST_URI|ARGS|HEADERS "(?:(?:^|[?&])(?i:x-debug|debug)=(?:1|true))|(?:\?.*=(?:.*\.(?:js|css)))*.*$" \\
    "id:100013,phase:2,t:none,t:lowercase,deny,status:403"
# Nested wildcard quantifiers (.*=.*)*.*$ triggered exponential backtracking on unmatched trailing strings
```

## 4. Remediation Patch / Corrected Configuration
```
# Hardened unanchored rule with bounded possessive quantifiers
SecRule REQUEST_URI|ARGS|HEADERS "^[a-zA-Z0-9_.-]{1,128}=(?:1|true)$" \\
    "id:100013,phase:2,t:none,t:lowercase,deny,status:403"
# Configured PCRE execution limits in nginx.conf
pcre_jit on;
pcre_recursion_limit 1000;
pcre_match_limit 5000;
```

## 5. Prevention & Hardening Checklist
- [ ] Enforce static static analysis on all regex rules via RE2/PCRE linting in pre-commit CI
- [ ] Configure global pcre_match_limit and pcre_recursion_limit guards in edge Nginx daemons
- [ ] Mandate phased canary rollout across single PoPs before global rule distribution
