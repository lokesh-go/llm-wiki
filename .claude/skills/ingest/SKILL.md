# Skill: /ingest

## Trigger
User types: /ingest <category> <url|text>
Categories: article | doc | paper | note

---

## Step 1 — FETCH
- Fetch the URL, strip nav/ads/footers/JS/cookie banners
- Convert to clean markdown, preserve headings, paragraphs, code blocks
- Download inline images → raw/assets/<filename>
- Build frontmatter:
    title:     <extracted page title>
    url:       <source url>
    author:    <extracted or "unknown">
    published: <extracted date or "unknown">
    fetched:   <today YYYY-MM-DD>
    category:  <article|doc|paper|note>
    status:    raw
- Save to the correct folder based on category:
    article → raw/articles/<slugified-title>.md
    doc     → raw/docs/<slugified-title>.md
    paper   → raw/papers/<slugified-title>.md
    note    → raw/notes/<slugified-title>.md

## Category-Specific Notes
- article: general tech articles, blog posts, tutorials
- doc:     reference/official documentation — preserve structure more literally, less summary
- paper:   academic — extract abstract, methodology, findings as distinct sections
- note:    pasted text or local file path instead of URL; minimal processing

## Step 2 — LOG THE FETCH
Append to wiki/log.md:
  ## [YYYY-MM-DD] ingest | <title>
  Fetched from <url>. Saved to raw/<category>s/<slug>.md. Status: raw.

## Step 3 — AUTO COMPILE
- Immediately execute /compile on the saved raw file
- Load and follow: .claude/skills/compile/SKILL.md
- Do NOT wait for user confirmation before compiling

## Notes
- raw/ is READ ONLY after save — /compile reads it, never modifies content
- Only status: field in frontmatter changes (raw → compiled) during /compile
- User can manually call /compile later: /compile raw/<category>s/<slug>.md
- Batch compile an entire folder: /compile raw/articles/
