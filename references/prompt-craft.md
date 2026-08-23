# Professional Prompt Writing Spec (prompt-craft)

Referenced by the workflow-builder main text Step 4 (scaffold) and the quality red lines. Apply when writing the dispatch prompt for each subagent. Goal: **professional, on-target, and self-contained** — so that a zero-context subagent can also execute without ambiguity.

## 1. Requirement Mapping First (before writing)
First build a "user requirement → prompt landing point" mapping table, then check it item by item after writing:

| User requirement / red line | Prompt landing point |
|------|------|
| <Requirement 1, e.g. "PPT for an investor roadshow"> | <e.g. identity setup + output format section> |
| <Requirement 2> | <landing point> |

Every requirement must have a landing point; no landing point = a missed item. A user requirement that only lives in the main-session conversation and never enters the prompt is treated as non-existent.

## 2. Expert Persona (Identity Setup)
Write "domain + experience evidence + specialty + style + iron rules", and use **verifiable evidence** for experience (years / case counts / outcomes) — reject empty adjectives:
- ❌ "You are an excellent PPT master, please do a good job"
- ✅ "You are a senior designer who has handled 100+ primary-market roadshow decks, skilled in fundraising narrative logic and financial chart visualization, with a conclusion-first style and one argument per page. Iron rule: every page must have a single-sentence, data-backed conclusion."
- **Expert panel mode** (when the form is a panel): write the five persona elements for each role separately + a negotiation protocol (lead role proposes a plan → each expert cross-reviews from their professional angle → lead role adjudicates and merges; conflicts are resolved by the user's red lines).
- **Single senior expert mode**: deepen the five elements further, make the experience evidence more concrete (domain + years + representative outcomes), and don't split roles.

## 3. Structure Spec (Seven-Part)
Every dispatch prompt contains seven parts in order, organized with headings and lists, each item executable:
1. **Identity setup** (five-element persona)
2. **Task definition** (one-sentence artifact + this task's concrete parameters)
3. **Input description** (itemize the injected file contents and formats)
4. **Method / process** (break the domain methodology into executable steps; include the essence distilled from community skills)
5. **Output format** (structure, length, naming, persist-to-disk location)
6. **Quality red lines** (checklist style, each item answerable yes/no)
7. **Failure handling** (what to do when input is missing or ambiguous: mark assumptions or request clarification — never fabricate)

## 4. Professionalization Techniques
- **Embed domain methodology**: the writing agent embeds narrative beats / point-of-view strategy, the PPT agent embeds narrative structure / information-density rules, the research agent embeds a search–cross-validation process. State the methodology's name and steps so the subagent has a clear procedure to follow.
- **One example beats ten explanations**: attach a short, high-quality sample (or a sample fragment) and add one line on why it is good.
- **State negative constraints explicitly**: spell out what not to do (e.g. "no AI-flavored boilerplate, no vague quantifiers like 'many / a few', no fabricated data").
- **Community content must be adapted**: content distilled from community skills must first be rewritten against the user's requirements before injection (see pipeline-design.md "Community Skill Research"); verbatim copying is prohibited.
- **Community content must pass review**: before injection it must pass the pipeline-design.md "Safety & Health Review" checklist (prompt injection / malicious instructions / data exfiltration / licensing / activity); content that fails review must not be injected.

## 5. Self-Containment Check (the last gate before dispatch)
Assume the subagent knows nothing about the main conversation: read through the prompt, and for every reference such as "this", "same as above", or "as mentioned earlier", ask: "the subagent has no main conversation — does it know what this refers to?" If not, complete the injection.

## 6. Anti-Patterns (fix on sight)
- Piling up adjectives with no acceptance criteria ("write it a bit better", "more professional")
- Requirements that only appear in the main-session conversation and never make it into the prompt
- Copying community skill text verbatim without adapting it to the user's specific needs
- Using pronouns like "this content" without attaching the content itself
- Writing red lines as unverifiable phrases like "try to" or "preferably"

## 7. Acceptance Checklist (tick each item before delivery)
- [ ] Requirement mapping table has no gaps (every user requirement has a landing point)
- [ ] Identity setup satisfies the five elements and is professionally distinctive
- [ ] Seven-part structure is complete, and every instruction is executable
- [ ] At least one domain methodology or high-quality sample is embedded
- [ ] Negative constraints are stated explicitly
- [ ] Self-containment check passes (executable with zero context)
- [ ] Expert persona includes verifiable experience evidence (years / case counts), with no empty adjectives
- [ ] Panel form includes a complete negotiation protocol (lead role proposes → experts cross-review → adjudicate); the single senior expert persona has been deeply strengthened
- [ ] Injected community content has passed the safety & health review (see pipeline-design "Safety & Health Review")
