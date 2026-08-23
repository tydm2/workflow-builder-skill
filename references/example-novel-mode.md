# Full Worked Example: Three-Subagent Pipeline for Novel Writing Mode (example-novel-mode)

Referenced by the workflow-builder main document. This is the source case for this skill's methodology, showing a real, production-deployed "orchestrator brain + specialists + one-sentence wiring" structure.

## Scenario
Goal: write web novels for publication on Fanqie Novel (番茄小说). Platform characteristics: algorithm-driven free model with strict rules (three-chance contract system, debut-period read-through rate decides survival, AI-flavored writing strictly prohibited); creation must closely align with data metrics.

## Topology
```
Run planning → Planner (orchestrator brain · planning) → User confirms
        → Build outline → Outliner (specialist · outline skeleton) → User confirms
        → Write first draft / fill in the flesh → Writer (specialist · prose drafting) → User review
        → Publish to Fanqie
```

| Agent | One-line responsibility | Knowledge base type | Trigger word |
|--------|-----------|-----------|--------|
| Planner (orchestrator brain) | Takes the user's direction, outputs a six-section plan (genre / golden finger / 50-chapter framework) | Fully built-in (user explicitly said "no need to search again") | Run planning |
| Outliner | Takes the plan, outputs a stitched worldview + characters + main-line/subplot skeleton | Built-in rules + refreshing-type worldview module library (core duty is to search trending worldviews) | Build outline / stitch worldview |
| Writer | Takes the outline skeleton, outputs 2000–2500-word chapter prose + a continuity log | Built-in craft standards + refreshing-type real-human style corpus | Write first draft / fill in the flesh |

## Key Design Decisions (Transferable Lessons)
1. **Network access is split by core value**: Planner's value is in stably executing the user's intent → built-in, no network; Outliner/Writer's value is in capturing trends (trending worldviews / real-human style) → refreshing type, search first then work on each invocation.
2. **Quality red lines all come from platform rules and user-emphasized points**: for example, "avoid AI-flavored prose, prohibit heavy psychological description" was turned into an 8-item pre-delivery quality checklist (banned-word list + psychological description ≤10% + sentence-length variance); no draft goes out without passing.
3. **File contract chain**: `plans/plan-NN.md → outlines/outline-NN.md → chapters/chNNN.md + continuity.md`; each stage's output is archived with a numbered file, and a new task first reads the archive to avoid duplication.
4. **Stitching = controlled innovation**: Outliner's stitch-protocol defines a five-step method + five-dimension variation (trigger mechanism / cost / social structure / power ceiling / tone — change at least 3 of them), ensuring "borrow from trends without being identical".
5. **Handoff terms written at the end of every charter**: the upstream charter spells out "who is downstream, where output goes, what major changes must trigger a notice", forming a closed loop.
6. **The README is the master navigation**: directory tree + pipeline diagram + trigger word mapping table, so any session that loads it can immediately grasp the whole picture.

## Directory Layout (Excerpt)
```
novel/
  README.md                        # navigation + pipeline diagram + trigger word mapping
  agents/
    planner/AGENT.md + knowledge/ (platform-rules / market-insights / plan-template)
    outliner/AGENT.md + knowledge/ (worldview-modules / outline-rules / stitch-protocol / outline-template)
    writer/AGENT.md + knowledge/ (style-corpus / anti-ai-style / chapter-craft / chapter-template)
  plans/  outlines/  chapters/     # three-stage output archive
```
