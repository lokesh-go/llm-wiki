# Wiki Writing Rules

## Frontmatter Schema (include on EVERY wiki page)
```yaml
---
t: concept|entity|pattern|practice|source|comparison|query
cat: system-design|architecture|design-patterns|practices|data-storage|technologies|languages
tags: [term1, term2]
src: <count>
rel: [page1, page2]
upd: YYYY-MM-DD
---
```

## Page Opening Rule (REQUIRED after frontmatter)
First line after frontmatter MUST be a blockquote summary:
> One-sentence description of what this page covers.

The LLM uses this line during /query for fast relevance check — read summary first, decide to read full page or skip.

## Wikilink Rule (CRITICAL for Obsidian graph)
- Cross-references MUST appear as [[wikilinks]] in the page BODY
- The `rel:` frontmatter field is metadata only — NOT picked up by Obsidian graph
- More wikilinks in body = richer, more connected graph

## wiki/index.md Entry Format
```
- [[page-name]] | type | tag1,tag2,tag3 | src-count
```

## wiki/log.md Entry Format
```
## [YYYY-MM-DD] <op> | <title>
<one line of what was done>
```
ops: init | ingest | compile | query | lint

---

## Page Templates

### Concept Page — wiki/concepts/<name>.md
```
---
t: concept
cat: <vocab-category>
tags: [<main-term>, <related-term>]
src: 0
rel: []
upd: <date>
---
> <One-sentence definition of this concept.>

## Definition
<2-3 sentences>

## Key Properties
- <property>

## When to Use
<context and scenarios>

## When NOT to Use / Trade-offs
<anti-patterns and limitations>

## Related Concepts
See also: [[concept-a]], [[concept-b]]

## Sources
- [[source-slug]]: <one-line takeaway>
```

### Entity Page — wiki/entities/<name>.md
```
---
t: entity
cat: technologies
tags: [<name>, <type>]
src: 0
rel: []
upd: <date>
---
> <What this technology is in one line.>

## What It Is
<Brief description>

## Primary Use Cases
- <use case>

## Strengths
- <strength>

## Weaknesses / Limitations
- <limitation>

## Best Used With
[[entity-a]], [[concept-b]]

## NOT a Good Fit When
<context>

## Sources
- [[source-slug]]: <takeaway>
```

### Pattern Page — wiki/patterns/<name>.md
```
---
t: pattern
cat: design-patterns|architecture
tags: [<pattern-name>]
src: 0
rel: []
upd: <date>
---
> <What problem this pattern solves in one line.>

## Intent
<Problem it solves>

## Structure
<How it works — use a simple diagram or bullet list>

## When to Use
<scenarios>

## When NOT to Use
<anti-patterns>

## Example
<code snippet or pseudo-code>

## Related Patterns
[[pattern-a]], [[pattern-b]]

## Sources
- [[source-slug]]: <takeaway>
```

### Best Practice Page — wiki/best-practices/<topic>.md
```
---
t: practice
cat: <category>
tags: [<topic>]
src: 0
rel: []
upd: <date>
---
> Consolidated do's and don'ts for <topic>.

## Do
- ✅ <best practice>

## Don't
- ❌ <anti-pattern>

## Why It Matters
<context>

## Related
[[concept-a]], [[entity-b]]

## Sources
- [[source-slug]]: <takeaway>
```

### Comparison Page — wiki/comparisons/<a-vs-b>.md
```
---
t: comparison
cat: <category>
tags: [<term-a>, <term-b>]
src: 0
rel: []
upd: <date>
---
> When to choose [[term-a]] over [[term-b]] and vice versa.

## Overview

| | [[term-a]] | [[term-b]] |
|--|--|--|
| Best for | | |
| Throughput | | |
| Latency | | |
| Complexity | | |

## Choose [[term-a]] When
<scenarios>

## Choose [[term-b]] When
<scenarios>

## Sources
- [[source-slug]]: <takeaway>
```

### Source Page — wiki/sources/<slug>.md
```
---
t: source
cat: <category>
tags: [<main vocab terms>]
src: 0
rel: []
upd: <date>
---
> <Title> — <author>, <date>. Key topic: <topic>.

## Summary
- <bullet 1>
- <bullet 2>
- <bullet 3>

## Key Claims
1. <claim>

## Concepts Covered
[[concept-1]], [[concept-2]]

## Entities Mentioned
[[Redis]], [[Kafka]]

## My Notes
<space for manual annotations>
```

### Query Page — wiki/queries/<slug>.md
```
---
t: query
cat: <category>
tags: [<relevant terms>]
src: 0
rel: []
upd: <date>
---
> Answer to: "<original question>"

## Answer
<synthesized answer>

## Pages Consulted
[[page-1]], [[page-2]]

## Confidence
high | medium | low

## Follow-up Questions
- <question worth exploring>
```
