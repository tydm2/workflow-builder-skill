# workflow-builder

[English](./README.md) · [简体中文](./README.zh-CN.md) · [हिन्दी](./README.hi.md) · [Português](./README.pt.md) · [Español](./README.es.md) · [日本語](./README.ja.md) · [Deutsch](./README.de.md) · [Français](./README.fr.md) · [Русский](./README.ru.md) · [한국어](./README.ko.md)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![Version](https://img.shields.io/badge/version-2.0.0-blue.svg)](./CHANGELOG.md)
[![100%25 AI-crafted](https://img.shields.io/badge/100%25-AI--crafted-9cf.svg)](#disclaimer)

**Convierte una idea de dominio en un flujo de trabajo multiagente listo para ejecutar: 1 cerebro orquestador + N subagents especialistas + disparadores de una sola frase.**

`workflow-builder` es una skill de agente que crea el andamiaje de un pipeline multiagente basado en archivos a partir de un único requisito de dominio: un cerebro planificador, subagents expertos, knowledge bases por agente, handoff contracts explícitos y un security gate — para que cualquier host de agentes pueda cargar el resultado y empezar a producir de inmediato.

## Por qué destaca

La mayoría de las plantillas multiagente se quedan en «aquí tienes un rol y un prompt». Esta skill va más allá con cinco diferenciadores:

- **🔒 Security gate (el núcleo)** — un control de entrega independiente que revisa cada `AGENT.md` y archivo de knowledge generado en busca de inyección de prompts, instrucciones maliciosas, exfiltración de datos, envenenamiento de la cadena de suministro y seguridad de plataforma, *más una segunda revisión independiente*. Las plantillas de la comunidad rara vez auditan lo que generan.
- **🧠 Subagents que evolucionan solos** — cada subagent incluye un **protocolo de autoiteración** (feedback-log + usage-log + retrospectivas 5-Why + contract freeze), de modo que un agente generado sigue mejorando a partir del uso real en lugar de quedarse como un prompt congelado.
- **👥 Agentes de nivel experto, tú decides** — cada especialista es o bien un **panel de expertos** (1 líder + 2–4 roles sénior con un mecanismo de negociación) o bien un **único experto sénior**; la elección se basa en evidencia (papers / repos con muchas estrellas / consenso de la comunidad) y la decides *tú* — nunca una opción por defecto.
- **🔌 Adaptable a la plataforma** — emite `AGENT.md` (DSH), `AGENTS.md` (Codex CLI) o `.claude/agents/<name>.md` (Claude Code) con un mapeo de herramientas por plataforma, para que un mismo diseño funcione en distintos hosts.
- **♻️ Reutilización de blueprints + ADR** — los flujos de trabajo terminados se archivan como blueprints reutilizables con Architecture Decision Records, y el propio flujo de trabajo evoluciona a partir del feedback de uso.

Además: **investigación de skills de la comunidad** opcional (destila las mejores skills de la comunidad manteniendo las fuentes), **modo dual crear + editar** y una **única fuente de verdad** para los contratos de archivos.

## Cómo funciona — 8 pasos

1. **Aclarar** — preguntas basadas en opciones sobre el dominio, el modo de uso (nuevo / editar / ambos), las etapas, las líneas rojas de calidad, la frescura del knowledge, la investigación de la comunidad, las palabras disparadoras y la plataforma objetivo.
2. **Investigación de la comunidad (opcional)** — encuentra las mejores skills de la comunidad, destila las partes reutilizables, conserva las fuentes y ejecuta la revisión de seguridad.
3. **Diseñar la topología** — 1 cerebro + 2–4 especialistas; tú eliges panel vs. único experto sénior; decisión de crear/editar por especialista.
4. **Crear el andamiaje** — genera `agents/<name>/AGENT.md` + `knowledge/` a partir de la plantilla de charter (tabla de variables rellenada por agente).
5. **Rellenar las knowledge bases** — integradas (offline) y actualizables (búsqueda primero, con una sección de «actualizaciones recientes»).
6. **Conectar el pipeline** — handoff contracts, diagrama del pipeline en el README, registro de palabras disparadoras, logs a nivel de flujo de trabajo y archivo de blueprints.
7. **Aceptar y entregar** — un recorrido sobre el papel **y después una primera ejecución de humo de extremo a extremo**; informa del árbol, los disparadores y los comandos de primera ejecución.
8. **Security gate** — revisión completa de cada archivo de charter y de knowledge para los cinco elementos de seguridad, más una segunda pasada independiente.

## Salida

```
your-workflow/
  README.md                  # diagrama del pipeline + registro de disparadores + ADR + protocolo de iteración en tiempo de ejecución
  shared/                    # librerías compartidas entre agentes
  agents/<name>/AGENT.md     # charter: identidad, protocolo, líneas rojas de calidad, autoiteración
  agents/<name>/knowledge/   # knowledge bases integradas y actualizables
  blueprints/<domain>.md     # topología reutilizable + registros de decisión ADR
  feedback-log.md / usage-log.md  # autoevolución a nivel de flujo de trabajo
  <stage>/                   # artefactos versionados por etapa
```

## Instalación

```
~/.dsh/skills/workflow-builder/    # global
.dsh/skills/workflow-builder/      # por proyecto
```

Después invócala con frases como *«constrúyeme un flujo de trabajo de <dominio>»*, *«monta un pipeline plan→ejecución»*, *«arma un equipo de subagents»* — o mediante la opción ④ del menú `/skill` de **set-skill**.

## Ejemplos

- `references/example-novel-mode.md` — un pipeline de tres agentes para escribir novelas (Planner → Outliner → Writer).
- `examples/deep-research-pipeline/` — un pipeline de investigación profunda construido por uno mismo (Planner → Researcher → Writer → Reviewer) con charters y knowledge bases completos.

## Documentación

- `references/pipeline-design.md` — metodología de topología, selección de la forma de experto, división del knowledge, investigación de la comunidad y revisión de seguridad
- `references/agent-charter-template.md` — plantilla estándar de AGENT.md
- `references/prompt-craft.md` — especificación profesional de redacción de prompts para subagents
- `references/platform-adapter.md` — mapeo de DSH / Codex CLI / Claude Code
- `references/contract-spec.md` — única fuente de verdad para los contratos de archivos
- `references/blueprint-reuse.md` — archivado y reutilización de blueprints, ADR e iteración en tiempo de ejecución a nivel de flujo de trabajo

## Skill complementaria

Esta skill está diseñada para funcionar con **[set-skill](https://github.com/tydm2/create-generate-skill)** — la meta-skill para crear y auditar skills. El menú `/skill` de `set-skill` enruta hasta aquí como opción ④, y `workflow-builder` reutiliza los mecanismos feedback-log / usage-log / contract-freeze de `set-skill` para la autoevolución de los subagents.

## Requisitos

- Un host de agentes capaz de ejecutar subagents y leer archivos — DSH de forma nativa; Codex CLI / Claude Code mediante el adaptador.
- Búsqueda web para la investigación de la comunidad (opcional; se degrada de forma elegante cuando no está disponible).

## Aviso legal

> **Esta skill está 100% creada por IA.** Los problemas son inevitables — los debates y las pull requests son bienvenidos. El autor la itera activamente en función del uso real y seguirá refinándola con el tiempo.

## Licencia

[MIT](./LICENSE)
