# LLM-Wiki — Tech Knowledge Base

Personal tech knowledge wiki. LLM maintains wiki/ and vocabs/.

## Directory Access Rules (STRICT)

| Directory | Who reads it   | Who writes it | Notes |
|-----------|----------------|---------------|-------|
| raw/      | /compile ONLY  | /ingest ONLY  | NEVER read during /query or /lint |
| vocabs/   | /compile, /query, /lint | /compile (new terms) | canonical term registry |
| wiki/     | /query, /lint  | /compile, /query (saves) | LLM-readable knowledge base |

raw/ is the source archive. wiki/ is the LLM-readable knowledge base.
/query ALWAYS reads from wiki/ — NEVER from raw/.
If a topic has not been compiled into wiki/ yet, it does not exist for /query.

## Token-Efficient Reading Protocol
For /query and /lint — ALWAYS follow this order, never scan raw/:
1. vocabs/index.md  → identify term categories (~200 tokens)
2. wiki/index.md    → filter pages by tags   (~400 tokens)
3. Open only matched wiki/ pages (3-7 max)

## Skills
/ingest  → .claude/skills/ingest/SKILL.md
/compile → .claude/skills/compile/SKILL.md
/query   → .claude/skills/query/SKILL.md
/lint    → .claude/skills/lint/SKILL.md

## Key File Locations
Vocab registry:  vocabs/index.md
Vocab details:   vocabs/system-design.md | architecture.md | design-patterns.md
                 practices.md | data-storage.md | technologies.md | languages.md
Wiki catalog:    wiki/index.md
Wiki overview:   wiki/overview.md
Wiki page rules: wiki/CLAUDE.md
Operation log:   wiki/log.md
Raw archive:     raw/ (written by /ingest, read by /compile ONLY)
