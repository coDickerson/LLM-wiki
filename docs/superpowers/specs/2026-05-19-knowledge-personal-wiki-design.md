# PRD: Knowledge — Personal LLM Wiki

**Date:** 2026-05-19
**Author:** Cole Dickerson (with Claude)
**Status:** Approved

---

## Context

Cole is a college student building a personal LLM wiki inspired by Andrej Karpathy's LLM wiki pattern. The project lives in `/Users/coledickerson/Projects/Knowledge` as an Obsidian vault. The goal is to create a system that learns about Cole over time, compounds his knowledge across personal/academic/career domains, and serves as a second brain that Claude can draw on during conversations and project work.

The system starts with low friction (Approach A: lean CLAUDE.md + on-demand skill) but is architected to evolve naturally into a richer schema (Approach B: Karpathy-style typed pages, structured index, lint passes) as the wiki grows — without requiring a painful migration.

---

## Goals

1. **Learn about Cole** — across three simultaneous domains: personal (self-reflection, goals, values, psychology), academic (what he's studying, ideas he's engaging with), and career (opportunities, skills, ambitions).
2. **Build a compounding knowledge bank** — every daily note and article ingested makes the wiki richer, not just bigger. Synthesis accumulates.
3. **Second brain for Claude interactions** — when Cole wants to talk through ideas or work on projects, the wiki provides grounded context without burning excessive tokens.
4. **Daily engagement** — the system should feel rewarding to interact with every day, whether via a 3-minute daily note or a longer article ingest.
5. **Automatic node connections** — as content grows, the graph of connections between ideas, people, goals, and concepts grows automatically.
6. **Graceful capability expansion** — the system can absorb new tools (search, RAG, MCP) without structural rebuilding.

---

## Architecture

### Directory Structure

```
Knowledge/
├── CLAUDE.md                  # Living schema + lean identity context (maintained by Claude)
├── Brains/                    # Raw sources — immutable, Claude reads but never writes
│   └── Clippings/             # Web articles clipped via Obsidian Web Clipper
├── Wiki/                      # LLM-maintained markdown pages
│   ├── index.md               # Catalog of all wiki pages (updated every ingest)
│   ├── log.md                 # Append-only chronological record of all operations
│   ├── Daily/                 # Processed daily notes (one page per day)
│   ├── Concepts/              # Ideas, frameworks, mental models
│   ├── People/                # People Cole knows or studies
│   ├── Goals/                 # Current and past goals across domains
│   ├── Projects/              # Active and archived projects
│   ├── Sources/               # Summaries of articles, papers, content consumed
│   └── YC Descriptions/       # Existing — job descriptions (to be processed into Sources/)
└── llm-wiki.md                # Karpathy's reference doc (read-only reference)
```

### Three Layers

**Raw Sources (`Brains/`):** Cole's curated input — web clippings, job descriptions, articles, notes. Immutable. Claude reads, never writes.

**Wiki (`Wiki/`):** Claude-maintained markdown pages. The persistent, compounding artifact. Cole reads; Claude writes and maintains.

**CLAUDE.md:** The living brain — lean always-on context for every session, plus the schema that tells Claude how to operate. Two sections:
- *Identity snapshot*: who Cole is, current goals, key interests, academic context. Updated when something meaningfully changes (not after every session).
- *Current thread*: what Cole has been thinking about lately, recent themes in daily notes, active intellectual momentum. Updated after each processing run. Short — 3-5 bullets max.

---

## CLAUDE.md Structure

The CLAUDE.md serves two purposes: it's the always-on lightweight context AND the operating schema for the wiki. It must stay lean enough that it doesn't bloat every session.

```markdown
# Knowledge Wiki — Schema & Context

## Who I Am (Identity Snapshot)
[Cole's goals, academic context, key interests — updated by Claude when meaningfully changed]

## Current Thread
[3-5 bullets: what I've been thinking about lately — updated after each /process run]

## Wiki Schema
[Page conventions, frontmatter format, naming rules — stable after initial setup]

## Operations
[How to run /process, /knowledge, /lint — the workflows Claude follows]
```

---

## Page Conventions

All wiki pages get YAML frontmatter from day one. This costs nothing now but prevents painful retrofitting later:

```yaml
---
type: daily | concept | person | goal | project | source
date: YYYY-MM-DD
tags: []
connections: []
---
```

Pages use `[[Wiki Links]]` for cross-references. Claude adds these whenever processing content that touches existing pages.

Naming convention: descriptive, no dates in filenames except for `Daily/YYYY-MM-DD.md`.

---

## Operations

### `/process` — Core ingest workflow

Triggered by Cole when he wants to integrate recent Obsidian daily notes or new raw sources. Steps Claude follows:

