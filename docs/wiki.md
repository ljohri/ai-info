---
layout: default
title: Personal wiki (Karpathy-style)
---

These are **curated starting points** for setting up a **personal wiki in the spirit of Andrej Karpathy’s “LLM wiki” idea**: mostly **plain Markdown on disk**, **git** for history, **raw sources** kept separate from **synthesized wiki pages**, and an **agent-oriented “schema” file** (instructions for how the wiki is structured and maintained)—often with a coding agent or LLM helping **compile** raw material into linked pages rather than relying on classic RAG over the same corpus on every query.

## Primary source

- **[LLM Wiki (Karpathy gist)](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)** — Original write-up: layered layout (sources vs wiki vs instructions), workflows, and motivation. Copy the pattern and adapt folders and conventions to your stack.
- **Sample `claude.md` (this repository)** — Starter agent instructions you can copy into your own wiki repo (rename to `CLAUDE.md` if your tool expects that). [Browse on GitHub](https://github.com/ljohri/ai-info/blob/main/claude.md) · [Raw file for copy-paste](https://raw.githubusercontent.com/ljohri/ai-info/main/claude.md)

## Explainers and implementations (third party)

These are **not official**; they are useful when you want narrative, diagrams, or worked examples based on the gist above.

- [Andrej Karpathy’s LLM Wiki: Create your own knowledge base](https://medium.com/@urvvil08/andrej-karpathys-llm-wiki-create-your-own-knowledge-base-8779014accd5) (Medium) — Walkthrough of the three-layer pattern.
- [What Is the Karpathy LLM Wiki Pattern?](https://www.mindstudio.ai/blog/karpathy-llm-wiki-pattern-knowledge-base-without-rag/) (MindStudio) — Compares the “compiled wiki” mindset to always-on retrieval.
- [Building and understanding Karpathy’s LLM Wiki](https://www.reddit.com/r/AI_Agents/comments/1sqg5ew/spent_a_weekend_actually_understanding_and/) (Reddit) — First-hand build notes, tradeoffs, and when it works well.

## Tooling people often combine

- **Editor / graph:** [Obsidian](https://obsidian.md/) (local Markdown vault; graph and plugins) — optional; the gist is plain files.
- **Version control:** Git (and GitHub/GitLab) for the wiki repo.
- **Agents:** Whatever you already use (e.g. Claude Code, Codex, or similar) to apply the “schema” and update pages—per your gist-derived `AGENTS.md` / `CLAUDE.md`-style instructions.

## Popular YouTube channels (learning feed)

These are widely used, high-signal **video sources** you can pull ideas from, watch later, or **summarize into your wiki** as raw or distilled notes. They match this page’s focus: **understanding software, models, and research** in depth—not an exhaustive list.

- **[Andrej Karpathy](https://www.youtube.com/@AndrejKarpathy)** — Walkthroughs, LLM/AI engineering, and “from scratch” style explanations; pairs naturally with the LLM-wiki gist.
- **[3Blue1Brown](https://www.youtube.com/@3blue1brown)** — Intuition for math behind ML (vectors, gradient descent, networks) with strong visuals; good to capture as short concept pages in your wiki.
- **[Yannic Kilcher](https://www.youtube.com/@YannicKilcher)** — Paper and architecture deep dives; useful when you want structured notes and citations to primary papers.
- **[Two Minute Papers](https://www.youtube.com/@TwoMinutePapers)** — Short research storylines; a fast way to spot topics worth full write-ups in your wiki.
- **[Computerphile](https://www.youtube.com/@Computerphile)** — Core CS, algorithms, and systems topics that age well in a long-lived notes repo.

Add other links here as the community vet them.
