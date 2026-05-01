---
layout: default
title: Personal wiki (Karpathy-style)
---

These are **curated starting points** for setting up a **personal wiki in the spirit of Andrej Karpathy’s “LLM wiki” idea**: mostly **plain Markdown on disk**, **git** for history, **raw sources** kept separate from **synthesized wiki pages**, and an **agent-oriented “schema” file** (instructions for how the wiki is structured and maintained)—often with a coding agent or LLM helping **compile** raw material into linked pages rather than relying on classic RAG over the same corpus on every query. **This repository** includes a starting [**`claude.md`**](https://github.com/ljohri/ai-info/blob/main/claude.md) at the repo root (not under `docs/`) you can open or copy from GitHub.

## Claude Code

- **[Claude Code — Optimal Usage](claude-code-usage.html)** — Workflow practices, CLAUDE.md tips, `claude -p` patterns, and curated YouTube / written resources.
- **[`claude -p` for Rego Policy Generation](claude-p-rego.html)** — Using Claude Code's print/pipe mode to generate OPA Rego from natural language prompts, with system-specific constraints. Research in progress.

## Primary source

- **[LLM Wiki (Karpathy gist)](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)** — Original write-up: layered layout (sources vs wiki vs instructions), workflows, and motivation. Copy the pattern and adapt folders and conventions to your stack.
- **[`claude.md` (sample in this repository)](https://github.com/ljohri/ai-info/blob/main/claude.md)** — Starter agent instructions you can copy into your own wiki (rename to `CLAUDE.md` if your tool expects that). [Raw `claude.md` for copy-paste](https://raw.githubusercontent.com/ljohri/ai-info/main/claude.md).

## Explainers and implementations (third party)

These are **not official**; they are useful when you want narrative, diagrams, or worked examples based on the gist above.

- [Andrej Karpathy’s LLM Wiki: Create your own knowledge base](https://medium.com/@urvvil08/andrej-karpathys-llm-wiki-create-your-own-knowledge-base-8779014accd5) (Medium) — Walkthrough of the three-layer pattern.
- [What Is the Karpathy LLM Wiki Pattern?](https://www.mindstudio.ai/blog/karpathy-llm-wiki-pattern-knowledge-base-without-rag/) (MindStudio) — Compares the “compiled wiki” mindset to always-on retrieval.
- [Building and understanding Karpathy’s LLM Wiki](https://www.reddit.com/r/AI_Agents/comments/1sqg5ew/spent_a_weekend_actually_understanding_and/) (Reddit) — First-hand build notes, tradeoffs, and when it works well.

## Tooling people often combine

- **Editor / graph:** [Obsidian](https://obsidian.md/) (local Markdown vault; graph and plugins) — optional; the gist is plain files.
- **Version control:** Git (and GitHub/GitLab) for the wiki repo.
- **Agents:** Whatever you already use (e.g. Claude Code, Codex, or similar) to apply the “schema” and update pages—per your gist-derived `AGENTS.md` / `CLAUDE.md`-style instructions. This repo’s starting file: **[`claude.md`](https://github.com/ljohri/ai-info/blob/main/claude.md)**.

## YouTube walkthroughs (setting up a Karpathy-style personal wiki)

These are **community videos** that focus on the **LLM / markdown wiki** pattern from Karpathy’s gist—folders, raw vs compiled wiki, agent instructions (`AGENTS.md` / `CLAUDE.md` in your own vault), and workflows. You can line that up with this repo’s **[`claude.md`](https://github.com/ljohri/ai-info/blob/main/claude.md)**. 

- [Karpathy’s LLM Wiki — full beginner setup guide](https://www.youtube.com/watch?v=iXd0t60YmMw) — walkthrough: layout, agent/schema file, and keeping sources separate from the wiki.
- [How to build a personal LLM knowledge base (Karpathy’s method)](https://www.youtube.com/watch?v=VRub1w-APTc) — end-to-end “collect → compile → query” using local Markdown and tooling (e.g. Obsidian-style vaults in many of these walkthroughs).
- [Karpathy’s LLM wiki — goes further than everyone realised](https://www.youtube.com/watch?v=ijBJVzxSBRA) — discussion of the pattern and what people miss when they treat it as “just RAG.”
- [Build a personal knowledge base (using Andrej …)](https://www.youtube.com/watch?v=PPr3BTSlMwY) — setup-angled overview tied to the same compiled-wiki concept.
- [Karpathy’s LLM wiki changes everything (LLM wiki / pattern)](https://www.youtube.com/watch?v=04z2M_Nv_Rk) — explainer of why the “stop re-reading the corpus on every question” idea matters for a personal base.

