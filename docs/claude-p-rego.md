---
layout: default
title: claude -p for Rego Policy Generation
permalink: /claude-p-rego.html
---

# `claude -p` for Rego Policy Generation

Research notes and patterns for using `claude -p` (print/pipe mode) to generate [OPA Rego](https://www.openpolicyagent.org/) policies from natural language prompts, constrained by system-specific conventions.

Parent page: [Claude Code — Optimal Usage](claude-code-usage.html)

---

## Why `claude -p` for Rego?

Rego has a steep learning curve ("the Rego tax") — syntax is unfamiliar, default-deny patterns are non-obvious, and test coverage requirements are high. `claude -p` lets you:

- Feed a natural language policy requirement and get runnable Rego back.
- Inject system-specific constraints via `--append-system-prompt` without repeating them on every call.
- Pipe the output directly into your CI/CD validation step (`opa check`, `opa test`).
- Build a repeatable, auditable pipeline: prompt → Rego → test → lint → commit.

---

## Baseline pattern

```bash
# Single policy from a requirement string
echo "Only allow GET requests to /public/* paths" | \
  claude -p \
  --append-system-prompt "$(cat .claude/rego-constraints.md)" \
  --output-format text \
  > policies/http_allow.rego

# Validate immediately
opa check policies/http_allow.rego
opa test policies/
```

The `rego-constraints.md` file (see below) encodes how Rego is used *in your system* so Claude generates compatible code without you repeating it every time.

---

## `rego-constraints.md` — system prompt for your context

Create `.claude/rego-constraints.md` (or use `--append-system-prompt` inline). Include:

```markdown
## Rego generation constraints

- Package prefix: `data.authz.<module>`
- Always include `default allow = false`
- Decision variable is always named `allow` (boolean)
- Input schema: `input.user` (object with `.role`, `.id`), `input.request` (`.method`, `.path`, `.resource`)
- Do NOT use `import future.keywords` — this cluster runs OPA 0.58
- Each policy file must be accompanied by a `_test.rego` with at least one allow and one deny case
- Use `rego.v1` import when targeting OPA >= 0.60 (flag: OPA_VERSION env var)
- Avoid `http.send` in policies (no external calls in decision path)
```

Adjust every line to match your OPA version, input schema, naming conventions, and cluster constraints.

---

## End-to-end pipeline sketch

```bash
#!/usr/bin/env bash
# Usage: ./gen-policy.sh "deny access to /admin for non-admin users"
set -euo pipefail

REQUIREMENT="$1"
MODULE=$(echo "$REQUIREMENT" | claude -p \
  --append-system-prompt "$(cat .claude/rego-constraints.md)
  Respond with only the Rego module, no explanation." \
  --output-format text)

# Write policy
echo "$MODULE" > "policies/${2:-generated}.rego"

# Generate tests
echo "$MODULE" | claude -p \
  --append-system-prompt "Write OPA test file for this Rego policy. Output only Rego." \
  --output-format text > "policies/${2:-generated}_test.rego"

# Validate
opa check policies/
opa test policies/
```

---

## Existing tools and prior art

| Resource | Notes |
|---|---|
| [Void3110/rego-skill](https://github.com/Void3110/rego-skill) | Claude Code skill: generates, reviews, and tests Rego policies with a security checklist |
| [mcpmarket.com — Rego Policy Dev skill](https://mcpmarket.com/tools/skills/rego-policy-development) | Installable skill; follows default-deny, runs `opa check` + `opa test` automatically |
| [Eliminating the Rego tax (Red Hat, 2026)](https://next.redhat.com/2026/03/20/eliminating-the-rego-tax-how-ai-orchestrators-automate-kubernetes-compliance/) | AI orchestrators generating Kubernetes OPA Gatekeeper policies from live cluster state |
| [ARPaCCino paper](https://arxiv.org/html/2507.10584v1) | Agentic-RAG system generating Rego from natural language; Claude Sonnet 4 scored best |
| [awesome-opa](https://github.com/open-policy-agent/awesome-opa) | Curated list of OPA tooling, frameworks, integrations |
| [OPA Rego tutorial — Spacelift](https://spacelift.io/blog/open-policy-agent-rego) | Good refresher on Rego fundamentals before prompting |

---

## Prompt patterns that work well

**Role + constraint injection:**
```
You are an OPA policy engineer. Generate a Rego policy that:
- <requirement>
Constraints: <paste rego-constraints.md or key lines>
Output only the .rego file contents.
```

**Iterative refinement via pipe:**
```bash
# Start from an existing policy and tighten it
cat policies/http_allow.rego | claude -p \
  "Add a rule to also deny requests where input.user.role == 'guest' to /admin paths" \
  --append-system-prompt "$(cat .claude/rego-constraints.md)"
```

**Test generation from policy:**
```bash
cat policies/http_allow.rego | claude -p \
  "Write a complete _test.rego for this policy with allow, deny, and edge cases. Output only Rego."
```

---

## Open questions / future research

- How to feed live OPA bundle context (existing policies, schemas) into the prompt without hitting token limits — consider summary via `--max-turns 1` pre-pass.
- Evaluating output quality: integrate `opa eval` with known fixture inputs as a test harness in CI.
- Version-pinning: how to handle `rego.v1` vs legacy syntax when the cluster OPA version varies across environments.
- RAG vs compiled wiki: is a compiled policy-pattern wiki (this approach) better than always-on retrieval over a policy library? (See ARPaCCino paper above.)
- Using the `rego-skill` as a Claude Code skill vs raw `claude -p` — tradeoffs in composability.
