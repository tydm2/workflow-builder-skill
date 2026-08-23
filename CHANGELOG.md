# Changelog

All notable changes to `workflow-builder` are documented here. The skill follows [Semantic Versioning](https://semver.org/).

## [2.0.0] — English rewrite for GitHub

- Full English translation of `SKILL.md` and all references.
- Added the previously-missing `references/contract-spec.md` and `references/blueprint-reuse.md` (referenced since v1.6.0 but never actually created).
- Added a self-built worked example: `examples/deep-research-pipeline/` (Planner → Researcher → Writer → Reviewer).
- Restructured the repo following popular skill conventions: `README.md` / `LICENSE` / `CHANGELOG.md` / `references/` / `examples/`.
- Added an AI-crafted disclaimer to the README.

## [1.6.0] — Battle-tested iteration

- Clarify now asks usage mode (new / edit-existing / both) + domain-adaptive follow-ups.
- Dual-mode (create/edit) design criteria in the main flow.
- Acceptance upgraded to paper walkthrough + first smoke run.
- Single source of truth for file contracts (`contract-spec.md`).
- Workflow-level runtime iteration loop (feedback-log / usage-log).
- Blueprint archiving + ADR decision records (`blueprint-reuse.md`).
- Security gate adds an independent second review.
- AGENT.md template adds a variable table and edit branch.

## [1.5.0] — Security gate

- Added step 8 "Security gate" (borrowing `crashcartlabs/skill-kit`'s security-review): prompt injection / malicious instructions / data exfiltration / supply-chain poisoning / platform safety, full review before delivery. This is the skill's core differentiator vs. community multi-agent templates.

## [1.4.0] — Self-evolving subagents

- `agent-charter-template` gains a self-iteration protocol (feedback-log + usage-log + 5-Why + contract freeze, reusing set-skill mechanisms).

## [1.3.0] — Platform adaptation

- Added `references/platform-adapter.md` (DSH / Codex CLI / Claude Code mechanism mapping & file naming).

## [1.2.1] — Expert-form selection

- Expert form becomes "evidence suggestion + user decision"; the AI presents trade-offs, the user decides.

## [1.2.0] — Expert-level subagents

- Subagents upgraded to expert level (expert panel vs. single senior expert, evidence-driven), with runtime re-evaluation by "output quality > token cost".

## [1.1.1] — Community research generalized

- Removed dependence on a specific retrieval tool; "any retrieval capability available"; added graceful degradation.

## [1.1.0] — Community skill research

- Added the optional community skill research step + `references/prompt-craft.md`.

## [1.0.0] — Initial release

- Distilled from a novel-writing three-agent pipeline (Planner / Outliner / Writer).
