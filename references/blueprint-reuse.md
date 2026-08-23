# Blueprint Archiving, Reuse & ADR (blueprint-reuse)

Referenced by `workflow-builder` steps 1/6/7 and the "Runtime iteration" section. Covers three things: **blueprint archiving**, **ADR decision records**, and the **workflow-level runtime iteration loop** that keeps a delivered workflow improving after handoff.

## 1. Blueprint — what and why

A **blueprint** is the distilled, reusable description of a finished workflow's topology — independent of any single project's content. When you need a similar workflow again, you reuse the blueprint (topology + agent split + contracts) and only re-answer the differences, instead of re-designing from zero.

## 2. Blueprint file structure

`<root>/blueprints/<domain>.md`, with these sections:

```markdown
# <domain> blueprint

## Topology
<ascii diagram: brain → specialists, with stage order>

## Agent list
| Agent | Form (panel/single) | Duty | Key knowledge |
|-------|---------------------|------|---------------|

## Handoff contracts
<one row per adjacent pair: upstream output → downstream read → trigger>

## ADR (decision records)
<the why-behind-the-why, see §3>

## Reusable parts
<the parts worth copying verbatim into the next similar workflow: role split,
 QA checklists, prompt skeletons, knowledge structures>

## Safety review
<security-gate result + date + second-review date>
```

## 3. ADR — Architecture Decision Records

Each non-obvious design choice gets a one-paragraph ADR so future readers understand *why*, not just *what*:

```markdown
### ADR-NN: <title>
- **Context**: <what situation forced a choice>
- **Options**: <A / B / C, one line each>
- **Decision**: <chosen option>
- **Why**: <the deciding evidence or constraint>
- **Consequences**: <what this costs or forecloses>
```

Record ADRs for decisions like "why 3 specialists not 4", "why the writer is an expert panel while the researcher is single-expert", "why X is a refreshable knowledge base".

## 4. Reuse flow

1. **Before designing** (step 1), list `blueprints/` for a matching domain.
2. If a close-enough blueprint exists, **reuse its topology** and ask the user only about the differences (new stages? different platform? different quality red lines?).
3. Copy the reusable parts; keep the ADRs; write new ADRs only for what actually changes.

## 5. Workflow-level runtime iteration loop

A delivered workflow is **not one-shot** — it self-evolves. Initialize at the generated root:

- `feedback-log.md` — distilled revision requests (two sections: active / consumed; `#`-prefixed lines are hard constraints; same format as set-skill's requirement memory).
- `usage-log.md` — appended usage trail (TRIGGER OK / TRIGGER MISS / LOAD FAIL / EXEC POOR); MISS and POOR are upgraded to 5-Why retrospectives.

**When the user says "optimize / iterate this workflow":**

1. Read `feedback-log.md` unconsumed requirements + `usage-log.md` last 10 entries.
2. Produce an improvement list (diagnosis + suggested change each).
3. User confirms → revise README / the relevant AGENT.md / the blueprint.
4. Re-run the smoke test → mark entries consumed (`[consumed by vX]`).
5. **Freeze the trigger-word registry**; everything else is fair game.

## 6. When to update the blueprint

After any substantial iteration, update `<root>/blueprints/<domain>.md` so the reusable knowledge doesn't live only in the current project's files. Keep the blueprint as the durable "what we learned" artifact that feeds the next similar workflow.
