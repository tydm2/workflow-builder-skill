# workflow-builder

[English](./README.md) · [简体中文](./README.zh-CN.md) · [हिन्दी](./README.hi.md) · [Português](./README.pt.md) · [Español](./README.es.md) · [日本語](./README.ja.md) · [Deutsch](./README.de.md) · [Français](./README.fr.md) · [Русский](./README.ru.md) · [한국어](./README.ko.md)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![Version](https://img.shields.io/badge/version-2.0.0-blue.svg)](./CHANGELOG.md)
[![100%25 AI-crafted](https://img.shields.io/badge/100%25-AI--crafted-9cf.svg)](#disclaimer)

**하나의 도메인 아이디어를 바로 실행 가능한 멀티에이전트 워크플로로 — 오케스트레이터 브레인 1개 + 전문 subagent N개 + 한 문장 트리거.**

`workflow-builder`는 단 하나의 도메인 요구사항으로부터 파일 기반 멀티에이전트 파이프라인을 스캐폴딩하는 에이전트 스킬입니다. planner 브레인, 전문 subagent, 에이전트별 knowledge base, 명시적인 handoff contract, security gate를 생성하므로, 어떤 에이전트 호스트든 출력물을 로드하여 즉시 작업을 시작할 수 있습니다.

## 차별점

대부분의 멀티에이전트 템플릿은 "역할과 프롬프트" 수준에서 그칩니다. 이 스킬은 다섯 가지 차별점으로 한발 더 나아갑니다.

- **🔒 Security gate (핵심 차별점)** — 생성된 모든 `AGENT.md`와 knowledge 파일을 prompt injection, 악성 지시, 데이터 유출, 공급망 오염, 플랫폼 안전성의 관점에서 검토하는 독립형 전달 게이트이며, *독립적인 2차 검토*까지 포함합니다. 커뮤니티 템플릿은 자신이 생성하는 것을 거의 감사하지 않습니다.
- **🧠 자기 진화하는 subagent** — 모든 subagent가 **자기 반복 프로토콜**(feedback-log + usage-log + 5-Why 회고 + contract freeze)을 내장하므로, 생성된 에이전트는 고정된 프롬프트에 머무르지 않고 실제 사용을 통해 계속 개선됩니다.
- **👥 전문가 수준 에이전트, 선택은 당신 몫** — 각 전문가는 **전문가 패널**(리드 1명 + 협상 메커니즘을 갖춘 시니어 역할 2~4명) 또는 **단일 시니어 전문가** 중 하나입니다. 선택은 근거 기반(논문 / 스타 많은 저장소 / 커뮤니티 합의)으로 이루어지며 *당신*이 결정합니다 — 기본값은 없습니다.
- **🔌 플랫폼 적응형** — 플랫폼별 도구 매핑과 함께 `AGENT.md`(DSH), `AGENTS.md`(Codex CLI), `.claude/agents/<name>.md`(Claude Code)를 생성하므로, 하나의 설계가 여러 호스트에서 동작합니다.
- **♻️ Blueprint 재사용 + ADR** — 완성된 워크플로는 Architecture Decision Records와 함께 재사용 가능한 blueprint로 보관되며, 워크플로 자체도 사용 피드백을 통해 자기 진화합니다.

추가로: 선택적 **커뮤니티 스킬 리서치**(출처를 보존하며 최고 수준의 커뮤니티 스킬을 정제), **생성 + 편집 듀얼 모드**, 그리고 파일 계약을 위한 **단일 진실 공급원(single source of truth)**.

## 작동 방식 — 8단계

1. **요구사항 명확화** — 도메인, 사용 모드(신규 / 편집 / 둘 다), 단계, 품질 레드라인, knowledge 최신성, 커뮤니티 리서치, 트리거 단어, 대상 플랫폼에 대한 선택지 기반 질문.
2. **커뮤니티 리서치 (선택)** — 최고의 커뮤니티 스킬을 찾고, 재사용 가능한 부분을 정제하며, 출처를 보존하고, 안전성 검토를 수행합니다.
3. **토폴로지 설계** — 브레인 1개 + 전문가 2~4명; 패널 vs 단일 시니어 전문가를 선택하고, 전문가별 생성/편집 여부를 판단합니다.
4. **스캐폴딩** — charter 템플릿에서 `agents/<name>/AGENT.md` + `knowledge/`를 생성합니다(에이전트별로 변수 테이블을 채움).
5. **knowledge base 채우기** — 내장형(오프라인)과 갱신형(검색 우선, "최근 업데이트" 섹션 포함).
6. **파이프라인 연결** — handoff contract, README 파이프라인 다이어그램, 트리거 단어 레지스트리, 워크플로 수준 로그, blueprint 아카이브.
7. **수락 및 전달** — 문서 기반 워크스루 **후 첫 엔드투엔드 스모크 실행**; 트리, 트리거, 최초 실행 명령을 보고합니다.
8. **Security gate** — 모든 charter 및 knowledge 파일을 다섯 가지 안전 항목에 대해 전체 검토하고, 독립적인 2차 검토를 추가로 수행합니다.

## 산출물

```
your-workflow/
  README.md                  # 파이프라인 다이어그램 + 트리거 레지스트리 + ADR + 런타임 반복 프로토콜
  shared/                    # 에이전트 간 공용 라이브러리
  agents/<name>/AGENT.md     # charter: 정체성, 프로토콜, 품질 레드라인, 자기 반복
  agents/<name>/knowledge/   # 내장형 및 갱신형 knowledge base
  blueprints/<domain>.md     # 재사용 가능한 토폴로지 + ADR 결정 기록
  feedback-log.md / usage-log.md  # 워크플로 수준 자기 진화
  <stage>/                   # 단계별 버전 관리 산출물
```

## 설치

```
~/.dsh/skills/workflow-builder/    # 전역
.dsh/skills/workflow-builder/      # 프로젝트별
```

그런 다음 *"build me a <domain> workflow"*, *"set up a plan→execute pipeline"*, *"assemble a subagent team"* 같은 문구로 호출하거나, **set-skill**의 `/skill` 메뉴 항목 ④를 통해 호출합니다.

## 예시

- `references/example-novel-mode.md` — 소설 작성 3에이전트 파이프라인 (Planner → Outliner → Writer).
- `examples/deep-research-pipeline/` — 전체 charter와 knowledge base를 갖춘 자체 구축 딥리서치 파이프라인 (Planner → Researcher → Writer → Reviewer).

## 문서

- `references/pipeline-design.md` — 토폴로지 방법론, 전문가 형태 선택, knowledge 분할, 커뮤니티 리서치 및 안전성 검토
- `references/agent-charter-template.md` — AGENT.md 표준 템플릿
- `references/prompt-craft.md` — 전문가 수준의 subagent 프롬프트 작성 명세
- `references/platform-adapter.md` — DSH / Codex CLI / Claude Code 매핑
- `references/contract-spec.md` — 파일 계약의 단일 진실 공급원(single source of truth)
- `references/blueprint-reuse.md` — blueprint 보관 및 재사용, ADR, 워크플로 수준 런타임 반복

## 연계 스킬

이 스킬은 스킬을 생성하고 감사하는 메타 스킬인 **[set-skill](https://github.com/tydm2/create-generate-skill)**과 함께 동작하도록 설계되었습니다. `set-skill`의 `/skill` 메뉴는 항목 ④로 여기로 라우팅되며, `workflow-builder`는 subagent 자기 진화를 위해 `set-skill`의 feedback-log / usage-log / contract-freeze 메커니즘을 재사용합니다.

## 요구 사항

- subagent를 실행하고 파일을 읽을 수 있는 에이전트 호스트 — DSH 네이티브; Codex CLI / Claude Code는 어댑터를 통해.
- 커뮤니티 리서치를 위한 웹 검색 (선택 사항; 사용할 수 없을 때는 정상적으로 기능이 축소됩니다).

## 면책 조항

> **이 스킬은 100% AI가 제작했습니다.** 문제는 불가피합니다 — 토론과 풀 리퀘스트를 환영합니다. 작성자는 실제 사용을 바탕으로 적극적으로 반복 개선하고 있으며, 앞으로도 계속 다듬어 나갈 것입니다.

## 라이선스

[MIT](./LICENSE)
