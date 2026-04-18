# Skill: /compile

## Trigger
- Auto-called after /ingest (do not skip)
- Manual: /compile raw/articles/<slug>.md

## Step 0 — VOCAB LOOKUP (always do this first)
1. Read vocabs/index.md (read the entire file — it is tiny, ~200 tokens)
2. Scan the raw content for tech terms
3. For each term found:

   CASE A — term exists in vocabs/index.md:
     → Use that canonical name when writing wiki pages
     → If you need the definition, read vocabs/<category>.md

   CASE B — term NOT in vocabs/index.md:
     → Determine the correct category (system-design / architecture /
       design-patterns / practices / data-storage / technologies)
     → Add term entry to vocabs/<category>.md
     → Add term to the correct line in vocabs/index.md
     → Use that canonical name going forward

## Step 1 — WRITE SOURCE PAGE
- Create wiki/sources/<slug>.md using Source Page template from wiki/CLAUDE.md
- Update the raw file: change status: raw → status: compiled

## Step 2 — UPDATE WIKI PAGES
For each concept and entity identified (using canonical vocab names):
  If wiki page already exists:
    → Append new insights, note the source, check for contradictions
    → Increment src: count in frontmatter
  If wiki page does not exist:
    → Create from appropriate template in wiki/CLAUDE.md
  CRITICAL RULE: all cross-references MUST be [[wikilinks]] in the page BODY
                 (not just in frontmatter rel: field)

## Step 3 — SPECIAL PAGES (create if applicable)
- Article makes a comparison → create/update wiki/comparisons/<a-vs-b>.md
- Article lists best practices → create/update wiki/best-practices/<topic>.md
- Article is about a design pattern → create/update wiki/patterns/<name>.md

## Step 4 — UPDATE INDEX + LOG
- wiki/index.md: add or update entry using format:
    - [[page-name]] | <type> | tag1,tag2,tag3 | <src-count>
  Update frontmatter: upd, pages count, sources count
- wiki/log.md: append at the bottom:
    ## [YYYY-MM-DD] compile | <article title>
    <one line: what was added or updated>
