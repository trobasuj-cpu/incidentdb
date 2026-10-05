# [INC-2023-K8S-01] Production Pod Eviction Cascades via Java Heap Allocation Mismatch under cgroup v2
**Company:** Datadog Community | **Date:** 2023-04-14 | **Severity:** HIGH  
**Technologies:** Kubernetes, Linux, cgroupv2, Java, JVM  
**Categories:** OOM_KILL, MEMORY_SAFETY, KUBERNETES_ORCHESTRATION  

---

## 1. Symptoms & Observed Errors
```text
Last State: Terminated
Reason: OOMKilled
Exit Code: 137
Container: payment-processor-api
[Kernel] Memory cgroup out of memory: Killed process 18291 (java) total-vm:4294967296B, anon-rss:2147483648B, file-rss:1048576B
Pod status: CrashLoopBackOff
```

## 2. Root Cause Analysis
Following a node OS upgrade from Ubuntu 20.04 to Ubuntu 22.04 with default cgroup v2, legacy OpenJDK 11 containers failed to recognize the memory limits imposed by Kubernetes manifests. The JVM read the host's physical memory (128GB) instead of the pod's limit (2GB) and dynamically set its maximum heap to 32GB. During traffic surges, the Linux kernel cgroup controller immediately issued SIGKILL (code 137) to the processes.

## 3. Breaking Configuration / Problematic Code
```
# Pod spec with memory limit but legacy JVM flags:
spec:
  containers:
  - name: payment-processor-api
    image: openjdk:11-jre-slim  # Unpatched OpenJDK 11.0.1 without cgroup v2 support
    resources:
      limits:
        memory: "2Gi"
      requests:
        memory: "1Gi"
    env:
    - name: JAVA_OPTS
      value: "-Xmx1536m" # Insufficient margin for off-heap native memory & metaspace
```

## 4. Remediation Patch / Corrected Configuration
```
# Modern container spec with cgroup v2 aware JDK and percentage flags
spec:
  containers:
  - name: payment-processor-api
    image: eclipse-temurin:17-jre  # Fully cgroup v2 aware
    resources:
      limits:
        memory: "2Gi"
      requests:
        memory: "2Gi"
    env:
    - name: JAVA_OPTS
      value: "-XX:+UseContainerSupport -XX:MaxRAMPercentage=75.0 -XX:+ExitOnOutOfMemoryError"
```

## 5. Prevention & Hardening Checklist
- [ ] Verify all JVM runtimes inside Docker containers are upgraded to versions supporting cgroup v2 hierarchy
- [ ] Set MaxRAMPercentage to 75% max to preserve 25% overhead for thread stacks, metaspace, and native memory
- [ ] Implement synthetic memory load testing in staging whenever container host kernel versions change
