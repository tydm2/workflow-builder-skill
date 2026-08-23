# workflow-builder

[English](./README.md) · [简体中文](./README.zh-CN.md) · [हिन्दी](./README.hi.md) · [Português](./README.pt.md) · [Español](./README.es.md) · [日本語](./README.ja.md) · [Deutsch](./README.de.md) · [Français](./README.fr.md) · [Русский](./README.ru.md) · [한국어](./README.ko.md)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![Version](https://img.shields.io/badge/version-2.0.0-blue.svg)](./CHANGELOG.md)
[![100%25 AI-crafted](https://img.shields.io/badge/100%25-AI--crafted-9cf.svg)](#disclaimer)

**Verwandle eine einzelne Domänen-Idee in einen sofort lauffähigen Multi-Agent-Workflow — 1 Orchestrator-Gehirn + N spezialisierte subagents + Ein-Satz-Trigger.**

`workflow-builder` ist ein Agent-Skill, der aus einer einzigen Domänen-Anforderung eine dateibasierte Multi-Agent-Pipeline aufbaut: ein Planner-Gehirn, Experten-subagents, agentenspezifische knowledge bases, explizite handoff contracts und ein security gate — sodass jeder Agent-Host die Ausgabe laden und sofort mit der Produktion beginnen kann.

## Warum es heraussticht

Die meisten Multi-Agent-Vorlagen hören bei „hier ist eine Rolle und ein Prompt" auf. Dieser Skill geht mit fünf Alleinstellungsmerkmalen weiter:

- **🔒 Security gate (das Kernstück)** — ein eigenständiges Liefer-Gate, das jede generierte `AGENT.md`- und Wissensdatei auf prompt injection, bösartige Anweisungen, Datenexfiltration, supply-chain poisoning und Plattformsicherheit prüft, *plus eine unabhängige Zweitprüfung*. Community-Vorlagen prüfen selten, was sie generieren.
- **🧠 Sich selbst weiterentwickelnde subagents** — jeder subagent wird mit einem **Selbst-Iterations-Protokoll** ausgeliefert (feedback-log + usage-log + 5-Why-Retrospektiven + contract freeze), sodass ein generierter Agent sich durch echte Nutzung laufend verbessert, statt ein eingefrorener Prompt zu bleiben.
- **👥 Agents auf Expertenniveau, deine Entscheidung** — jeder Spezialist ist entweder ein **Expertengremium** (1 Lead + 2–4 Senior-Rollen mit einem Verhandlungsmechanismus) oder ein **einzelner Senior-Experte**; die Wahl ist evidenzbasiert (Papers / Repos mit vielen Sternen / Community-Konsens) und *du* entscheidest — niemals eine Standardvorgabe.
- **🔌 Plattform-adaptiv** — erzeugt `AGENT.md` (DSH), `AGENTS.md` (Codex CLI) oder `.claude/agents/<name>.md` (Claude Code) mit plattformspezifischem Tool-Mapping, sodass ein Entwurf auf verschiedenen Hosts funktioniert.
- **♻️ Blueprint-Wiederverwendung + ADR** — fertige Workflows werden als wiederverwendbare blueprints mit Architecture Decision Records archiviert, und der Workflow selbst entwickelt sich anhand des Nutzungsfeedbacks weiter.

Außerdem: optionale **Community-Skill-Recherche** (die besten Community-Skills destillieren, Quellen bleiben erhalten), **Create + Edit als Dual-Modus** und eine **single source of truth** für Datei-Contracts.

## So funktioniert es — 8 Schritte

1. **Klären** — optionenbasierte Fragen zu Domäne, Nutzungsmodus (neu / bearbeiten / beides), Stufen, Qualitäts-Red-Lines, Wissensaktualität, Community-Recherche, Trigger-Wörtern und Zielplattform.
2. **Community-Recherche (optional)** — Top-Community-Skills finden, wiederverwendbare Teile destillieren, Quellen behalten, die Sicherheitsprüfung ausführen.
3. **Topologie entwerfen** — 1 Gehirn + 2–4 Spezialisten; du wählst Gremium vs. einzelner Senior-Experte; Create/Edit-Beurteilung pro Spezialist.
4. **Aufbauen** — generiere `agents/<name>/AGENT.md` + `knowledge/` aus der Charter-Vorlage (Variablentabelle pro Agent ausgefüllt).
5. **Knowledge bases befüllen** — integriert (offline) und aktualisierbar (search-first mit einem Abschnitt „recent updates").
6. **Pipeline verdrahten** — handoff contracts, README-Pipeline-Diagramm, Trigger-Wort-Registry, Workflow-Level-Logs, Blueprint-Archiv.
7. **Abnehmen & ausliefern** — Walkthrough auf dem Papier **dann ein erster End-to-End-Smoke-Run**; den Baum, die Trigger und die Erstlauf-Befehle berichten.
8. **Security gate** — vollständige Prüfung jeder Charter- und Wissensdatei auf die fünf Sicherheitspunkte, plus ein unabhängiger zweiter Durchgang.

## Ausgabe

```
your-workflow/
  README.md                  # Pipeline-Diagramm + Trigger-Registry + ADR + Laufzeit-Iterations-Protokoll
  shared/                    # agentenübergreifende Bibliotheken
  agents/<name>/AGENT.md     # Charter: Identität, Protokoll, Qualitäts-Red-Lines, Selbst-Iteration
  agents/<name>/knowledge/   # integrierte & aktualisierbare knowledge bases
  blueprints/<domain>.md     # wiederverwendbare Topologie + ADR-Entscheidungsprotokolle
  feedback-log.md / usage-log.md  # Selbst-Evolution auf Workflow-Ebene
  <stage>/                   # versionierte Artefakte pro Stufe
```

## Installation

```
~/.dsh/skills/workflow-builder/    # global
.dsh/skills/workflow-builder/      # pro Projekt
```

Rufe ihn dann mit Phrasen auf wie *„baue mir einen <domain>-Workflow"*, *„richte eine Plan→Ausführung-Pipeline ein"*, *„stelle ein subagent-Team zusammen"* — oder über **set-skill**s `/skill`-Menüpunkt ④.

## Beispiele

- `references/example-novel-mode.md` — eine Drei-Agenten-Pipeline zum Romanschreiben (Planner → Outliner → Writer).
- `examples/deep-research-pipeline/` — eine selbstgebaute Deep-Research-Pipeline (Planner → Researcher → Writer → Reviewer) mit vollständigen Charters und knowledge bases.

## Dokumentation

- `references/pipeline-design.md` — Topologie-Methodik, Auswahl der Expertenform, Wissensaufteilung, Community-Recherche & Sicherheitsprüfung
- `references/agent-charter-template.md` — Standardvorlage für AGENT.md
- `references/prompt-craft.md` — Spezifikation für professionelles Prompt-Schreiben von subagents
- `references/platform-adapter.md` — Zuordnung für DSH / Codex CLI / Claude Code
- `references/contract-spec.md` — single source of truth für Datei-Contracts
- `references/blueprint-reuse.md` — Blueprint-Archivierung & -Wiederverwendung, ADR, Laufzeit-Iteration auf Workflow-Ebene

## Begleit-Skill

Dieser Skill ist für die Zusammenarbeit mit **[set-skill](https://github.com/tydm2/create-generate-skill)** konzipiert — dem Meta-Skill zum Erstellen und Prüfen von Skills. Das `/skill`-Menü von `set-skill` leitet hierher als Punkt ④ weiter, und `workflow-builder` nutzt die feedback-log / usage-log / contract-freeze-Mechanismen von `set-skill` für die Selbst-Evolution von subagents wieder.

## Anforderungen

- Ein Agent-Host, der subagents ausführen und Dateien lesen kann — nativ bei DSH; Codex CLI / Claude Code über den Adapter.
- Websuche für die Community-Recherche (optional; fällt bei Nichtverfügbarkeit sauber zurück).

## Haftungsausschluss

> **Dieser Skill ist zu 100 % KI-erstellt.** Probleme sind unvermeidlich — Diskussionen und Pull Requests sind willkommen. Der Autor iteriert aktiv auf Basis der realen Nutzung daran und wird ihn im Laufe der Zeit weiter verfeinern.

## Lizenz

[MIT](./LICENSE)
