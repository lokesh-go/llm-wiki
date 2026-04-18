# Skill: /query

## Trigger
User types: /query <question>

## STRICT RULE
NEVER read from raw/ during /query.
raw/ is the source archive — only /compile reads it.
wiki/ is the LLM-readable knowledge base — /query ALWAYS reads from wiki/ only.
If a topic has not been compiled into wiki/ yet, say so and suggest running /ingest.

---

## Step 1 — ROUTE (read vocabs/index.md)
1. Read the FULL vocabs/index.md file (~200 tokens)
2. From the question, identify which vocab categories are relevant
   Example: "When should I use Redis vs Kafka?"
   → matches categories: technologies, data-storage, system-design
3. Note the canonical term names from those categories
   (these are the tags you will match against in Step 2)

---

## Step 2 — FILTER (read wiki/index.md)
1. Read wiki/index.md
2. Scan EVERY section for entries whose tags overlap with Step 1 terms:
   - ## Concepts
   - ## Entities
   - ## Patterns
   - ## Best Practices
   - ## Comparisons
   - ## Sources
   - ## Saved Queries   ← DO NOT SKIP THIS SECTION
3. From all matching entries across all sections, select top 3-7 candidates
   ranked by how many tags overlap with the question's terms

### Priority Rule for Saved Queries:
If a "## Saved Queries" entry has tags that match the question:
  → ALWAYS include that page as a candidate, ranked FIRST
  → A saved query was compiled specifically to answer a question like this
  → It will likely satisfy the query with far fewer tokens than reading raw pages

---

## Step 3 — READ (open only matched pages)
For each candidate page (starting from top-ranked):

1. Open the page
2. Read ONLY the > blockquote summary line (first line after frontmatter):
   - If summary directly answers or closely matches the question:
       → Read the full page
       → If this is a Saved Query page AND the answer is sufficient:
           STOP here. Do not open remaining candidates.
           Go to Step 4.
   - If summary does not match:
       → Skip this page, move to next candidate

3. Continue until you have 2-5 pages with confirmed relevant content

### Short-circuit rule:
If a Saved Query page fully answers the question → skip all remaining candidates.
No need to re-read the underlying concept/entity pages.
This is the token-saving payoff of saved queries.

---

## Step 4 — ANSWER
- Synthesize a clear answer from the pages read
- Cite pages inline: [[page-name]]
- If answer came primarily from a Saved Query page, note that:
  "Answer sourced from saved query [[page-slug]]"
- State confidence: high | medium | low
- State which pages were consulted and how many tokens were saved
  vs. reading from scratch (estimate: "read 1 saved query vs ~5 pages")

---

## Step 5 — SAVE DECISION
Ask the user: "Worth saving to wiki? (y/n)"

If no → stop.

If yes → do ALL of the following:

### 5a — Write the Query Page
File: wiki/queries/<slugified-question>.md
Template from wiki/CLAUDE.md (Query Page template):
  Frontmatter:
    t: query
    cat: <most relevant vocab category from Step 1>
    tags: [<canonical vocab terms used in the answer>]
    src: 0
    rel: [<all pages consulted in Step 3>]
    upd: <today YYYY-MM-DD>
  Opening line: > Answer to: "<exact question asked>"
  Sections:
    ## Answer         ← full synthesized answer with [[citations]]
    ## Pages Consulted ← [[page1]], [[page2]] as wikilinks (for graph edges)
    ## Confidence     ← high | medium | low
    ## Follow-up Questions ← related questions worth exploring

### 5b — Update wiki/index.md
Under "## Saved Queries" section, add:
  - [[<slug>]] | query | tag1,tag2,tag3 | 0
  (tags must match canonical vocab terms so future queries can find this)
Update frontmatter:
  - Increment `pages:` count by 1
  - Update `upd:` to today's date

### 5c — Update wiki/log.md
Append:
  ## [YYYY-MM-DD] query | <original question>
  <one line: key answer or decision captured>

### 5d — Backlink from Consulted Pages (prevents orphan in graph)
For each wiki page read in Step 3:
  - Open the page
  - Under its "## Sources" section add:
      - [[<query-slug>]]: referenced in query "<question>"
  - Increment its `src:` frontmatter count by 1

This ensures the saved query page is connected in the Obsidian graph
and will NOT be flagged as an orphan by /lint.
