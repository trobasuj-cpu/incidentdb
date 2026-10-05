# [INC-2021-FASTLY-01] Global CDN Cascade Failure via Dormant VCL Compilation Bug Triggered by Customer Update
**Company:** Fastly | **Date:** 2021-06-08 | **Severity:** CRITICAL  
**Technologies:** Varnish, VCL, C, CDN  
**Categories:** EDGE_INGRESS, COMPILER_BUG, SOFTWARE_DEFECT  

---

## 1. Symptoms & Observed Errors
```text
Fastly Error 503: Service Unavailable
Guru Mediation: #18.239401.1623145200.0
[CRITICAL] varnishd child process exited with status 11 (Segmentation Fault)
Signal 11 received in VCL execution unit: vcl_recv()
All healthy upstream backend pools collapsed to 0% available capacity.
```

## 2. Root Cause Analysis
A software deployment delivered in May contained a latent bug in the VCL (Varnish Configuration Language) compiler. On June 8, a single customer made a valid configuration update containing a specific combination of conditions that triggered this dormant bug. When the configuration propagated to the global edge, the Varnish daemon crashed with a segmentation fault across 85% of Fastly's global POPs.

## 3. Breaking Configuration / Problematic Code
```
# Customer VCL configuration triggering edge compiler fault:
sub vcl_recv {
    if (req.http.Fastly-Debug && req.url ~ "^/api/v2/(.*)") {
        set req.backend = F_origin;
        # Dormant bug in VCL compiler failed on specific null pointer in custom header regex
    }
}
```

## 4. Remediation Patch / Corrected Configuration
```
# Hardened VCL compiler parser with defensive pointer bounds checking
void compile_header_match(struct vcl_compiler *ctx, struct ast_node *node) {
    if (node == NULL || node->header_name == NULL || node->pattern == NULL) {
        vcl_compiler_error(ctx, "Invalid header match node structure");
        return;
    }
    generate_safe_regex_bytecode(ctx, node->header_name, node->pattern);
}
```

## 5. Prevention & Hardening Checklist
- [ ] Validate all customer configuration changes against a complete canary simulation harness before production deploy
- [ ] Isolate custom edge scripts into WebAssembly (Wasm) memory-sandboxed runtimes to prevent process crashes
- [ ] Implement automated circuit breakers to stop configuration distribution upon edge daemon segfaults
