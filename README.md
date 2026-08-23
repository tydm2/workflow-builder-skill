# workflow-builder

[English](./README.md) · [简体中文](./README.zh-CN.md)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![Version](https://img.shields.io/badge/version-2.0.0-blue.svg)](./CHANGELOG.md)
[![100%25 AI-crafted](https://img.shields.io/badge/100%25-AI--crafted-9cf.svg)](#disclaimer)

**Turn one domain idea into a ready-to-run multi-agent workflow — 1 orchestrator brain + N specialist subagents + one-sentence triggers.**

`workflow-builder` is an agent skill that scaffolds a file-based multi-agent pipeline from a single domain requirement: a planner brain, expert subagents, per-agent knowledge bases, explicit handoff contracts, and a security gate — so any agent host can load the output and start producing immediately.

## Why it stands out

Most multi-agent templates stop at "here's a role and a prompt." This skill goes further with five differentiators:

- **🔒 Security gate (the core one)** — a standalone delivery gate that reviews every generated `AGENT.md` and knowledge file for prompt injection, malicious instructions, data exfiltration, supply-chain poisoning, and platform safety, *plus an independent second review*. Community templates rarely audit what they generate.
- **🧠 Self-evolving subagents** — every subagent ships with a **self-iteration protocol** (feedback-log + usage-log + 5-Why retrospectives + contract freeze), so a generated agent keeps improving from real usage instead of staying a frozen prompt.
- **👥 Expert-level agents, your call** — each specialist is either an **expert panel** (1 lead + 2–4 senior roles with a negotiation mechanism) or a **single senior expert**; the choice is evidence-driven (papers / high-star repos / community consensus) and *you* decide — never a default.
- **🔌 Platform-adaptive** — emits `AGENT.md` (DSH), `AGENTS.md` (Codex CLI), or `.claude/agents/<name>.md` (Claude Code) with per-platform tool mapping, so one design works across hosts.
- **♻️ Blueprint reuse + ADR** — finished workflows are archived as reusable blueprints with Architecture Decision Records, and the workflow itself self-evolves from usage feedback.

Plus: optional **community skill research** (distill best-in-class community skills with sources kept), **create + edit dual mode**, and a **single source of truth** for file contracts.

## How it works — 8 steps

1. **Clarify** — option-based questions on domain, usage mode (new / edit / both), stages, quality red lines, knowledge freshness, community research, trigger words, and target platform.
2. **Community research (optional)** — find top community skills, distill reusable parts, keep sources, run the safety review.
3. **Design the topology** — 1 brain + 2–4 specialists; you pick panel vs. single-senior-expert; per-specialist create/edit judgment.
4. **Scaffold** — generate `agents/<name>/AGENT.md` + `knowledge/` from the charter template (variable table filled per agent).
5. **Fill knowledge bases** — built-in (offline) and refreshable (search-first with a "recent updates" section).
6. **Wire the pipeline** — handoff contracts, README pipeline diagram, trigger-word registry, workflow-level logs, blueprint archive.
7. **Accept & deliver** — paper walkthrough **then a first end-to-end smoke run**; report the tree, triggers, and first-run commands.
8. **Security gate** — full review of every charter & knowledge file for the five safety items, plus an independent second pass.

## Output

```
your-workflow/
  README.md                  # pipeline diagram + trigger registry + ADR + runtime iteration protocol
  shared/                    # cross-agent libraries
  agents/<name>/AGENT.md     # charter: identity, protocol, quality red lines, self-iteration
  agents/<name>/knowledge/   # built-in & refreshable knowledge bases
  blueprints/<domain>.md     # reusable topology + ADR decision records
  feedback-log.md / usage-log.md  # workflow-level self-evolution
  <stage>/                   # versioned artifacts per stage
```

## Install

```
~/.dsh/skills/workflow-builder/    # global
.dsh/skills/workflow-builder/      # per project
```

Then invoke it with phrases like *"build me a <domain> workflow"*, *"set up a plan→execute pipeline"*, *"assemble a subagent team"* — or via **set-skill**'s `/skill` menu item ④.

## Examples

- `references/example-novel-mode.md` — a novel-writing three-agent pipeline (Planner → Outliner → Writer).
- `examples/deep-research-pipeline/` — a self-built deep-research pipeline (Planner → Researcher → Writer → Reviewer) with full charters and knowledge bases.

## Docs

- `references/pipeline-design.md` — topology methodology, expert-form selection, knowledge split, community research & safety review
- `references/agent-charter-template.md` — AGENT.md standard template
- `references/prompt-craft.md` — professional subagent prompt-writing spec
- `references/platform-adapter.md` — DSH / Codex CLI / Claude Code mapping
- `references/contract-spec.md` — single source of truth for file contracts
- `references/blueprint-reuse.md` — blueprint archiving & reuse, ADR, workflow-level runtime iteration

## Companion skill

This skill is designed to work with **[set-skill](https://github.com/tydm2/create-generate-skill)** — the meta-skill for creating and auditing skills. `set-skill`'s `/skill` menu routes here as item ④, and `workflow-builder` reuses `set-skill`'s feedback-log / usage-log / contract-freeze mechanisms for subagent self-evolution.

## Requirements

- An agent host that can run subagents and read files — DSH native; Codex CLI / Claude Code via the adapter.
- Web search for community research (optional; degrades gracefully when unavailable).

## Disclaimer

> **This skill is 100% AI-crafted.** Issues are inevitable — discussion and pull requests are welcome. The author actively iterates on it based on real-world usage, and will keep refining it over time.

## License

[MIT](./LICENSE)
