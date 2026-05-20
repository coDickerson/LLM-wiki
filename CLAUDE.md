# Knowledge Wiki

## Who I Am

*[Fill this in now — replace this block with 3–5 sentences about yourself: your academic context, primary interests, and what you're working toward. Update it whenever something meaningfully shifts, not after every session.]*

**Name:** Cole Dickerson
**Context:** UC Berkeley student
**Domains tracked:** Personal · Academic · Career

## Current Thread

*Updated by Claude after each /process run. Max 5 bullets. Reflects recent themes across daily notes.*

- [Not yet populated — run /process with your first daily note to initialize]

---

## Wiki Schema

### Page Types & Locations
| Type | Location | When to create |
|------|----------|----------------|
| `daily` | `Wiki/Daily/YYYY-MM-DD.md` | Every processed daily note |
| `concept` | `Wiki/Concepts/[Name].md` | Ideas, frameworks, mental models |
| `person` | `Wiki/People/[Name].md` | People Cole knows or studies |
| `goal` | `Wiki/Goals/[Name].md` | Goals across personal/academic/career |
| `project` | `Wiki/Projects/[Name].md` | Active and archived projects |
| `source` | `Wiki/Sources/[Title].md` | Articles, papers, content consumed |

### Required Frontmatter (all pages)
```yaml
---
type: daily | concept | person | goal | project | source
date: YYYY-MM-DD
tags: []
connections: []
---
```

### Naming & Linking Rules
- Descriptive titles. No dates in filenames except `Daily/YYYY-MM-DD.md`.
- Always use `[[Page Name]]` when referencing an existing wiki page.
- Err on the side of over-linking.
- After linking two pages, add a one-line synthesis note on the linked page explaining *why* they connect.
- Update `connections:` frontmatter on both pages.

### Raw Sources (read-only)
`Brains/` contains source material — Claude reads but **never writes** here.
- `Brains/Clippings/` — web articles via Obsidian Web Clipper
- `Wiki/YC Descriptions/` — existing job descriptions (process into `Wiki/Sources/` over time)

---

## Operations

Three commands. Always run Claude Code from the project root.

**`/process`** — Ingest daily notes or raw sources. Run after writing a daily note or adding articles to Brains/.

**`/knowledge`** — Load wiki context for the current conversation. Run at the start of sessions where your personal knowledge should inform the work.

**`/lint`** — Wiki health check. Run at ~25, ~50, ~100 pages or whenever the wiki feels stale.

---

## Schema Evolution

After each `/process` run, Claude emits one schema health note. Act on these when they feel obvious, not on a schedule.

Milestones:
- ~25 pages: first `/lint` run, consider formalizing 1–2 page types
- ~50 pages: structured index categories, 3–4 formal page types
- ~100 pages: consider local search tooling (qmd or custom)
- Future: RAG exploration, MCP wiki server
