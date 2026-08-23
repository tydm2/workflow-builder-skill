# workflow-builder

[English](./README.md) · [简体中文](./README.zh-CN.md) · [हिन्दी](./README.hi.md) · [Português](./README.pt.md) · [Español](./README.es.md) · [日本語](./README.ja.md) · [Deutsch](./README.de.md) · [Français](./README.fr.md) · [Русский](./README.ru.md) · [한국어](./README.ko.md)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![Version](https://img.shields.io/badge/version-2.0.0-blue.svg)](./CHANGELOG.md)
[![100%25 AI-crafted](https://img.shields.io/badge/100%25-AI--crafted-9cf.svg)](#disclaimer)

**一つのドメインアイデアを、すぐに実行できるマルチエージェントワークフローへ — オーケストレーター「脳」1つ + 専門サブエージェント N 個 + 一言のトリガー。**

`workflow-builder` は、単一のドメイン要件からファイルベースのマルチエージェントパイプラインを組み立てるエージェントスキルです。プランナー脳、専門サブエージェント、エージェントごとの knowledge base、明示的な handoff contract、そして security gate を備えており、どんなエージェントホストでも出力を読み込んで即座に成果を生み出し始められます。

## 特長

多くのマルチエージェントテンプレートは「これが役割、これがプロンプト」で終わってしまいます。このスキルは、5 つの差別化ポイントでさらに先へ進みます。

- **🔒 Security gate（中核）** — 生成されたすべての `AGENT.md` と knowledge ファイルを、prompt injection、悪意ある指示、データ流出、supply-chain poisoning、プラットフォーム安全性についてレビューする独立した配信ゲートです。*さらに独立した 2 回目のレビュー*も行います。コミュニティのテンプレートが自らの生成物を監査することは稀です。
- **🧠 自己進化するサブエージェント** — すべてのサブエージェントには **self-iteration protocol**（feedback-log + usage-log + 5-Why の振り返り + contract freeze）が組み込まれており、生成されたエージェントは凍結されたプロンプトのままでなく、実際の利用から改善を続けます。
- **👥 専門家レベルのエージェント、決めるのはあなた** — 各スペシャリストは **expert panel**（リード 1 名 + シニア役割 2〜4 名、交渉メカニズム付き）か、**単独のシニアエキスパート**のいずれかです。その選択はエビデンス駆動（論文 / 高スターのリポジトリ / コミュニティの合意）で、*あなた*が決めます — デフォルトはありません。
- **🔌 プラットフォーム適応型** — プラットフォームごとのツール対応付きで `AGENT.md`（DSH）、`AGENTS.md`（Codex CLI）、または `.claude/agents/<name>.md`（Claude Code）を出力するため、一つの設計がさまざまなホストで動作します。
- **♻️ Blueprint の再利用 + ADR** — 完成したワークフローは Architecture Decision Records（ADR）付きの再利用可能な blueprint としてアーカイブされ、ワークフロー自体も利用フィードバックから自己進化します。

さらに、任意の**コミュニティスキル調査**（最優秀クラスのコミュニティスキルを、出典を保持したまま抽出）、**作成 + 編集のデュアルモード**、ファイル契約の**単一の真実源（single source of truth）**も備えています。

## 仕組み — 8 つのステップ

1. **Clarify（明確化）** — ドメイン、利用モード（新規 / 編集 / 両方）、ステージ、品質のレッドライン、knowledge の鮮度、コミュニティ調査、トリガーワード、対象プラットフォームについて、選択式の質問を行います。
2. **Community research（任意）** — 優れたコミュニティスキルを見つけ、再利用可能な部分を抽出し、出典を保持し、安全性レビューを実行します。
3. **トポロジの設計** — 脳 1 つ + スペシャリスト 2〜4 名。panel か単独シニアエキスパートかをあなたが選択し、スペシャリストごとに作成/編集を判断します。
4. **Scaffold（雛形生成）** — charter テンプレートから `agents/<name>/AGENT.md` + `knowledge/` を生成します（変数テーブルはエージェントごとに記入）。
5. **knowledge base の作成** — 内蔵型（オフライン）と更新可能型（「最近の更新」セクション付きの検索優先）です。
6. **パイプラインの配線** — handoff contract、README のパイプライン図、トリガーワードレジストリ、ワークフローレベルのログ、blueprint アーカイブ。
7. **Accept & deliver（受け入れと配信）** — 机上ウォークスルー、**その後最初のエンドツーエンドのスモークラン**を行い、ツリー、トリガー、初回実行コマンドを報告します。
8. **Security gate** — すべての charter と knowledge ファイルを 5 つの安全項目について完全レビューし、さらに独立した 2 回目のパスを行います。

## Output

```
your-workflow/
  README.md                  # パイプライン図 + トリガーレジストリ + ADR + 実行時イテレーションプロトコル
  shared/                    # エージェント横断ライブラリ
  agents/<name>/AGENT.md     # charter: アイデンティティ、プロトコル、品質レッドライン、自己イテレーション
  agents/<name>/knowledge/   # 内蔵型 & 更新可能型 knowledge base
  blueprints/<domain>.md     # 再利用可能なトポロジ + ADR 決定記録
  feedback-log.md / usage-log.md  # ワークフローレベルの自己進化
  <stage>/                   # ステージごとのバージョン管理された成果物
```

## Install

```
~/.dsh/skills/workflow-builder/    # グローバル
.dsh/skills/workflow-builder/      # プロジェクト単位
```

その後、*"build me a <domain> workflow"*（「<ドメイン>のワークフローを作って」）、*"set up a plan→execute pipeline"*（「計画→実行パイプラインをセットアップして」）、*"assemble a subagent team"*（「サブエージェントチームを編成して」）といったフレーズで呼び出すか、**set-skill** の `/skill` メニューの項目 ④ から起動します。

## 例

- `references/example-novel-mode.md` — 小説執筆の 3 エージェントパイプライン（Planner → Outliner → Writer）。
- `examples/deep-research-pipeline/` — 完全な charter と knowledge base を備えた自作のディープリサーチパイプライン（Planner → Researcher → Writer → Reviewer）。

## ドキュメント

- `references/pipeline-design.md` — トポロジの方法論、エキスパート形式の選択、knowledge の分割、コミュニティ調査と安全性レビュー
- `references/agent-charter-template.md` — AGENT.md 標準テンプレート
- `references/prompt-craft.md` — プロフェッショナルなサブエージェントプロンプト作成仕様
- `references/platform-adapter.md` — DSH / Codex CLI / Claude Code の対応付け
- `references/contract-spec.md` — ファイル契約の単一の真実源
- `references/blueprint-reuse.md` — blueprint のアーカイブと再利用、ADR、ワークフローレベルの実行時イテレーション

## 連携スキル

このスキルは、スキルの作成と監査を行うメタスキル **[set-skill](https://github.com/tydm2/create-generate-skill)** と連携するよう設計されています。`set-skill` の `/skill` メニューは項目 ④ としてここへルーティングされ、`workflow-builder` はサブエージェントの自己進化のために `set-skill` の feedback-log / usage-log / contract-freeze の仕組みを再利用します。

## 要件

- サブエージェントを実行し、ファイルを読み取れるエージェントホスト — DSH ネイティブ。Codex CLI / Claude Code はアダプター経由。
- コミュニティ調査のためのウェブ検索（任意。利用できない場合は正常に機能を縮退します）。

## 免責事項

> **このスキルは 100% AI によって作成されています。** 問題の発生は避けられません — 議論やプルリクエストを歓迎します。作者は実際の利用状況に基づいて積極的に改善を重ねており、今後も磨き上げを続けます。

## ライセンス

[MIT](./LICENSE)
