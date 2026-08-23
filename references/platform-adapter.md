# Platform Adapter Table (platform-adapter)

Referenced by the workflow-builder main text "Platform Adapter" and the call protocol of generated artifacts. Maps the DSH-specific mechanisms involved in this skill to their equivalents on mainstream agent platforms such as Codex and Claude Code. When generating each agent charter, replace tool names and file naming according to the target platform.

## Five-Category Mechanism Mapping

| Mechanism | DSH (native to this skill) | Codex CLI | Claude Code | Generic/fallback |
|------|------|------|------|------|
| **Clarification question** | `ask_user_question` (option-style) | Ask an option-style text question directly and wait for the user's reply | Same as left | Any platform can use "option-style questioning" without depending on a tool name |
| **Subagent dispatch** | `subagent` tool (runs in background, returns a subagent id) | No native subagent; community solution: subagents-mcp uses `codex exec --profile <agent>` + AGENTS.md persona to start an independent session | Preset `.claude/agents/<name>.md` subagent, invoked via the Task tool | Hand a self-contained prompt to a "new session/subtask" to execute (one at a time) |
| **Continue conversation / iterate** | `send_message` (continue conversation with the same subagent) | Continue the conversation within the same codex session, or `codex exec --continue` | Continue the conversation within the same session | Continue the conversation within the same session, preserving context |
| **Web search** | `web_search` | No built-in; via MCP/commands or other external tools | WebSearch tool | "Any retrieval capability available in the current environment"; if none, fall back to cascade degradation (see pipeline-design "Generality conventions") |
| **Persist to disk / knowledge base** | Any file read/write | Working-directory file read/write (codex exec session) | Workspace file read/write | Platform working-directory conventions |

## File Naming and Location Adaptation

| Artifact | DSH | Codex | Claude Code | Notes |
|------|------|------|------|------|
| Agent charter | `<root>/agents/<name>/AGENT.md` | `<root>/AGENTS.md` (or `.codex/AGENTS.md` layered) | `.claude/agents/<name>.md` (frontmatter: name/description/tools) | Place by target platform when generating; the charter body (identity/protocol/red lines) is generic |
| Knowledge base | `<root>/agents/<name>/knowledge/*.md` | Same as left (charter references relative paths) | Same as left | Only the path changes with the charter location |
| This skill itself | `~/.dsh/skills/workflow-builder/SKILL.md` | Project `skills/` or `.codex/skills/` (SKILL.md) | `.claude/skills/<name>/SKILL.md` | frontmatter's name+description is the generic contract; metadata/user-invocable are DSH extension fields that other platform parsers usually ignore unknown fields, and under strict validation keep only name+description |

## Adaptation Steps (execute at generation time)
1. When clarifying the blueprint, confirm the target platform (default DSH; user may choose Codex / Claude Code / other).
2. When generating the charter, replace per the table above: tool names, file naming and placement, subagent declaration method.
3. In `<root>/README.md`, note "target platform + equivalents for each mechanism", so any agent can follow it directly.
4. Check before delivery: no platform-specific tool names left over (unless already annotated with equivalents).

## Known Limitations
- Codex's SKILL.md support is experimental (official docs/skills.md, Issue #5291); if the platform does not yet load SKILL.md, this skill's body can be used as a system prompt / AGENTS.md reference.
- Codex has no native multi-subagent parallelism; the pipeline still runs, only concurrency is limited (one subtask at a time).
- Tool capabilities on each platform evolve with versions; the adaptation table is a baseline. At execution time, follow the target platform's current documentation.
