# Knowledge Wiki Setup — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Bootstrap the Knowledge personal LLM wiki with a CLAUDE.md schema, wiki directory structure, initialized index/log, and three project-level skills (`/process`, `/knowledge`, `/lint`).

**Architecture:** All files are markdown. Skills live in `.claude/skills/<name>/SKILL.md` within the project root. CLAUDE.md is the always-on lean context. Wiki pages accumulate in `Wiki/` subdirectories. No code, no dependencies — everything is plain text that Claude reads and writes.

**Tech Stack:** Markdown, Obsidian (for viewing), Claude Code skills system, YAML frontmatter

---

## File Map

| File | Purpose |
|------|---------|
| `CLAUDE.md` | Living schema + lean always-on identity context |
| `Wiki/index.md` | Catalog of all wiki pages — updated every ingest |
| `Wiki/log.md` | Append-only chronological operation log |
| `Wiki/Daily/` | One page per processed daily note |
| `Wiki/Concepts/` | Ideas, frameworks, mental models |
| `Wiki/People/` | People Cole knows or studies |
| `Wiki/Goals/` | Current and past goals |
| `Wiki/Projects/` | Active and archived projects |
| `Wiki/Sources/` | Summaries of articles and content consumed |
| `.claude/skills/process/SKILL.md` | `/process` skill — ingest workflow |
| `.claude/skills/knowledge/SKILL.md` | `/knowledge` skill — context loader |
| `.claude/skills/lint/SKILL.md` | `/lint` skill — health check |

---

## Task 1: Create Directory Structure

**Files:**
- Create: `Wiki/Daily/`, `Wiki/Concepts/`, `Wiki/People/`, `Wiki/Goals/`, `Wiki/Projects/`, `Wiki/Sources/`
- Create: `.claude/skills/process/`, `.claude/skills/knowledge/`, `.claude/skills/lint/`

- [ ] **Step 1: Create wiki subdirectories**

Run from `/Users/coledickerson/Projects/Knowledge`:
```bash
mkdir -p Wiki/Daily Wiki/Concepts Wiki/People Wiki/Goals Wiki/Projects Wiki/Sources
```

- [ ] **Step 2: Create skill directories**

```bash
mkdir -p .claude/skills/process .claude/skills/knowledge .claude/skills/lint
```

- [ ] **Step 3: Initialize git**

The wiki is a git repo — version history is free and lets you see how your knowledge evolves.

```bash
git init && echo ".DS_Store\n.Rhistory" > .gitignore
```

- [ ] **Step 4: Verify structure**

```bash
find Wiki -type d && find .claude/skills -type d
```

Expected output:
```
Wiki
Wiki/Daily
Wiki/Concepts
Wiki/People
Wiki/Goals
Wiki/Projects
Wiki/Sources
.claude/skills
.claude/skills/process
.claude/skills/knowledge
.claude/skills/lint
```

---

## Task 2: Create CLAUDE.md

**Files:**
- Create: `CLAUDE.md`

- [ ] **Step 1: Write CLAUDE.md**

Create `/Users/coledickerson/Projects/Knowledge/CLAUDE.md` with this exact content:

````markdown
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
````

- [ ] **Step 2: Verify CLAUDE.md**

```bash
grep -c "##" CLAUDE.md
```

Expected: `7` (Who I Am, Current Thread, Wiki Schema, Page Types, Naming, Raw Sources, Operations, Schema Evolution — at least 7 `##` headings)

---

## Task 3: Create Wiki/index.md

**Files:**
- Create: `Wiki/index.md`

- [ ] **Step 1: Write Wiki/index.md**

Create `/Users/coledickerson/Projects/Knowledge/Wiki/index.md` with this content:

```markdown
# Wiki Index

*Maintained by Claude. Updated on every /process run. Do not edit manually.*

**Last updated:** 2026-05-20
**Total pages:** 0

---

## Daily

*(none yet)*

## Concepts

*(none yet)*

## People

*(none yet)*

## Goals

*(none yet)*

## Projects

*(none yet)*

## Sources

*(none yet)*
```

