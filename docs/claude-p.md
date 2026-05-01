---
layout: default
title: claude -p — Print & Pipe Mode
permalink: /claude-p.html
---

# `claude -p` — Print & Pipe Mode

Deep-dive on `claude -p` (the `--print` flag): non-interactive, scriptable, composable. The foundation for all Claude Code automation.

Parent: [Claude Code — Optimal Usage](claude-code-usage.html)

---

## What it does

`claude -p` processes a single prompt and exits — no interactive TUI, no waiting for input. This makes it a Unix-style utility you can pipe into, pipe out of, and chain with other tools.

```bash
claude -p "Summarize what this module does"
cat file.ts | claude -p "Find potential bugs"
```

---

## Key flags

| Flag | Purpose |
|---|---|
| `-p` / `--print` | Non-interactive, single-shot mode |
| `--output-format text` | Plain text (default) |
| `--output-format json` | Structured JSON — useful for scripting |
| `--output-format stream-json` | Newline-delimited JSON for streaming pipelines |
| `--max-turns N` | Cap agentic steps; `1` = fast, `3` = thorough |
| `--append-system-prompt "..."` | Inject role or constraints without editing CLAUDE.md |
| `--bare` | Skip hooks, skills, MCP — faster cold start for tight loops |
| `--resume <session>` | Resume a prior session in non-interactive mode |

Full CLI reference: [code.claude.com/docs/en/headless](https://code.claude.com/docs/en/headless)

---

## Patterns

### Pipe any content as context

```bash
cat error.log | claude -p "Identify the root cause and suggest a fix"

git diff HEAD~1 | claude -p "Write a one-paragraph summary of these changes"
```

### Role injection via system prompt

```bash
gh pr diff "$PR_NUM" | claude -p \
  --append-system-prompt "You are a security engineer. Review only for vulnerabilities."
```

### Structured output for scripting

```bash
claude -p "List all exported functions in src/auth.ts" --output-format json \
  | jq '.[]'
```

### Batch processing

```bash
for f in src/**/*.ts; do
  echo "=== $f ==="
  cat "$f" | claude -p "Identify any TODOs and explain their context" --bare
done
```

### CI/CD integration

```bash
# Post-merge changelog update
git diff HEAD~1 HEAD -- '*.ts' | claude -p \
  "Append a new entry to CHANGELOG.md based on this diff" \
  --output-format text >> CHANGELOG.md

# Automated code review on PR
gh pr diff "$PR_NUM" | claude -p \
  "Review this diff for correctness and suggest improvements" \
  --output-format text
```

---

## Output format notes

- `text` — clean prose, safe to display or append to a file.
- `json` — wraps the response in a structured envelope; useful when downstream tooling needs to parse result, cost, and turn metadata.
- `stream-json` — newline-delimited JSON events emitted as Claude generates; good for long-running tasks where you want incremental output.

---

## Gotchas

- `--bare` skips CLAUDE.md, hooks, and skills — use only when you don't need project context.
- Print mode has its own token budget; very long pipes may be truncated. Use `--max-turns 1` to keep it predictable.
- `--resume` in print mode lets you continue a prior conversation non-interactively — useful for multi-step pipelines that need prior context.

Community use-case thread (what people are actually doing with `-p`): [anthropics/claude-code #762](https://github.com/anthropics/claude-code/issues/762)
