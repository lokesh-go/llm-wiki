# Skill: /lint

## Trigger
User types: /lint

---

## What to Check

### 1. Contradictions
- Read all wiki pages in: concepts/, entities/, patterns/, best-practices/, comparisons/
- Flag pairs of pages that make conflicting claims about the same topic
- Format: ⚠️ CONTRADICTION: [[page-a]] vs [[page-b]] — "<claim A>" conflicts with "<claim B>"

### 2. Orphan Pages
- Find pages in wiki/ (ALL subdirectories including queries/) with NO inbound [[wikilinks]]
- These are invisible in the Obsidian graph and will never be surfaced by /query
- Format: 🔗 ORPHAN: [[page-name]] — no pages link to this

### 3. Stub Pages
- Find wiki pages with fewer than 5 meaningful content lines (ignoring frontmatter, headings, empty lines)
- Format: 📄 STUB: [[page-name]] — only <N> content lines

### 4. Broken Links
- Find [[wikilinks]] in page bodies that point to non-existent pages
- Format: ❌ BROKEN LINK: [[missing-page]] referenced in [[source-page]]

### 5. Vocab Gaps
- Find tech terms used in wiki page bodies (as [[wikilinks]] or plain text) not present in vocabs/index.md
- These cause vocabulary drift — same concept with different page names
- Format: 📚 VOCAB GAP: "<term>" used in [[page-name]] but not in vocabs/index.md

### 6. Coverage Gaps
- Identify vocab terms in vocabs/index.md that have NO corresponding wiki page yet
- These are knowledge areas not yet explored
- Format: 🗺️ COVERAGE GAP: "<term>" in vocabs/ has no wiki page yet

### 7. Index Accuracy
- Verify wiki/index.md entries match actual files in wiki/ subdirectories
- Flag: entries in index with no matching file (stale entries)
- Flag: files in wiki/ with no entry in index (missing entries)
- Format: 🗂️ INDEX MISMATCH: [[page-name]] in index but file not found
- Format: 🗂️ INDEX MISSING: file wiki/<folder>/<name>.md has no index entry

### 8. Overview Staleness
- Check wiki/overview.md "Sources ingested" count vs actual files in wiki/sources/
- Check wiki/overview.md "Wiki pages" count vs actual wiki/index.md pages count
- If counts are off: flag and suggest running /compile to resync
- Format: 📊 OVERVIEW STALE: overview says <N> sources, actual count is <M>

---

## After Checks — Update wiki/overview.md
After all checks complete, update wiki/overview.md:
- Update "Last lint:" date to today
- Update source and page counts to match actual state
- Update `upd:` in frontmatter

## Output Format
Print a grouped report:
  LINT REPORT — [YYYY-MM-DD]
  ─────────────────────────
  ⚠️  Contradictions:  <count>
  🔗 Orphans:          <count>
  📄 Stubs:            <count>
  ❌ Broken links:     <count>
  📚 Vocab gaps:       <count>
  🗺️  Coverage gaps:   <count>
  🗂️  Index issues:    <count>
  📊 Overview issues:  <count>
  ─────────────────────────
  <detail lines per issue>
  ─────────────────────────
  Suggested next sources to ingest: <list based on coverage gaps>

Append to wiki/log.md:
  ## [YYYY-MM-DD] lint | Health check
  <N> issues found. <summary of main problems>.