- [ ] **Step 2: Verify**

```bash
grep "## " Wiki/index.md
```

Expected output includes: `## Daily`, `## Concepts`, `## People`, `## Goals`, `## Projects`, `## Sources`

---

## Task 4: Create Wiki/log.md

**Files:**
- Create: `Wiki/log.md`

- [ ] **Step 1: Write Wiki/log.md**

Create `/Users/coledickerson/Projects/Knowledge/Wiki/log.md` with this content:

```markdown
# Wiki Log

*Append-only. Format: `## [YYYY-MM-DD] <operation> | <summary>`*
*Quick scan: `grep "^## \[" Wiki/log.md | tail -10`*

---

## [2026-05-20] init | Wiki initialized — CLAUDE.md, index.md, log.md, and skill files created
```

- [ ] **Step 2: Verify**

```bash
grep "^## \[" Wiki/log.md
```

Expected: `## [2026-05-20] init | Wiki initialized — CLAUDE.md, index.md, log.md, and skill files created`

---

## Task 5: Create /process Skill

**Files:**
- Create: `.claude/skills/process/SKILL.md`

- [ ] **Step 1: Write the skill file**

Create `/Users/coledickerson/Projects/Knowledge/.claude/skills/process/SKILL.md` with this content:

````markdown
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
````

- [ ] **Step 2: Verify skill file**

```bash
grep "^name:" .claude/skills/process/SKILL.md
```

Expected: `name: process`

---

## Task 6: Create /knowledge Skill

**Files:**
- Create: `.claude/skills/knowledge/SKILL.md`

- [ ] **Step 1: Write the skill file**

Create `/Users/coledickerson/Projects/Knowledge/.claude/skills/knowledge/SKILL.md` with this content:

````markdown
---
name: knowledge
description: Load relevant wiki context before a conversation or project task. Invoke at the start of any session where you want Claude grounded in your personal knowledge base — ideas, goals, past reflections.
disable-model-invocation: true
---

# Knowledge — Wiki Context Loader

## Current Wiki State
- Index: !`cat Wiki/index.md`
- Recent log: !`grep "^## \[" Wiki/log.md | tail -3`

---

## Step 1: Read Current Thread

Read the **Current Thread** section of `CLAUDE.md`. This is the most recent snapshot of what Cole has been thinking about.

## Step 2: Identify Relevant Pages

Based on the current conversation topic or task (if already stated), identify the 3–5 most relevant pages from the index loaded above.

**If no specific topic is known yet**, default to:
1. The most recent daily note in `Wiki/Daily/`
2. Any active goal pages in `Wiki/Goals/`
3. Any concept pages that appear in Current Thread

## Step 3: Load Pages

Read the identified pages in full.

## Step 4: Acknowledge and Proceed

Say: "Loaded context from your wiki: [list the specific pages read]. What would you like to work on?"

When wiki knowledge is directly relevant to a response, reference it explicitly — e.g., "Based on what you wrote in [[Goal: X]]..." or "This connects to the [[Concepts/Y]] page."

**Do not load more than 5 pages unless Cole explicitly asks for more.** Keep context lean.
````

- [ ] **Step 2: Verify**

```bash
grep "^name:" .claude/skills/knowledge/SKILL.md
```

Expected: `name: knowledge`

---

## Task 7: Create /lint Skill

**Files:**
- Create: `.claude/skills/lint/SKILL.md`

- [ ] **Step 1: Write the skill file**

Create `/Users/coledickerson/Projects/Knowledge/.claude/skills/lint/SKILL.md` with this content:

````markdown
---
name: lint
description: Wiki health check. Run at milestones (~25, ~50, ~100 pages) or when the wiki feels stale. Identifies orphan pages, stale goals, missing connections, and schema improvement opportunities.
disable-model-invocation: true
---

