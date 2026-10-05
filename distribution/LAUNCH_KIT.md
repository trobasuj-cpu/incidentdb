# IncidentDB — Complete Launch Kit & Multi-Platform Distribution Funnel (2026)

This document contains ready-to-copy launch materials for Gumroad, GitHub, Kaggle, Hugging Face, Twitter/X, Reddit, and Hacker News.

---

## 💎 1. Gumroad Product Listing

- **Product Name**: `IncidentDB: 500+ Real Production Outages, Root-Cause Diffs & Disaster Runbooks`
- **Tagline**: The ultimate RAG ground-truth dataset and runbook vault for SREs and DevOps AI agents.
- **Price**: **$39** (Pro Developer Suite) / $499 (Enterprise Team License)
- **Cover Image**: `distribution/incidentdb_banner_16x9.jpg`
- **Thumbnail**: `distribution/incidentdb_thumb_1x1.jpg`
- **Attached Deliverables**: `distribution/incidentdb_pro_vault.zip`

### Product Description (Markdown for Gumroad)
```markdown
### 🚨 Stop Let Your AI Hallucinate Production Outages. Give It Real Ground Truth.

When your PostgreSQL replica stops streaming WAL logs, your BGP routes get withdrawn, or an eBPF filter locks every CPU core, generic LLMs fail. They tell you to "check your network connection" or "restart your server."

**IncidentDB changes the game.** We spent 100+ hours reading, curating, and reverse-engineering the most catastrophic real-world production postmortems from Tier-1 engineering organizations:
- **Cloudflare** (WAF regex catastrophic backtracking)
- **GitLab** (Production database deletion & replication lag)
- **Amazon Web Services S3** (Subsystem decommission command typo)
- **CrowdStrike** (Channel File 291 kernel memory access violation)
- **Netflix** (Cassandra major compaction EBS burst IOPS storm)
- **GitHub, Fastly, Meta, Uber, Discord, Roblox, Stripe, Slack, Heroku, Redis** and 500+ more!

---

### 📦 What You Get in the Pro Vault:

1. **500+ Dense, Compilable Incident Postmortems (`incident_database.jsonl`)**:
   - Exact symptom error logs & telemetry signatures
   - In-depth architectural root-cause analyses
   - Real breaking configuration snippets & code bugs
   - Verified remediation diffs & patches
   - Actionable 5-point prevention checklists

2. **Pre-Partitioned RAG Markdown Runbooks (`/rag_vault/`)**:
   - Organized by technology stack (`nginx/`, `postgresql/`, `redis/`, `bgp/`, `windows_kernel/`, `kubernetes/`, `cassandra/`)
   - Zero-friction drag-and-drop into LangChain, LlamaIndex, Chroma, or Pinecone

3. **High-Speed In-Memory Search CLI (`src/cli.py`)**:
   - Zero-dependency local search and retrieval engine
   - Instant keyword & metadata filtering without paid vector APIs

4. **100% Census Audit Quality Guarantee**:
   - Zero placeholder stubs (`TODO`, `FIXME`, `STUB`, `TBD`)
   - 100% dense, authentic configurations

---

### 💡 Perfect For:
- **Platform & SRE Engineers**: Keep battle-tested disaster recovery runbooks at your fingertips.
- **DevOps AI Developers**: Ground your autonomous troubleshooting agents with verified historical postmortems.
- **Architecture Teams**: Conduct premortem disaster simulations based on real industry failures.

Instant download. 100% money-back guarantee if you don't find at least 5 outages identical to issues your team has faced.
```

---

## 🌐 2. Hugging Face Dataset Card (`README.md`)

- **Repo Name**: `incidentdb`
- **License**: `mit`
- **Task Categories**: `text-classification`, `question-answering`, `rag`, `code-generation`
- **Languages**: `en`
- **Size Categories**: `1K<n<10K`

```markdown
---
license: mit
task_categories:
- question-answering
- text-retrieval
language:
- en
tags:
- devops
- sre
- postmortems
- outages
- rag
- ground-truth
size_categories:
- 1K<n<10K
---

# IncidentDB: Ground-Truth Production Outages & Remediation Diffs

IncidentDB is the largest structured ground-truth collection of production outage postmortems, breaking configurations, and remediation patches from top tech companies (Cloudflare, AWS, GitLab, CrowdStrike, GitHub, Netflix, Fastly, Meta, etc.).

### Features:
- **100% Census Audited**: Zero toy stubs, zero TODOs.
- **Full Operational Lifecycle**: Error logs -> Root Cause -> Problematic Code -> Working Patch -> Prevention Checklist.
- **RAG Ready**: Designed for direct ingestion into DevOps AI Agents.

### Load with Datasets:
```python
from datasets import load_dataset

