# LLM Wiki
  
  Personal knowledge wiki powered by Claude Code. Obsidian for reading, Claude for ingesting and
  connecting. Template that does not include the current state

  ## Commands
  
  Run from the project root in Claude Code:

  - `/process` — ingest a daily note or article into the wiki
  - `/knowledge` — load wiki context at the start of a session
  - `/lint` — health check at ~25, ~50, ~100 pages

  ## Structure

  | Path | Purpose |
  |------|---------|
  | `Wiki/` | All wiki pages (Daily, Concepts, People, Goals, Projects, Sources) |
  | `.claude/skills/` | Claude Code skill definitions |
  | `docs/` | Design spec and implementation plan |
  | `CLAUDE.md` | Personal context + wiki schema (local only, not tracked) |

  ## Not tracked

  `CLAUDE.md` and wiki content pages are intentionally excluded from git — they contain personal
  context and evolve continuously
