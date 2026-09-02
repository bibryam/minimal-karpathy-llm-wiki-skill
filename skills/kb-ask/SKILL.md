---
name: kb-ask
description: Answer a question from a knowledge base, read-only, with citations to wiki pages and raw sources. Use for any question about the wiki's subject matter, or when the user says ask the wiki, what do we know about, or query.
license: MIT
argument-hint: <question>
---

# kb-ask

Read-only. This skill answers; it never writes. If the answer is worth
keeping, offer to file it — filing is `/kb-ingest`'s job or an explicit
follow-up, not a side effect of asking.

## Protocol

1. Read `./CLAUDE.md`, then `wiki/index.md`.
2. Pick candidate pages from the index. Grep `wiki/` for the question's key
   terms to catch what the index does not surface.
3. OPEN and read the candidate pages in full. Follow their links one hop.
4. Fall back to `raw/` when the wiki is thin on the topic — and say so.
5. Answer: conclusion first, then the support. Cite every factual claim as
   `(wiki/concepts/x.md)` or `(raw/<file>)`.
6. State what you could NOT answer, and which source would settle it.

## Rules

- Never answer from a search snippet or a page title. Open the page.
- Separate what the wiki says from what you are inferring across pages.
  Label the inference.
- If two pages disagree, show both with citations and say which is
  better-sourced. Do not resolve it silently.
- A page marked `support: one-source` or `disputed` carries that caveat
  into the answer.
- Answer "the wiki does not cover this" plainly when it does not. Never
  fill the gap from your own priors without labelling it as outside the wiki.
- Write nothing. No new pages, no index update, no log entry.
