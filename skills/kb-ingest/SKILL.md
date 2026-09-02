---
name: kb-ingest
description: Ingest a source into the wiki — read it in full, compile durable cited pages, and update the index, log, and overview in one commit. Accepts files in raw/, URLs to fetch and archive first, a directory of a code repo read in place, or the answer /kb-ask just gave. Use when the user says ingest, compile this source, add this to the wiki, or pastes a link to read into it.
license: MIT
argument-hint: <raw/file | url | code:<repo>/<dir> | last-answer> [more ...]
---

# kb-ingest

Turn one source into durable, cited pages. Read `./CLAUDE.md` first — it is
this wiki's contract and it wins over anything here.

## Protocol

0. Given a URL rather than a file: fetch the page, keep the part the user
   asked for (one release, one article — not a whole changelog), save it as
   markdown to `raw/YYYY-MM-DD-slug.md` with the URL on line 1 and a note of
   what was excerpted, then continue. Archiving first is what makes the
   citation stable; a bare URL is not a source.
   Given `code:<repo>/<dir>`: `<repo>` is one of the repos `CLAUDE.md` names
   under Code and `<dir>` a path inside it — not inside this wiki. Read every file in that directory in
   place at the pinned tag or commit — nothing is copied into `raw/` — and cite
   every claim by pin, `<repo>@<tag-or-sha>:<path>:<line>`. Stamp the pages
   `verified-at` with that same tag or sha.
   Given "the last answer": the source is the code and pages that answer
   relied on; compile it into pages the same way, with the same pins.
1. Read `./CLAUDE.md`, then `wiki/index.md`. Know what exists before creating
   anything.
2. Read the source COMPLETELY. No skimming, no partial reads.
3. If the source arrived without the standard name or origin header, rename it
   to `YYYY-MM-DD-slug.md` and add the header now. This is the only moment a
   file in `raw/` may be touched; after this ingest it is immutable.
4. Search existing pages for every entity and concept the source raises.
   UPDATE an existing page rather than creating a near-duplicate. Check the
   filename is unique across the whole wiki before creating a page —
   wikilinks resolve by filename, so a duplicate silently breaks them.
5. Write `wiki/sources/<slug>.md`: key claims, the data, the quotes worth
   keeping. This is the evidence layer everything else cites.
6. Create or update entity and concept pages. Keep each page to a single idea;
   when one starts arguing two things, cut it in two.
7. Update `wiki/index.md`.
8. Rewrite `wiki/overview.md` if the source moved the big picture. Rewrite,
   do not append.
9. Append to `wiki/log.md`: what was created, what was updated, and what was
   left open.
10. One commit: `kb(ingest): <source title>`.

## Rules

- Several sources at once: show one filing plan for the whole batch, then run
  the protocol per source in the order given — index and log after each, one
  commit per source, so a bad one can be reverted alone. Do not merge sources
  into one pass; each gets read completely on its own.
- After step 3, never edit, reword, or delete anything in `raw/`.
- Every factual line cites `(raw/<file>)` or a URL. Uncited, it does not ship.
- Preserve the source's own claim before adding your reading of it.
- Contradiction found? Write it on both pages with both citations and mark it
  unresolved. Never silently reconcile.
- Resting on one source? Say `support: one-source`. Do not imply a consensus
  that does not exist.
- Preserve the source's language unless asked otherwise; keep frontmatter
  keys and filenames in English.
- Show a filing plan first when the change set exceeds ~10 files.