# Lint — Wiki Health Check

## Current Wiki State
- Index: !`cat Wiki/index.md`
- Full log: !`cat Wiki/log.md`

---

## Step 1: Inventory All Pages (use claude-haiku-4-5)

Read every file listed in `Wiki/index.md`. For each page, note:
- Does it have inbound `[[links]]` from other pages?
- Is `connections:` frontmatter populated?
- Is `date:` more than 60 days ago (potentially stale)?
- Does it have `status: active` on a goal that may no longer be current?

## Step 2: Cross-Reference Scan (use claude-haiku-4-5)

Scan all pages for concept names, people, and topics that appear 3+ times across different pages but don't have their own wiki page. List each candidate.

## Step 3: Generate Lint Report (use claude-sonnet-4-6)

Produce a report with these sections:

### Orphan Pages
Pages with no inbound links. For each: suggest a page that should link to it, OR flag for deletion if no longer relevant.

### Stale Content
Pages not updated in 60+ days. For each: note what's likely changed based on newer content.

### Missing Concept Pages
Concepts appearing 3+ times without their own page. List with example occurrences.

### Connection Gaps
Pages with empty `connections:` frontmatter despite visible relationships in the text.

### Schema Suggestions
If patterns suggest a useful new page type or directory, describe it in one sentence.

### Identity Snapshot Check
Compare **Who I Am** in `CLAUDE.md` against themes in recent daily notes. Flag anything outdated.

## Step 4: Append to Wiki/log.md

```
## [YYYY-MM-DD] lint | [N] orphans, [M] stale, [K] missing concepts — see report below

### Lint Report
[paste full report]
```

## Step 5: Summarize to Cole

Present 3–5 bullets with the most actionable findings. Ask: "Which of these would you like to address first?"
````

- [ ] **Step 2: Verify**

```bash
grep "^name:" .claude/skills/lint/SKILL.md
```

Expected: `name: lint`

---

## Task 8: End-to-End Verification

Verify the system works by running a real /process cycle with a sample daily note.

- [ ] **Step 1: Confirm all files exist**

```bash
ls CLAUDE.md Wiki/index.md Wiki/log.md \
  .claude/skills/process/SKILL.md \
  .claude/skills/knowledge/SKILL.md \
  .claude/skills/lint/SKILL.md
```

Expected: all six files listed with no "No such file" errors.

- [ ] **Step 2: Fill in CLAUDE.md Who I Am**

Open `CLAUDE.md` and replace the placeholder block in **Who I Am** with 3–5 sentences describing yourself: your academic context, primary interests, and what you're working toward. This is the only part of the wiki Cole writes directly.

- [ ] **Step 3: Run /process with a sample daily note**

Write a short sample daily note (3–5 sentences of real reflection) and invoke:
```
/process
```

When asked which notes to process, paste the sample content directly.

After the run, verify:
```bash
ls Wiki/Daily/
```
Expected: one `.md` file created with today's date.

```bash
grep "^## \[" Wiki/log.md
```
Expected: two entries — the init entry and a new process entry.

```bash
grep "Total pages:" Wiki/index.md
```
Expected: `Total pages: 1` (or more, depending on what the note contained).

- [ ] **Step 4: Verify CLAUDE.md Current Thread was updated**

```bash
grep -A 6 "## Current Thread" CLAUDE.md
```

Expected: the placeholder bullet is gone and 1–5 real bullets describe recent themes.

- [ ] **Step 5: Run /knowledge**

Invoke:
```
/knowledge
```

Verify that Claude:
1. Lists the pages it loaded
2. References something from your CLAUDE.md or wiki in its response

- [ ] **Step 6: Check Obsidian graph view**

Open the project in Obsidian and go to Graph View. You should see at minimum: the daily note page and any pages it links to, connected by edges.

- [ ] **Step 7: Final file count**

```bash
find Wiki -name "*.md" | wc -l
```

Expected: at least 3 (index.md, log.md, at least one Daily page).
