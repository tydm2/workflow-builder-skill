# workflow-builder

[English](./README.md) · [简体中文](./README.zh-CN.md) · [हिन्दी](./README.hi.md) · [Português](./README.pt.md) · [Español](./README.es.md) · [日本語](./README.ja.md) · [Deutsch](./README.de.md) · [Français](./README.fr.md) · [Русский](./README.ru.md) · [한국어](./README.ko.md)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![Version](https://img.shields.io/badge/version-2.0.0-blue.svg)](./CHANGELOG.md)
[![100%25 AI-crafted](https://img.shields.io/badge/100%25-AI--crafted-9cf.svg)](#disclaimer)

**Transforme uma ideia de domínio em um workflow multi-agente pronto para executar — 1 cérebro orquestrador + N subagents especialistas + gatilhos de uma frase.**

`workflow-builder` é uma skill de agente que gera a estrutura de um pipeline multi-agente baseado em arquivos a partir de um único requisito de domínio: um cérebro planejador, subagents especialistas, knowledge bases por agente, handoff contracts explícitos e um security gate — para que qualquer host de agente possa carregar o resultado e começar a produzir imediatamente.

## Por que se destaca

A maioria dos templates multi-agente para em "aqui está um papel e um prompt". Esta skill vai além com cinco diferenciais:

- **🔒 Security gate (o principal)** — um gate de entrega independente que revisa cada `AGENT.md` e arquivo de conhecimento gerado em busca de prompt injection, instruções maliciosas, exfiltração de dados, supply-chain poisoning e segurança de plataforma, *além de uma segunda revisão independente*. Os templates da comunidade raramente auditam o que geram.
- **🧠 Subagents auto-evolutivos** — cada subagent acompanha um **protocolo de auto-iteração** (feedback-log + usage-log + retrospectivas dos 5 Porquês + contract freeze), para que um agente gerado continue melhorando com o uso real em vez de permanecer um prompt congelado.
- **👥 Agentes de nível especialista, você decide** — cada especialista é ou um **painel de especialistas** (1 lead + 2–4 papéis sênior com um mecanismo de negociação) ou um **único especialista sênior**; a escolha é baseada em evidências (artigos / repositórios com muitas estrelas / consenso da comunidade) e *você* decide — nunca um padrão.
- **🔌 Adaptativo à plataforma** — emite `AGENT.md` (DSH), `AGENTS.md` (Codex CLI) ou `.claude/agents/<name>.md` (Claude Code) com mapeamento de ferramentas por plataforma, para que um único design funcione em diferentes hosts.
- **♻️ Reuso de blueprint + ADR** — workflows finalizados são arquivados como blueprints reutilizáveis com Architecture Decision Records, e o próprio workflow se auto-evolui a partir do feedback de uso.

E mais: **pesquisa de skills da comunidade** opcional (destilar as melhores skills da comunidade mantendo as fontes), **modo duplo criar + editar** e uma **fonte única da verdade** para os contratos de arquivo.

## Como funciona — 8 etapas

1. **Esclarecer** — perguntas baseadas em opções sobre domínio, modo de uso (novo / editar / ambos), etapas, linhas vermelhas de qualidade, atualidade do conhecimento, pesquisa na comunidade, palavras-gatilho e plataforma de destino.
2. **Pesquisa na comunidade (opcional)** — encontrar as melhores skills da comunidade, destilar as partes reutilizáveis, manter as fontes, executar a revisão de segurança.
3. **Projetar a topologia** — 1 cérebro + 2–4 especialistas; você escolhe painel vs. especialista sênior único; julgamento de criar/editar por especialista.
4. **Scaffold** — gerar `agents/<name>/AGENT.md` + `knowledge/` a partir do template de charter (tabela de variáveis preenchida por agente).
5. **Preencher as knowledge bases** — embutida (offline) e atualizável (busca primeiro, com uma seção de "atualizações recentes").
6. **Conectar o pipeline** — handoff contracts, diagrama de pipeline no README, registro de palavras-gatilho, logs no nível do workflow, arquivo de blueprint.
7. **Aceitar e entregar** — passo a passo no papel **e, em seguida, um primeiro smoke run ponta a ponta**; reportar a árvore, os gatilhos e os comandos de primeira execução.
8. **Security gate** — revisão completa de cada arquivo de charter e de conhecimento para os cinco itens de segurança, além de uma segunda passada independente.

## Saída

```
your-workflow/
  README.md                  # diagrama de pipeline + registro de gatilhos + ADR + protocolo de iteração em runtime
  shared/                    # bibliotecas entre agentes
  agents/<name>/AGENT.md     # charter: identidade, protocolo, linhas vermelhas de qualidade, auto-iteração
  agents/<name>/knowledge/   # knowledge bases embutidas e atualizáveis
  blueprints/<domain>.md     # topologia reutilizável + registros de decisão ADR
  feedback-log.md / usage-log.md  # auto-evolução no nível do workflow
  <stage>/                   # artefatos versionados por etapa
```

## Instalação

```
~/.dsh/skills/workflow-builder/    # global
.dsh/skills/workflow-builder/      # por projeto
```

Em seguida, invoque-a com frases como *"crie para mim um workflow de <domínio>"*, *"configure um pipeline planejar→executar"*, *"monte uma equipe de subagents"* — ou pelo item ④ do menu `/skill` do **set-skill**.

## Exemplos

- `references/example-novel-mode.md` — um pipeline de três agentes para escrever romances (Planner → Outliner → Writer).
- `examples/deep-research-pipeline/` — um pipeline de deep-research construído por conta própria (Planner → Researcher → Writer → Reviewer) com charters e knowledge bases completos.

## Documentação

- `references/pipeline-design.md` — metodologia de topologia, seleção da forma de especialista, divisão de conhecimento, pesquisa na comunidade e revisão de segurança
- `references/agent-charter-template.md` — template padrão de AGENT.md
- `references/prompt-craft.md` — especificação profissional de escrita de prompts para subagents
- `references/platform-adapter.md` — mapeamento DSH / Codex CLI / Claude Code
- `references/contract-spec.md` — fonte única da verdade para contratos de arquivo
- `references/blueprint-reuse.md` — arquivamento e reuso de blueprint, ADR, iteração em runtime no nível do workflow

## Skill complementar

Esta skill foi projetada para trabalhar com o **[set-skill](https://github.com/tydm2/create-generate-skill)** — a meta-skill para criar e auditar skills. O menu `/skill` do `set-skill` direciona para cá como item ④, e o `workflow-builder` reutiliza os mecanismos de feedback-log / usage-log / contract-freeze do `set-skill` para a auto-evolução dos subagents.

## Requisitos

- Um host de agente que possa executar subagents e ler arquivos — nativo do DSH; Codex CLI / Claude Code por meio do adapter.
- Busca na web para pesquisa na comunidade (opcional; degrada suavemente quando indisponível).

## Aviso legal

> **Esta skill é 100% criada por IA.** Problemas são inevitáveis — discussões e pull requests são bem-vindos. O autor itera ativamente sobre ela com base no uso real e continuará refinando-a ao longo do tempo.

## Licença

[MIT](./LICENSE)
