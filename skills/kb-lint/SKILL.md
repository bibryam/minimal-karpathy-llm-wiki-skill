---
name: kb-lint
description: Health-check a knowledge base and report structural, content, and cross-reference problems by severity — dead links, index drift, orphans, stale claims, unresolved contradictions. Reports only; never repairs without approval. Use when the user says lint, health check, review the wiki, or check consistency.
license: MIT
---

# kb-lint

A report, not a repair. Read `./CLAUDE.md`, then scan. Fix only what the user
approves, item by item.

## Scan

Read `wiki/index.md` and every page it lists.
Build a model of: pages and types, every filename (to catch duplicate
basenames), every internal link and its target, every `sources:` and
`verified-at:` entry, every contradiction note, and every tag.

## Checks

HIGH
- broken link — a link whose target file does not exist
- ambiguous link — two files share a basename, so a wikilink resolves
  unpredictably to one of them
- index drift — a page in `wiki/` missing from `index.md`, or listed but gone
- stale claim — a page asserts X, a newer source in `raw/` contradicts it,
  and the page was never updated
- uncited claim — a factual line in `wiki/` with no citation

MEDIUM
- orphan page — exists, nothing links to it
- missing page — an entity or concept named in 3+ pages with no page of its own
- unresolved contradiction — noted, still open after 2+ ingests
- stale overview — `overview.md` untouched across 3+ ingests
- stale verification — `verified-at` predates the current version of the
  thing the page describes
- isolated cluster — pages that link to each other but not to the rest
- unread source — a file in `raw/` that no wiki page cites. This is the
  un-ingested queue; no inbox directory is needed to track it.

LOW
- empty page — frontmatter, no real body
- one-source concept — evidence is a single source but `support:` claims more
- missing backlink — A links to B, B never links back, and it is relevant
- tag drift — the same concept tagged differently across pages

## Report

Group by severity, numbered, each with the file path and the one-line fix.
End with a count per severity and the three items worth doing first.

## Rules

- Never modify anything during a lint, including "obvious" fixes.
- Propose repairs as a numbered list; apply only the numbers the user names.
- Log the lint in `wiki/log.md` only if repairs were applied.