1. Read new daily notes or raw sources Cole points it at
2. Extract key themes, reflections, goals mentioned, people referenced
3. Create or update relevant wiki pages (Daily page + any touched Concepts, People, Goals)
4. Add `[[links]]` to cross-referenced pages
5. Update `index.md` with any new pages
6. Append entry to `log.md`
7. Run Haiku connection pass (see Connection System)
8. Update *Current Thread* section of CLAUDE.md
9. Emit one **schema health note** — a single observation about emerging patterns

Model: Sonnet for steps 1–6, 8–9. Haiku for step 7.

### `/knowledge` — On-demand wiki context skill

A Claude Code skill Cole invokes when he wants the wiki to inform a conversation or project. Steps:

1. Read `Wiki/index.md` to identify relevant pages
2. Read CLAUDE.md current thread for recent momentum
3. Load the 3–5 most relevant wiki pages into context
4. Proceed with the conversation or task grounded in that context

Keeps normal sessions lean — wiki only enters context when explicitly invoked.

### `/lint` — Schema health check

Triggered manually at meaningful milestones (~25, ~50, ~100 wiki pages) or on demand. Steps:

1. Scan all pages for: orphan pages, stale goals, contradictions, concepts lacking their own page
2. Flag missing cross-references
3. Suggest new page types if patterns have emerged
4. Propose CLAUDE.md updates if the identity snapshot is stale
5. File a lint report to `log.md`

Model: Haiku for scan, Sonnet for synthesis and suggestions.

### Daily Note Flow

Cole writes daily notes in Obsidian naturally (no required format initially). When ready to process, he triggers `/process` and points it at the notes. Claude reads, extracts, and integrates — Cole never writes the wiki directly.

---

## Connection System

Two-tier approach, cheap to run:

**Tier 1 — Wiki Links (always-on):** Every time Claude touches a page during `/process`, it adds `[[Page Name]]` links to cross-referenced content. This builds the Obsidian graph view organically.

**Tier 2 — Synthesis Notes (Haiku-powered):** After each ingest, Haiku runs a lightweight pass to:
- Identify which existing pages the new content relates to
- Add a one-line synthesis note explaining the connection
- Update the `connections:` frontmatter field

The synthesis notes explain *why* two things are connected, not just that they are.

---

## Evolution Milestones

| Milestone | What changes |
|-----------|-------------|
| Day 1 | CLAUDE.md, index.md, log.md, basic Daily pages |
| ~25 pages | First `/lint` run, schema health review, formalize 1–2 recurring page types |
| ~50 pages | Structured index with categories, 3–4 formal page types |
| ~100 pages | Full Approach B schema, consider local search tool (qmd or custom) |
| Future | RAG exploration, MCP wiki server, Dataview queries on frontmatter |

The schema health note emitted during every `/process` run is the primary driver of organic evolution.

---

## Model Usage

| Task | Model | Reason |
|------|-------|--------|
| Daily note processing | Sonnet | Quality extraction and synthesis |
| Article ingest | Sonnet | Understanding and integration |
| Query answering | Sonnet/Opus | Nuanced responses grounded in wiki |
| Connection synthesis | Haiku | Cheap, runs after every ingest |
| Lint scan | Haiku | Structural pattern detection |
| Lint synthesis | Sonnet | Recommendations require judgment |
| CLAUDE.md updates | Sonnet | Identity/thread updates need care |

---

## What Success Looks Like

**Month 1:** Daily notes are being processed. CLAUDE.md feels like it actually knows Cole. Obsidian graph has 20–30 nodes with visible clusters.

**Month 3:** Wiki has 50+ pages. `/knowledge` makes Claude's responses noticeably more grounded and personal. Schema health notes have prompted 2–3 new page types.

**Month 6:** The wiki is a genuine intellectual asset. Cole can ask "what have I been thinking about X?" and get a real answer from accumulated pages. The system is ready for search tooling exploration.

---

## Out of Scope (for now)

- RAG / vector embeddings — deferred; Cole wants to build this later as a learning project
- Automated daily note processing (no cron jobs) — Cole triggers `/process` manually
- Multi-device sync beyond Obsidian's native handling
- Any web UI or frontend

---

## Files to Create

| File | Purpose |
|------|---------|
| `CLAUDE.md` | Living schema + lean identity context (initial version) |
| `Wiki/index.md` | Empty catalog scaffold |
| `Wiki/log.md` | Initialized with first entry |
| `.claude/skills/process.md` | `/process` skill definition |
| `.claude/skills/knowledge.md` | `/knowledge` skill definition |
| `.claude/skills/lint.md` | `/lint` skill definition |

---

## Verification

1. Write a sample daily note, run `/process`, verify wiki pages created with correct frontmatter and links
2. Invoke `/knowledge`, verify CLAUDE.md and relevant pages load into context correctly
3. Check Obsidian graph view — nodes visible and connected
4. Run `/lint`, verify health report written to `log.md`
5. Confirm CLAUDE.md Current Thread section was updated by the process run
