# workflow-builder

[English](./README.md) · [简体中文](./README.zh-CN.md) · [हिन्दी](./README.hi.md) · [Português](./README.pt.md) · [Español](./README.es.md) · [日本語](./README.ja.md) · [Deutsch](./README.de.md) · [Français](./README.fr.md) · [Русский](./README.ru.md) · [한국어](./README.ko.md)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![Version](https://img.shields.io/badge/version-2.0.0-blue.svg)](./CHANGELOG.md)
[![100%25 AI-crafted](https://img.shields.io/badge/100%25-AI--crafted-9cf.svg)](#免责声明)

**把一个领域想法变成一条可直接运行的多智能体工作流 —— 1 个大脑 + N 个专精子智能体 + 一句话触发。**

`workflow-builder` 是一个智能体技能（agent skill）：它从单个领域需求出发，脚手架式生成一套基于文件的多智能体流水线——策划大脑、专家子智能体、每个智能体的知识库、显式交接契约，外加一道安全门禁——任何智能体宿主加载后即可立刻开始产出。

## 亮点（凭什么与众不同）

大多数多智能体模板止步于「给你一个角色和一段提示词」。本技能更进一步，有五大差异点：

- **🔒 安全门禁（核心）**——独立的一道交付门禁，审查每一个生成的 `AGENT.md` 与知识库文件，覆盖提示注入、恶意指令、数据外泄、供应链投毒、平台安全五项，并额外做一次独立二次复查。社区模板几乎从不审计它们生成的东西。
- **🧠 会自进化的子智能体**——每个子智能体都带**自我迭代协议**（feedback-log + usage-log + 5-Why 复盘 + 契约冻结），让生成的智能体在真实使用中持续变好，而不是一段冻结的提示词。
- **👥 专家级智能体，由你抉择**——每个专精智能体要么是**专家小组**（1 主角色 + 2–4 位资深专家，带协商机制），要么是**单一资深专家**；选择由证据驱动（论文 / 高星仓库 / 社区共识），最终由你拍板——从不默认。
- **🔌 跨平台适配**——输出 `AGENT.md`（DSH）、`AGENTS.md`（Codex CLI）或 `.claude/agents/<name>.md`（Claude Code），附各平台工具映射，一份设计多宿主通用。
- **♻️ blueprint 复用 + ADR**——交付的工作流归档为可复用 blueprint（含架构决策记录 ADR），且工作流本身会从使用反馈中自进化。

另外还有：可选的**社区技能调研**（提炼社区最优秀的技能精华并保留来源）、**新建 + 编辑双模式**、以及文件契约的**单一事实源**。

## 工作原理 —— 8 步

1. **澄清**——选项式提问：领域、使用方式（新建 / 编辑 / 两者）、阶段划分、质量红线、知识新鲜度、社区调研、触发词、目标平台。
2. **社区调研（可选）**——寻找社区优秀技能，提炼可复用零件，保留来源，执行安全审查。
3. **设计拓扑**——1 个大脑 + 2–4 个专精智能体；你选择小组制还是单一资深专家；每个专精智能体的新建/编辑判断。
4. **生成骨架**——按章程模板生成 `agents/<name>/AGENT.md` + `knowledge/`（逐智能体填写变量表）。
5. **填充知识库**——内置型（离线）与刷新型（先检索、带「最近更新」区）。
6. **接线**——交接契约、README 流水线图、触发词登记表、工作流级日志、blueprint 归档。
7. **验收交付**——纸面走查，**再做一次端到端首跑实测**；汇报目录树、各阶段触发方式与首跑命令。
8. **安全门禁**——对每份章程与知识库做五项安全审查 + 独立二次复查。

## 输出

```
your-workflow/
  README.md                  # 流水线图 + 触发词登记表 + ADR + 运行期迭代协议
  shared/                    # 跨智能体共享库
  agents/<name>/AGENT.md     # 章程：身份、协议、质量红线、自我迭代
  agents/<name>/knowledge/   # 内置型 & 刷新型知识库
  blueprints/<domain>.md     # 可复用拓扑 + ADR 决策记录
  feedback-log.md / usage-log.md  # 工作流级自进化
  <stage>/                   # 各阶段版本化产物
```

## 安装

```
~/.dsh/skills/workflow-builder/    # 全局
.dsh/skills/workflow-builder/      # 项目级
```

然后用「帮我搭一个 <领域> 工作流」「建一条策划→执行流水线」「组建一个子智能体团队」之类的话触发；或经 **set-skill** 的 `/skill` 菜单第 ④ 项进入。

## 示例

- `references/example-novel-mode.md` —— 小说写作三智能体流水线（Planner → Outliner → Writer）。
- `examples/deep-research-pipeline/` —— 自建的深度研究流水线（Planner → Researcher → Writer → Reviewer），含完整章程与知识库。

## 文档

- `references/pipeline-design.md` —— 拓扑方法论、专家形态选择、知识库二分、社区调研与安全审查
- `references/agent-charter-template.md` —— AGENT.md 标准模板
- `references/prompt-craft.md` —— 子代理派发提示词专业写作规范
- `references/platform-adapter.md` —— DSH / Codex CLI / Claude Code 映射
- `references/contract-spec.md` —— 文件契约单一事实源
- `references/blueprint-reuse.md` —— blueprint 归档复用、ADR、工作流级运行期迭代
- `references/example-novel-mode.md` —— 完整实例

## 配套技能

本技能设计为与 **[set-skill](https://github.com/tydm2/create-generate-skill)** 协同——那是创建与体检技能的元技能。`set-skill` 的 `/skill` 菜单第 ④ 项路由到本技能；`workflow-builder` 复用 `set-skill` 的 feedback-log / usage-log / 契约冻结机制来实现子智能体自进化。

## 环境要求

- 一个能运行子代理并读写文件的智能体宿主——DSH 原生；Codex CLI / Claude Code 经适配器。
- 社区调研需联网检索（可选；无检索能力时优雅降级）。

## 免责声明

> **本技能为 100% AI 制作。** 问题在所难免——欢迎讨论与 PR。作者会根据真实使用情况持续迭代优化。

## License

[MIT](./LICENSE)
