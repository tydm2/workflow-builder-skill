# File Contract Specification (contract-spec) — single source of truth

Referenced by `workflow-builder` steps 4/6/7 and the quality red lines. Every generated workflow keeps its **file contracts** here and in the root `README.md` — individual `AGENT.md` files *reference* this document instead of copying it. One change here propagates everywhere.

## 1. Why a single source of truth

When the trigger-word registry, task-package metadata, or naming rules are copied into each charter, they drift apart the first time someone edits one copy. Keeping the canonical definitions in **this file + the root README** and having each charter point to them prevents silent drift and broken handoffs.

## 2. Trigger-word registry (canonical)

The root `README.md` holds a table that is the **only authoritative** mapping of "what the user says → which agent loads":

| Trigger word | Agent | Reads | Writes |
|--------------|-------|-------|--------|
| `plan`, `规划` | planner | — | `outputs/plans/plan-NN.md` |
| `outline`, `大纲` | outliner | `outputs/plans/*.md` | `outputs/outlines/outline-NN.md` |

Rules:
- **Uniqueness**: a trigger word maps to exactly one agent; no two agents share a trigger word.
- **No collision**: trigger words must not collide with the host's built-in commands (e.g. `/skill`, `/iterate`, `git …`).
- **Charter reference**: each AGENT.md's invocation protocol states the trigger words, but the registry itself lives only in the root README. If they ever differ, the registry wins — and the charter should be fixed.

## 3. Task-package metadata (plan vs. edit)

Every task handed from the brain to a specialist is a single file in the stage directory, carrying a `mode` field:

```yaml
# plan-NN.md  (mode: plan  — create from scratch)
mode: plan
brief: <one-line objective>
inputs: [ <file>, ... ]     # optional
keep: []                    # unused in plan mode
change_request: []          # unused in plan mode

# edit-NN.md  (mode: edit  — revise existing artifact)
mode: edit
source_file: <path to the existing file>
change_request: <what to change, itemized>
keep: [ <things that must NOT change> ]
style: <style constraints to preserve>
```

- `plan-NN.md` → the agent produces a new artifact from scratch.
- `edit-NN.md` → the agent reads `source_file`, applies only `change_request`, preserves `keep` and `style`, writes a **new** file (never overwrites the original), and records a **per-change list** in a sibling `-meta.md`.
- `NN` is a zero-padded sequence number (01, 02, …), monotonically increasing per stage.

## 4. Naming rules

| Thing | Rule | Example |
|-------|------|---------|
| Agent directory | kebab-case role | `agents/outline-stitcher/` |
| Charter file | platform convention | `AGENT.md` / `AGENTS.md` / `.claude/agents/<name>.md` |
| Knowledge files | kebab-case, descriptive | `knowledge/community-refs.md` |
| Stage output dir | `outputs/<stage>/` | `outputs/outlines/` |
| Artifact file | `<stage>-NN.md` | `outline-01.md` |
| Edit metadata | `<artifact>-meta.md` | `outline-01-meta.md` |
| Blueprint | `blueprints/<domain>.md` | `blueprints/deep-research.md` |

## 5. Handoff contract fields

Each handoff (written in the upstream agent's charter "Downstream handoff" section) must specify, unambiguously:

1. **Upstream output**: exact location + format the upstream agent writes.
2. **Confirmation gate**: the upstream output is only consumable *after the user confirms it*.
3. **Downstream read**: exact glob/path the downstream agent reads.
4. **Trigger reminder**: the exact phrase the user says to start the downstream stage.
5. **Impact note**: what the downstream must re-check if the upstream output changes afterward.

## 6. How charters reference (not copy)

In an AGENT.md, write only the *reference plus differences*:

> File contracts, the trigger registry, and naming rules are authoritative in the root `README.md` + `references/contract-spec.md`; this charter only states references and differences.

Do not paste the whole registry or naming table into a charter.
