---
layout: default
title: Claude Code — Optimal Usage
permalink: /claude-code-usage.html
---

# Claude Code — Optimal Usage

A curated guide to getting the most out of Claude Code, the AI coding agent that lives in your terminal. Covers workflow practices, key CLI patterns, and pointers to the best community resources.

See also: [Using `claude -p` to generate Rego policies](claude-p-rego.html)

---

## What is Claude Code?

Claude Code is Anthropic's agentic CLI — it reads your codebase, runs shell commands, edits files, and handles git workflows through natural language. It operates interactively (REPL) or non-interactively via `claude -p` (print / pipe mode).

- Official docs: [code.claude.com/docs](https://code.claude.com/docs)
- GitHub: [anthropics/claude-code](https://github.com/anthropics/claude-code)

---

## Core workflow practices

### Context hygiene

- Run `/clear` at the start of every new task — accumulated context slows responses and causes drift.
- Keep `CLAUDE.md` under 200 lines so it loads every session without wasting tokens. Put: common commands, coding standards, important file paths.
- Use **skills** (`.claude/skills/`) for domain knowledge that's only needed sometimes — they load on demand, keeping the base context lean.

### Plan before coding

Use `/plan` or ask Claude to outline its approach before writing code. For anything non-trivial, forcing end-to-end thinking surfaces wrong assumptions before they become wrong code.

### Checkpointing and rewind

`Esc+Esc` (or `/rewind`) opens a scrollable menu of every checkpoint Claude has created mid-session. You can restore the code, the conversation, or both — useful when a direction goes wrong.

### Effort control

`/effort high` or `/effort` triggers adaptive reasoning on Opus 4. Use for hard architecture problems; skip for quick edits to avoid burning tokens.

---

## `claude -p` — print / pipe mode

Print mode is the foundation for all scripting and automation. It processes a single prompt and exits — no interactive TUI.

```bash
# Basic
claude -p "Fix ESLint errors in src/"

# Pipe stdin as context
cat error.log | claude -p "Identify the root cause"

# PR security review
gh pr diff "$PR_NUM" | claude -p \
  --append-system-prompt "You are a security engineer. Review for vulnerabilities."

# Structured JSON output
claude -p "List all exported functions in src/" --output-format json
```

Key flags:

| Flag | Purpose |
|---|---|
| `-p` / `--print` | Non-interactive, single-shot mode |
| `--output-format json` | Structured output for scripting |
| `--output-format stream-json` | Newline-delimited JSON for streaming |
| `--max-turns N` | Cap agentic steps (1 = fast, 3 = thorough) |
| `--append-system-prompt "..."` | Inject extra role/constraints |
| `--bare` | Skip hooks, skills, MCP; faster cold start |

Full reference: [code.claude.com/docs/en/headless](https://code.claude.com/docs/en/headless) · [CLI flags — mager.co](https://www.mager.co/blog/2026-04-17-claude-code-cli-flags/)

Community use-case thread: [anthropics/claude-code #762](https://github.com/anthropics/claude-code/issues/762)

---

## CLAUDE.md — agent instructions

The `CLAUDE.md` (or `claude.md`) at the repo root is loaded at the start of every session. Think of it as a standing brief for the agent: what the project is, what commands to run, what conventions to follow.

Best practices:
- Keep it short — under 200 lines.
- Put build/test/lint commands so Claude can run them without asking.
- Name important files and modules.
- State any conventions (naming, patterns, things to avoid).

---

## YouTube resources

- [Mastering Claude Code in 30 minutes](https://www.youtube.com/watch?v=6eBSHbLKuN0) — compact end-to-end walkthrough.
- [The Only Claude Code Tutorial You Need (2026)](https://www.youtube.com/watch?v=Q0bsphUTLtw) — covers latest updates.
- [Claude Code Tutorial playlist (Net Ninja)](https://www.youtube.com/playlist?list=PL4cUxeGkcC9g4YJeBqChhFJwKQ9TRiivY) — multi-part series from basics to agents.
- [Vibe Coding with Claude Code — Scrimba](https://scrimba.com/articles/best-claude-code-tutorials-and-courses-in-2026/) — interactive browser-based format; pause and edit the instructor's code.

---

## Written guides

- [50 Claude Code Tips and Best Practices](https://www.builder.io/blog/claude-code-tips-best-practices) — Builder.io; thorough daily-use list.
- [Claude Code Tips I Wish I'd Had From Day One](https://marmelab.com/blog/2026/04/24/claude-code-tips-i-wish-id-had-from-day-one.html) — Marmelab; practitioner notes.
- [45 tips — ykdojo/claude-code-tips (GitHub)](https://github.com/ykdojo/claude-code-tips) — includes status line script, system prompt tricks, Gemini CLI as sub-agent.
- [Best Practices — official docs](https://code.claude.com/docs/en/best-practices) — Anthropic's own guide.
- [Complete Claude Code CLI Guide (auto-updated)](https://github.com/Cranot/claude-code-guide) — GitHub repo refreshed every 2 days.

---

## Advanced: `claude -p` for automation pipelines

`claude -p` makes Claude Code a composable Unix utility:

```bash
# Batch: run over every file in a directory
for f in src/**/*.ts; do
  claude -p "Add JSDoc to all exported functions" --file "$f"
done

# CI/CD: post-merge doc generation
git diff HEAD~1 HEAD -- '*.ts' | claude -p \
  "Update CHANGELOG.md based on this diff" --output-format text >> CHANGELOG.md

# Generate structured data from logs
cat deploy.log | claude -p "Extract all error codes as JSON array" \
  --output-format json > errors.json
```

For Rego / OPA policy generation specifically, see the dedicated page: [claude -p for Rego Policy Generation](claude-p-rego.html).
