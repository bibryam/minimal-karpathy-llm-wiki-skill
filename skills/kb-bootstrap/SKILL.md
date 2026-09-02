---
name: kb-bootstrap
description: Scaffold a new LLM-maintained markdown wiki — ask what it is about, then write CLAUDE.md, the wiki skeleton, and the first commit. Use when the user asks to start, bootstrap, or set up a new wiki or knowledge base.
license: MIT
argument-hint: [target-directory]
---

# kb-bootstrap

Create one wiki in its own repo. Two layers and a contract, nothing else:
`raw/` holds immutable sources, `wiki/` is yours to maintain, `CLAUDE.md` says
how.

## Ask (one message, and skip whatever you already know)

1. Name, one line on the subject, what it is deliberately NOT about — and,
   if it describes a codebase, the repo path and the tag to pin.
2. Target directory.
3. Any page type beyond sources, entities, concepts, comparisons, synthesis?

That is the whole interview. Do not offer options nobody asked for.

## Then

1. Stop if the target already holds a `CLAUDE.md` or `wiki/`, unless the user
   explicitly approves adopting it.
2. Create `raw/` and `wiki/{sources,entities,concepts,comparisons,synthesis}`,
   each with a `.gitkeep`.
3. Write `CLAUDE.md` from `references/templates/CLAUDE.md.tmpl`: fill in the
   name and subject line, fill or delete the Code section, keep the page
   types chosen, delete the guidance angle-brackets.
4. Copy `index.md`, `log.md`, `overview.md` from `references/templates/` into
   `wiki/`, stamped with today's date.
5. Write `.gitignore` (`.DS_Store`, `.obsidian/workspace*`).
6. `git init`, then commit everything: `kb: bootstrap <name>`.
7. Report the tree, then name the single next action: put a source in `raw/`
   and run `/kb-ingest`.

## Rules

- One wiki per repo.
- No demo content and no placeholder pages. An empty wiki is correct output.
- No search index. Grep carries a few hundred pages; add one past that.
- Resist adding structure the user has not asked for. Everything here is
  easier to add later than to remove.
