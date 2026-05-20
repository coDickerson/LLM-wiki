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

## Step 4: Write Report File and Append to Wiki/log.md

Write the full lint report to `Wiki/lint-YYYY-MM-DD.md` (use today's date). Use this structure:

```
---
type: lint-report
date: YYYY-MM-DD
---
# Lint Report — YYYY-MM-DD

### Orphan Pages
[orphan section]

### Stale Content
[stale section]

### Missing Concept Pages
[missing concepts section]

### Connection Gaps
[gaps section]

### Schema Suggestions
[schema section]

### Identity Snapshot Check
[identity section]
```

Then append ONE line to `Wiki/log.md`:

```
## [YYYY-MM-DD] lint | [N] orphans, [M] stale, [K] missing concepts — full report: [[lint-YYYY-MM-DD]]
```

## Step 5: Summarize to Cole

Present 3–5 bullets with the most actionable findings. Ask: "Which of these would you like to address first?"
