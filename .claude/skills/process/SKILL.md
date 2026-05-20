---
name: process
description: Ingest daily notes or raw sources into the Knowledge wiki. Creates/updates wiki pages, adds connections, and updates CLAUDE.md Current Thread. Run after writing a daily note or adding content to Brains/.
disable-model-invocation: true
---

# Process — Knowledge Wiki Ingest

## Current Wiki State
- Index: !`cat Wiki/index.md`
- Recent log: !`grep "^## \[" Wiki/log.md | tail -5`

---

## Step 1: Identify Input

Ask Cole: "Which notes or sources should I process? Provide a file path, folder, or paste the content directly."

Wait for the response before continuing.

## Step 2: Read and Extract

Read the provided content. Extract into these categories:

- **Themes** — recurring topics, ideas, frameworks mentioned
- **Reflections** — personal observations, realizations, self-assessments
- **Goals** — anything about what Cole wants to do, become, or achieve
- **People** — anyone mentioned by name or relationship
- **Projects** — any work or initiative referenced
- **Sources** — articles, books, or external content referenced

## Step 3: Create/Update Wiki Pages

### Daily Note → `Wiki/Daily/YYYY-MM-DD.md`

```
---
type: daily
date: YYYY-MM-DD
tags: []
connections: []
---
# YYYY-MM-DD

[Synthesized summary — not a verbatim copy. 2–4 sentences capturing the essence.]

## Themes
- [theme]

## Reflections
[Key personal observations in 2–4 sentences]

## Goals Mentioned
- [[Goal Name]] — [brief note on what was said]

## People
- [[Person Name]] — [context from today]
```

### Concept → `Wiki/Concepts/[Name].md`

```
---
type: concept
date: YYYY-MM-DD
tags: []
connections: []
---
# [Concept Name]

[2–3 sentence definition or description]

## Cole's Take
[Cole's perspective or how this applies to his context]

## Connections
- [[Related Page]] — [one line on the relationship]
```

### Person → `Wiki/People/[Name].md`

```
---
type: person
date: YYYY-MM-DD
tags: []
connections: []
---
# [Person Name]

[Who they are, how Cole knows them, or why they matter]

## Notes
- [observations, interactions, what Cole has learned from or about them]
```

### Goal → `Wiki/Goals/[Name].md`

```
---
type: goal
date: YYYY-MM-DD
tags: [personal | academic | career]
connections: []
status: active
---
# [Goal Name]

[What the goal is in 1–2 sentences]

## Why It Matters
[Cole's stated motivation]

## Progress
- [YYYY-MM-DD]: [note on progress or update]
```

### Source → `Wiki/Sources/[Title].md`

```
---
type: source
date: YYYY-MM-DD
tags: []
connections: []
---
# [Title]

**Source:** [URL or citation]

## Key Takeaways
- [takeaway 1]
- [takeaway 2]

## Cole's Reaction
[What stood out, what it changed or confirmed]

## Connections
- [[Related Page]] — [why connected]
```

### Project → `Wiki/Projects/[Name].md`

```
---
type: project
date: YYYY-MM-DD
tags: []
connections: []
status: active
---
# [Project Name]

[What the project is in 1–2 sentences]

## Goals
[What this project is trying to achieve]

## Progress
- [YYYY-MM-DD]: [note on progress or update]

## Connections
- [[Related Page]] — [why connected]
```

**On existing pages:** if the content adds to an existing page, update it — do not create a duplicate.

## Step 4: Add Wiki Links

On every page created or updated, scan the Current Wiki Index (loaded above) for related pages. Add `[[Page Name]]` links wherever relevant. Over-link rather than under-link.

## Step 5: Update Wiki/index.md

Add new pages under their category. Format:
```
- [[Page Name]] — one-line description
```

Update "Last updated" date and "Total pages" count.

## Step 6: Append to Wiki/log.md

```
## [YYYY-MM-DD] process | Processed [description] — created [N] pages, updated [M] pages
```

## Step 7: Connection Pass (use claude-haiku-4-5)

For each newly created or updated page:
1. Identify 2–3 existing pages it relates to (use the loaded index)
2. On those pages, add under a `## Connections` section:
   > *Connected to [[New Page]] — [one sentence on why]*
3. Update `connections:` frontmatter on both pages

**Use claude-haiku-4-5 for this step.** It's pattern-matching, not synthesis — Haiku is fast and cheap.

## Step 8: Update CLAUDE.md Current Thread

Replace the **Current Thread** section of `CLAUDE.md` with 3–5 bullets based on themes across recently processed notes. Use present-tense, active voice. Replace old bullets entirely — don't accumulate.

Example bullets:
- Exploring the tension between focus and optionality in career decisions
- Processing what it means to be genuinely good at something vs. appearing good
- Returning to [[Concepts/Decision-Making]] through the lens of recent internship research

## Step 9: Schema Health Note

Emit exactly one observation about emerging patterns in the wiki. Examples:
- "You now have 5 pages touching 'ambition' — worth a [[Concepts/Ambition]] page?"
- "Three daily notes mention [[People/[Name]]] — their page may need expansion"
- "Goal pages are growing — a /lint run might be useful soon"
- "Sources are accumulating — consider adding domain tags (ai, psychology, business)"