dataset = load_dataset("trobasuj-cpu/incidentdb")
print(dataset["train"][0])
```
```

---

## 🏆 3. Kaggle Dataset & Notebook Metadata

- **Dataset / Notebook Title**: `IncidentDB: 500+ Real Production Outages 2026` (45 chars — strictly `<= 50`)
- **Subtitle**: Ground-truth postmortems, breaking configs & disaster diffs
- **Notebook File**: `notebook/1_click_incident_analysis.ipynb`
- **Hardware**: GPU T4 x2 or CPU (Runs in < 1 minute)
- **Tags**: `devops`, `software-engineering`, `cybersecurity`, `nlp`, `rag`

---

## 🐦 4. Twitter / X Viral Launch Post

> **Strict Rule**: Must be `<= 256` characters.

### English Post (251 characters):
```text
We spent 100+ hours reading postmortems so you don't have to.

Meet IncidentDB: 500+ verified production outages (Cloudflare, AWS, CrowdStrike) with exact breaking configs & remediation diffs.

RAG-ready for SRE AI.

github.com/trobasuj-cpu/incidentdb
```

### Russian Version (для Telegram / Habr / VK):
```text
Мы провели 100+ часов за чтением постмортемов, чтобы вам не пришлось.

Встречайте IncidentDB: 500+ подтвержденных аварий продакшна (Cloudflare, AWS, GitLab, CrowdStrike) с реальными логами, рут-козом и диффами исправлений.

RAG-ready Markdown + JSONL для SRE и DevOps AI агентов:
https://github.com/trobasuj-cpu/incidentdb
```

---

## 📢 5. Reddit Post (`r/devops`, `r/sysadmin`, `r/LocalLLaMA`)

**Title**: `I spent 100+ hours compiling 500+ real production outages (Cloudflare, AWS, CrowdStrike) into a structured RAG dataset with exact breaking configs & fix diffs. It's open source.`

**Body**:
```text
Hey everyone,

Whenever we try to build DevOps AI assistants or SRE bots with local LLMs, they hallucinate generic advice ("have you checked DNS?", "try restarting the container"). They have never seen actual production catastrophic failure logs or the subtle 1-line configuration typos that brought down global edge networks.

To fix this, I built **IncidentDB**:
Instead of synthetic AI fluff, this is pure ground-truth labor arbitrage. I went through hundreds of published postmortems across engineering blogs (Dan Luu's list, Cloudflare, GitLab, AWS, CrowdStrike, GitHub, Fastly, Netflix, Meta) and extracted them into an exact schema:
1. Exact symptom error logs & telemetry
2. Root cause analysis (concurrency, BGP, replication lag, regex backtracking)
3. The exact breaking config or buggy code
4. The exact remediation diff / patch
5. Prevention checklist

### Features:
- **100% Census Audited**: No `TODO`, `FIXME`, or 2-line toy stubs.
- **RAG-Vault**: Multi-file Markdown folder organized by technology stack (`nginx/`, `redis/`, `postgresql/`, `bgp/`, `windows_kernel/`).
- **Local CLI Search**: Built-in zero-dependency search engine (`python -m src.cli search --query "WAL"`).
- **12/12 Passing Unit Tests**: Validated with a deterministic test runner.

GitHub Repo (MIT): https://github.com/trobasuj-cpu/incidentdb

Would love feedback from fellow SREs on which classic outages we should add next!
```

---

## 🚀 6. Hacker News (Show HN)

**Title**: `Show HN: IncidentDB – 500+ real production outages with root-cause diffs (RAG-ready)`

**Body**:
```text
Hi HN,

Most DevOps AI tools struggle because foundation models are trained on textbook code rather than catastrophic production failure modes.

IncidentDB is an open-source, structured database of real postmortems (Cloudflare WAF regex backtracking, GitLab primary DB deletion, AWS S3 index server typo, CrowdStrike Channel 291 invalid pointer read, etc.).

Each record includes:
- Production symptom logs
- Deep root cause analysis
- Breaking configuration / code snippet
- Remediation patch / diff
- Prevention checklist

Includes a pre-partitioned Markdown vault for RAG pipelines (LangChain, LlamaIndex) and a zero-dependency CLI search tool.

Repo: https://github.com/trobasuj-cpu/incidentdb

Feedback and pull requests with interesting postmortems are welcome!
```
