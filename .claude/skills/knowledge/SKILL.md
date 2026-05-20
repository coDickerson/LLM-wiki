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

When wiki knowledge is directly relevant to a response, reference it explicitly — e.g., "Based on what you wrote in [[Goal Name]]..." or "This connects to the [[Concept Name]] page."

**Do not load more than 5 pages unless Cole explicitly asks for more.** Keep context lean.
