# LLM-Wiki

> A personal, compounding tech knowledge base maintained by an LLM.  
> Based on [Andrej Karpathy's LLM-Wiki pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).

---

## What Is This?

Traditional AI assistants re-derive knowledge from scratch on every query — expensive, slow, and non-cumulative. **LLM-Wiki is different.**

You feed it sources (articles, docs, papers, notes). An LLM **compiles** them into a persistent, interlinked knowledge base in a structured wiki format. Future queries search the wiki — not the raw internet, not raw documents — resulting in fast, token-efficient answers that compound over time.

```
Without LLM-Wiki:
  question → LLM reads internet/documents → answer (lost)

With LLM-Wiki:
  /ingest article <url> → wiki grows
  /query "your question" → LLM reads compiled wiki → answer (saved, reusable)
```

The more you ingest, the smarter and cheaper the system becomes.

---

## How It Works

```
┌─────────────────────────────────────────────────────────────┐
│                        YOU                                  │
│  /ingest article <url>    /query "question"    /lint        │
└────────────┬──────────────────────┬────────────────────────┘
             │                      │
             ▼                      ▼
       ┌──────────┐          ┌─────────────────────┐
       │  raw/    │          │     wiki/           │
       │ archive  │──────►   │  knowledge base     │
       │(immutable│ /compile  │  (LLM-readable)    │
       │after     │          │                     │
       │ ingest)  │          │ concepts/ entities/ │
       └──────────┘          │ patterns/ sources/  │
                             │ comparisons/ queries│
                             └──────────┬──────────┘
                                        │
                             ┌──────────▼──────────┐
                             │     vocabs/          │
                             │  canonical term      │
                             │  registry            │
                             └─────────────────────┘
```

**Three zones, strict access rules:**

| Zone | Purpose | Read by | Written by |
|------|---------|---------|-----------|
| `raw/` | Source archive (unchanged originals) | `/compile` only | `/ingest` only |
| `vocabs/` | Canonical term dictionary | `/compile`, `/query`, `/lint` | `/compile` (new terms) |
| `wiki/` | LLM-readable knowledge base | `/query`, `/lint` | `/compile`, `/query` (saves) |

> `/query` **never** reads from `raw/`. It always reads from the compiled `wiki/`.

---

## Directory Structure

```
llm-wiki/
│
├── CLAUDE.md                        # Root constitution — read first by Claude on every session
├── README.md                        # This file
│
├── .claude/                         # Claude Code configuration
│   └── skills/                      # Skill definitions (loaded on-demand, not on every session)
│       ├── ingest/
│       │   └── SKILL.md             # /ingest workflow: fetch URL → save raw → trigger compile
│       ├── compile/
│       │   └── SKILL.md             # /compile workflow: raw → vocab lookup → wiki pages
│       ├── query/
│       │   └── SKILL.md             # /query workflow: vocab filter → wiki search → answer
│       └── lint/
│           └── SKILL.md             # /lint workflow: health check across all wiki files
│
├── raw/                             # Source archive — immutable after ingestion
│   ├── articles/                    # From: /ingest article <url>
│   ├── docs/                        # From: /ingest doc <url>      (official documentation)
│   ├── papers/                      # From: /ingest paper <url>    (academic papers)
│   ├── notes/                       # From: /ingest note <text>    (pasted text or local file)
│   └── assets/                      # Downloaded images from articles
│
├── vocabs/                          # Canonical term registry — the LLM's shared dictionary
│   ├── index.md                     # ← ALWAYS READ FIRST. Flat term→category map (~200 tokens)
│   ├── system-design.md             # scalability, CAP-theorem, caching, circuit-breaker, etc.
│   ├── architecture.md              # microservices, CQRS, DDD, event-sourcing, saga-pattern, etc.
│   ├── design-patterns.md           # All 22 GoF patterns (creational/structural/behavioral)
│   ├── practices.md                 # TDD, CI-CD, observability, blue-green, chaos-engineering, etc.
│   ├── data-storage.md              # SQL, NoSQL, ACID, indexing, connection-pooling, etc.
│   ├── technologies.md              # Kafka, Redis, PostgreSQL, Kubernetes, Docker, etc.
│   └── languages.md                 # Go, Node.js, TypeScript, Rust — language-specific concepts
│
└── wiki/                            # LLM-readable compiled knowledge base
    ├── CLAUDE.md                    # Wiki writing rules + all page templates
    ├── index.md                     # Content catalog — all pages with tags (Tier 2 read for /query)
    ├── log.md                       # Append-only operation history (ingest/compile/query/lint)
    ├── overview.md                  # High-level synthesis: coverage state, page counts, gaps
    │
    ├── concepts/                    # Abstract tech concepts: scalability.md, caching.md, CQRS.md
    ├── entities/                    # Named tools/technologies: redis.md, kafka.md, kubernetes.md
    ├── patterns/                    # Design & architecture patterns: observer.md, saga.md, BFF.md
    ├── best-practices/              # Do/don't pages: api-design.md, caching-strategies.md
    ├── comparisons/                 # A-vs-B pages: kafka-vs-rabbitmq.md, sql-vs-nosql.md
    ├── sources/                     # One page per ingested source (bibliography)
    └── queries/                     # Saved /query answers for future reuse
```

### Why Each Directory Matters

**`vocabs/`** — The most important directory after `wiki/`. Every wiki page uses canonical terms from here. This prevents vocabulary drift (same concept with 5 different names) and enables the tag-based search in `wiki/index.md` to work reliably.

**`wiki/sources/`** — Every ingested article gets a source page. This is the bibliography. When a wiki concept page says `[[martin-fowler-cqrs]]`, you can trace the claim back to its origin. `/lint` uses this for contradiction detection.

**`wiki/index.md`** — The LLM's map of the entire wiki. It's read on every `/query` to filter relevant pages before opening anything. Format: `[[page]] | type | tag1,tag2,tag3 | src-count`.

**`wiki/overview.md`** — Human-readable dashboard. Shows coverage per category, page counts, and knowledge gaps. Updated by `/compile` and `/lint`.

**`wiki/queries/`** — Saved answers from `/query`. If you ask the same question again, the answer is found here directly instead of re-reading 5 pages. Token-saving payoff from previous sessions.

**`.claude/skills/`** — Skill files are NOT loaded on every session (unlike `CLAUDE.md`). They're loaded on-demand only when you invoke a skill. This keeps the base session lean.

---

## Skills

### `/ingest <category> <url|text>`

Fetches a source and saves it to `raw/`, then automatically triggers `/compile`.

```bash
# Ingest a blog article
/ingest article https://martinfowler.com/bliki/CQRS.html

# Ingest official documentation
/ingest doc https://docs.kafka.apache.org/intro

# Ingest an academic paper
/ingest paper https://arxiv.org/abs/...

# Ingest a note (pasted text or local file path)
/ingest note "My notes on Redis architecture..."
```

**What it does:**
1. Fetches URL → strips ads/nav/footers → converts to clean markdown
2. Downloads images to `raw/assets/`
3. Saves to `raw/<category>s/<slugified-title>.md` with frontmatter (`status: raw`)
4. Logs to `wiki/log.md`
5. Auto-triggers `/compile` immediately

---

### `/compile [file|folder]`

Reads from `raw/`, converts to wiki format using vocab-aware extraction, updates all index files.

```bash
# Auto-called after /ingest — but you can also call manually:

# Recompile a single raw file
/compile raw/articles/understanding-cqrs-pattern.md

# Batch compile an entire folder
/compile raw/articles/
```

**What it does:**
1. Reads `vocabs/index.md` (always first)
2. For each tech term found:
   - **Case A** (known term): use canonical name from vocabs
   - **Case B** (new term): add to `vocabs/<category>.md` and `vocabs/index.md`
3. Writes `wiki/sources/<slug>.md` (source page with summary + claims)
4. Creates/updates concept, entity, pattern, comparison, best-practice pages in `wiki/`
5. Updates `wiki/index.md` (page catalog + tag entries)
6. Updates `wiki/overview.md` (coverage per category, page counts)
7. Appends to `wiki/log.md`
8. Sets raw file `status: raw → compiled`

---

### `/query <question>`

Searches the wiki using a 3-tier token-efficient lookup. Never touches `raw/`.

```bash
/query "When should I use Redis vs Kafka?"
/query "What is the difference between CQRS and event sourcing?"
/query "Which design pattern solves the problem of multiple notification subscribers?"
```

**What it does:**
1. Reads `vocabs/index.md` → identifies relevant categories from the question
2. Reads `wiki/index.md` → filters pages by matching tags across ALL sections
   - Saved queries are prioritised — if one matches, it's read first
   - If a saved query fully answers the question, stops there (short-circuit)
3. Opens 3–7 matched pages; reads `>` summary line first to confirm relevance
4. Synthesises answer with `[[citations]]` + confidence level
5. Prompts: **"Worth saving to wiki? (y/n)"**
   - If yes: writes `wiki/queries/<slug>.md`, updates `wiki/index.md`, `wiki/log.md`, backlinks consulted pages

---

### `/lint`

Health check across the entire wiki. Run this periodically (every 10–20 ingests).

```bash
/lint
```

**What it checks:**

| Check | What it finds |
|-------|---------------|
| ⚠️ Contradictions | Pages making conflicting claims |
| 🔗 Orphans | Pages with no inbound `[[wikilinks]]` (invisible in Obsidian graph) |
| 📄 Stubs | Pages with < 5 meaningful content lines |
| ❌ Broken links | `[[links]]` pointing to non-existent pages |
| 📚 Vocab gaps | Terms used in wiki not registered in `vocabs/index.md` |
| 🗺️ Coverage gaps | Vocab terms with no wiki page yet |
| 🗂️ Index issues | Stale index entries or files missing from index |
| 📊 Overview staleness | Counts in `wiki/overview.md` out of sync with actual files |

After checks: updates `wiki/overview.md` and appends to `wiki/log.md`.  
Ends with: suggested next articles to ingest based on coverage gaps.

---

## How Queries Are Token-Efficient

```
TIER 1 — ROUTE    (vocabs/index.md)       ~200 tokens
                   "What categories match this question?"

TIER 2 — FILTER   (wiki/index.md)         ~400 tokens
                   "Which pages have matching tags?"

TIER 3 — READ     (3–7 wiki pages)        ~1000–3000 tokens
                   "Read only relevant, pre-compiled pages"
                   ↑ Summary line first → skip if not relevant

Total: ~1600–3600 tokens per query (vs. reading everything = 50,000+ tokens)
```

The wiki format is specifically designed for LLM reading:
- Every page starts with a `>` one-line summary for fast relevance scanning
- Compact YAML frontmatter (`t`, `cat`, `tags`, `src`, `rel`, `upd`)
- `[[wikilinks]]` in body for cross-referencing and Obsidian graph edges

---

## Obsidian Graph View Setup

![Obsidian Graph View](https://github.com/user-attachments/assets/eec97bbf-002c-47af-9dbb-f6e502575586)

The wiki is designed to be visualised in [Obsidian](https://obsidian.md/).

1. Open the `llm-wiki/` folder as an Obsidian vault
2. Set attachments folder: **Settings → Files → Attachment folder path** → `raw/assets`
3. Enable **Tags** in Graph View settings
4. Set Graph Groups (color-coding by folder):

| Group filter | Color | Meaning |
|---|---|---|
| `path:wiki/concepts/` | Blue | Abstract concepts |
| `path:wiki/entities/` | Green | Technologies/tools |
| `path:wiki/sources/` | Yellow | Ingested sources |
| `path:wiki/patterns/` | Purple | Design patterns |
| `path:wiki/comparisons/` | Orange | A-vs-B comparisons |
| `path:wiki/queries/` | Pink | Saved query answers |
| `path:vocabs/` | Grey | Vocabulary definitions |

**How graph edges are built:**
- `[[wikilinks]]` in page **body** → edge in the graph
- `tags:` in frontmatter → tag node, pages cluster together
- `rel:` in frontmatter → metadata only, NOT a graph edge

---

## Recommended Workflow

### Getting Started (Phase 1)
```
1. Open this repo in Obsidian
2. Open Claude Code in this directory
3. Run your first ingest:
   /ingest article https://martinfowler.com/bliki/CQRS.html
4. Watch wiki/ populate automatically
5. Check Obsidian graph — nodes should appear
```

### Building Up (Phase 2 — 5-20 sources)
```
- Ingest 1-2 articles per session
- Run /query on things you're curious about
- Save good answers to wiki/queries/
- Run /lint after every ~10 ingests
```

### Scaling (Phase 3 — 50+ sources)
```
- Use /compile raw/articles/ for batch processing
- /lint will guide you toward coverage gaps
- /query becomes faster as saved queries accumulate
```

---

## File Conventions

### Raw File Frontmatter (saved by /ingest)
```yaml
---
title: "Understanding CQRS"
url: https://martinfowler.com/bliki/CQRS.html
author: Martin Fowler
published: 2011-07-14
fetched: 2026-04-18
category: article
status: raw          # → changes to "compiled" after /compile
---
```

### Wiki Page Frontmatter (written by /compile)
```yaml
---
t: concept           # concept|entity|pattern|practice|source|comparison|query
cat: architecture    # vocab category
tags: [CQRS, event-sourcing, DDD]
src: 3               # number of sources this page is backed by
rel: [event-sourcing, DDD]
upd: 2026-04-18
---
> One-sentence summary for fast relevance check during /query.
```

### Vocab Index Format (`vocabs/index.md`)
```
category:  term1, term2, term3,
           term4, term5
```

### Wiki Index Format (`wiki/index.md`)
```
- [[page-name]] | type | tag1,tag2,tag3 | src-count
```

### Log Format (`wiki/log.md`)
```
## [YYYY-MM-DD] <op> | <title>
<one line of what was done>
ops: init | ingest | compile | query | lint
```

---

## Current Vocabulary Coverage

| Category | File | Terms |
|----------|------|-------|
| System Design | `vocabs/system-design.md` | 16 terms |
| Architecture | `vocabs/architecture.md` | 13 terms |
| Design Patterns | `vocabs/design-patterns.md` | 22 GoF patterns |
| Engineering Practices | `vocabs/practices.md` | 11 terms |
| Data & Storage | `vocabs/data-storage.md` | 11 terms |
| Technologies | `vocabs/technologies.md` | 20+ tools |
| Languages | `vocabs/languages.md` | 30 terms (Go, Node, TS, Rust) |

Vocabulary grows automatically as new terms are encountered during `/compile`.

---

## Credits

Inspired by [Andrej Karpathy's LLM-Wiki gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) — the idea that an LLM should maintain a wiki rather than re-derive knowledge on every query.

[def]: https://github.com/user-attachments/assets/eec97bbf-002c-47af-9dbb-f6e502575586