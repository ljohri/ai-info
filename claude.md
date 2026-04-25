# Sample agent instructions (LLM / personal wiki)

Copy this into your own wiki repository root as `claude.md` or `CLAUDE.md` (tooling varies). Tweak names, paths, and rules to match your layout.

## Purpose

- Your job is to **maintain a compiled Markdown wiki** that summarizes and cross-links material from a separate **raw sources** area.
- Prefer **editing the wiki and sources on disk** over answering from memory alone.
- The wiki is **versioned in git**; make coherent commits when you change multiple related files.

## Directory layout (example—rename to match your repo)

- `raw/` — Ingest: PDFs, transcripts, clippings, notes you do **not** rewrite lightly; new material lands here.
- `wiki/` — The living wiki: small focused pages, cross-links, front matter optional.
- `claude.md` or `CLAUDE.md` — This file: global rules for agents.

## Conventions

- **Wiki page names:** short, `kebab-case` filenames, one main topic per file (e.g. `wiki/attention-patterns.md`).
- **Linking:** use relative links between wiki pages. Link out to `raw/` when pointing at immutable sources.
- **When adding sources:** add under `raw/`, then add or update wiki pages that **summarize** and **link** to the source file path, not a paste of the full source into the wiki (unless a short quote is enough).
- **Orphans:** if you add a new wiki page, link it from at least one other wiki page, or from an index.
- **Tone:** clear, scannable Markdown; `##` headings; bullets for checklists; cite paper titles / URLs in wiki text where relevant.

## Workflows

1. **Ingest** — New material → place in `raw/`, name descriptively, then create/update `wiki/*` to integrate.
2. **Compile** — Read relevant `raw/` + existing `wiki/` before editing; then update the smallest set of files that keep the graph coherent.
3. **Lint (optional self-check before finishing)** — Scan for dead links, orphan pages, and stale "TODO" if your repo uses them.

## What not to do

- Do not delete or rewrite `raw/` to “clean” it unless the user asked.
- Do not invent citations; if something is unknown, mark it in the wiki as open or to verify.
- Do not replace the whole wiki with a single giant file.

---

This file is a **starter** for [ai-info](https://github.com/ljohri/ai-info); replace the paths and project name for your own vault.
