# minimal-karpathy-llm-wiki-skill

Four Agent Skills that turn a folder of markdown into a knowledge base an LLM
maintains for you. Sources go in, cited pages come out, and the pages keep
improving as you add more. No database, no embeddings, no runtime code — the
whole thing is markdown in a git repo you own.

Implements the LLM Wiki pattern from
[Karpathy's gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).

## How it works

```text
        you                        the skills                   the wiki

  put a source ────────────>  /kb-ingest ──────────────>  raw/    immutable
  in raw/                     reads it in full,           wiki/   pages, index,
                              writes cited pages,                 log, overview
                              updates index + log

  ask a question ──────────>  /kb-ask ─────────────────>  reads index, opens
                              answers from pages,                pages, cites
                              never writes anything             them back

  every so often ──────────>  /kb-lint ────────────────>  reports contradictions,
                              a report, never a repair          orphans, dead
                                                                links, stale claims

  starting out ────────────>  /kb-bootstrap ───────────>  scaffolds a new wiki
                              three questions, then            + its CLAUDE.md
                              scaffolds + commits              + first commit
```

Every page cites its source, so the wiki stays auditable. Nothing is generated
that you cannot trace back to something in `raw/`.

## Two layers and a contract

```text
  raw/        source documents. Immutable — read from, never modified.
  wiki/       the agent's pages: sources, entities, concepts, comparisons,
              synthesis, plus index.md, log.md, overview.md
  CLAUDE.md   the conventions. The only operating contract.
```

That is the whole model, and it is Karpathy's. Everything else — extra page
types, a concept map, staging directories, zones for your own prose — is
easier to add to a wiki that needs it than to remove from one that does not.

## Install

One command, as a Claude Code plugin:

```text
/plugin marketplace add bibryam/minimal-karpathy-llm-wiki-skill
/plugin install llm-wiki@minimal-karpathy-llm-wiki-skill
```

Skills are then invoked as `/llm-wiki:kb-ingest`, `/llm-wiki:kb-ask`, and so on.

Or per wiki, as plain skills — local and reversible:

```bash
git clone https://github.com/bibryam/minimal-karpathy-llm-wiki-skill.git ~/projects/minimal-karpathy-llm-wiki-skill
mkdir -p <your-wiki>/.claude
ln -s ~/projects/minimal-karpathy-llm-wiki-skill/skills <your-wiki>/.claude/skills
```

## Use

```text
/kb-bootstrap        interview, then scaffold a new wiki and commit it
/kb-ingest <file>    a source becomes cited pages
/kb-ask <question>   a cited answer, read-only
/kb-lint             a health report you approve item by item

(prefixed as /llm-wiki:kb-ingest when installed as a plugin)
```

One wiki per repo. Open the folder in Obsidian if you want graph view and
backlinks — links are `[[wikilinks]]`, so it works with no configuration — but
the wiki is plain markdown and reads fine without it.

## Layout

```text
skills/
  kb-bootstrap/  SKILL.md + references/templates/
  kb-ingest/     SKILL.md
  kb-ask/        SKILL.md
  kb-lint/       SKILL.md
```

MIT licensed — copy it, fork it, use it however you like, no attribution
required.

Check the manifests with `claude plugin validate . --strict`.
