# workflow-builder

[English](./README.md) · [简体中文](./README.zh-CN.md) · [हिन्दी](./README.hi.md) · [Português](./README.pt.md) · [Español](./README.es.md) · [日本語](./README.ja.md) · [Deutsch](./README.de.md) · [Français](./README.fr.md) · [Русский](./README.ru.md) · [한국어](./README.ko.md)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![Version](https://img.shields.io/badge/version-2.0.0-blue.svg)](./CHANGELOG.md)
[![100%25 AI-crafted](https://img.shields.io/badge/100%25-AI--crafted-9cf.svg)](#disclaimer)

**Transformez une idée de domaine en un workflow multi-agents prêt à l'emploi — 1 cerveau orchestrateur + N subagents spécialisés + des déclencheurs en une seule phrase.**

`workflow-builder` est une compétence d'agent qui échafaude, à partir d'un unique besoin de domaine, un pipeline multi-agents basé sur des fichiers : un cerveau planificateur, des subagents experts, des knowledge bases par agent, des handoff contracts explicites et une security gate — afin que n'importe quel hôte d'agents puisse charger le résultat et se mettre immédiatement à produire.

## Ce qui le distingue

La plupart des templates multi-agents s'arrêtent à « voici un rôle et un prompt ». Cette compétence va plus loin grâce à cinq différenciateurs :

- **🔒 Security gate (le cœur du dispositif)** — une porte de livraison autonome qui passe en revue chaque `AGENT.md` et chaque fichier de knowledge généré, à la recherche de prompt injection, d'instructions malveillantes, d'exfiltration de données, de supply-chain poisoning et de risques de sécurité de plateforme, *plus une seconde revue indépendante*. Les templates communautaires auditent rarement ce qu'ils génèrent.
- **🧠 Subagents auto-évolutifs** — chaque subagent embarque un **protocole d'auto-itération** (feedback-log + usage-log + rétrospectives des 5 Pourquoi + contract freeze), de sorte qu'un agent généré ne cesse de s'améliorer à partir de l'usage réel au lieu de rester un prompt figé.
- **👥 Agents de niveau expert, à vous de choisir** — chaque spécialiste est soit un **expert panel** (1 lead + 2 à 4 rôles senior avec un mécanisme de négociation), soit un **unique expert senior** ; le choix s'appuie sur des preuves (papers / repos bien notés / consensus communautaire) et c'est *vous* qui décidez — jamais une valeur par défaut.
- **🔌 Adaptatif selon la plateforme** — génère `AGENT.md` (DSH), `AGENTS.md` (Codex CLI) ou `.claude/agents/<name>.md` (Claude Code) avec une correspondance d'outils propre à chaque plateforme, de sorte qu'un même design fonctionne sur tous les hôtes.
- **♻️ Réutilisation de blueprints + ADR** — les workflows terminés sont archivés en blueprints réutilisables accompagnés d'Architecture Decision Records, et le workflow lui-même s'auto-améliore grâce aux retours d'usage.

Et aussi : une **recherche de community skills** optionnelle (distiller les meilleures compétences communautaires en conservant les sources), un **double mode create + edit**, et une **source de vérité unique** pour les contrats de fichiers.

## Comment ça marche — 8 étapes

1. **Clarifier** — des questions à choix sur le domaine, le mode d'usage (nouveau / édition / les deux), les étapes, les lignes rouges de qualité, la fraîcheur des connaissances, la recherche communautaire, les mots déclencheurs et la plateforme cible.
2. **Recherche communautaire (optionnelle)** — trouver les meilleures community skills, en distiller les parties réutilisables, conserver les sources, lancer la revue de sécurité.
3. **Concevoir la topologie** — 1 cerveau + 2 à 4 spécialistes ; vous choisissez panel ou single-senior-expert ; jugement create/edit pour chaque spécialiste.
4. **Échafauder** — générer `agents/<name>/AGENT.md` + `knowledge/` à partir du template de charte (table de variables remplie pour chaque agent).
5. **Remplir les knowledge bases** — intégrées (hors ligne) et actualisables (recherche d'abord, avec une section « recent updates »).
6. **Câbler le pipeline** — handoff contracts, schéma du pipeline dans le README, registre des mots déclencheurs, logs au niveau du workflow, archive de blueprints.
7. **Accepter et livrer** — déroulement sur le papier **puis un premier smoke run de bout en bout** ; rapporter l'arborescence, les déclencheurs et les commandes de premier lancement.
8. **Security gate** — revue complète de chaque fichier de charte et de knowledge pour les cinq points de sécurité, plus une seconde passe indépendante.

## Sortie

```
your-workflow/
  README.md                  # schéma du pipeline + registre des déclencheurs + ADR + protocole d'itération à l'exécution
  shared/                    # bibliothèques partagées entre agents
  agents/<name>/AGENT.md     # charte : identité, protocole, lignes rouges de qualité, auto-itération
  agents/<name>/knowledge/   # knowledge bases intégrées et actualisables
  blueprints/<domain>.md     # topologie réutilisable + registres de décision ADR
  feedback-log.md / usage-log.md  # auto-évolution au niveau du workflow
  <stage>/                   # artefacts versionnés par étape
```

## Installation

```
~/.dsh/skills/workflow-builder/    # globale
.dsh/skills/workflow-builder/      # par projet
```

Invoquez-le ensuite avec des phrases telles que *"build me a <domain> workflow"*, *"set up a plan→execute pipeline"*, *"assemble a subagent team"* — ou via l'élément ④ du menu `/skill` de **set-skill**.

## Exemples

- `references/example-novel-mode.md` — un pipeline à trois agents pour l'écriture de romans (Planner → Outliner → Writer).
- `examples/deep-research-pipeline/` — un pipeline de deep-research auto-construit (Planner → Researcher → Writer → Reviewer) avec des chartes et des knowledge bases complètes.

## Documentation

- `references/pipeline-design.md` — méthodologie de topologie, choix de la forme expert, découpage des knowledge, recherche communautaire et revue de sécurité
- `references/agent-charter-template.md` — template standard AGENT.md
- `references/prompt-craft.md` — spécification professionnelle de rédaction de prompts pour subagents
- `references/platform-adapter.md` — correspondance DSH / Codex CLI / Claude Code
- `references/contract-spec.md` — source de vérité unique pour les contrats de fichiers
- `references/blueprint-reuse.md` — archivage et réutilisation des blueprints, ADR, itération à l'exécution au niveau du workflow

## Compétence associée

Cette compétence est conçue pour fonctionner avec **[set-skill](https://github.com/tydm2/create-generate-skill)** — la méta-compétence dédiée à la création et à l'audit de compétences. Le menu `/skill` de `set-skill` achemine ici en tant qu'élément ④, et `workflow-builder` réutilise les mécanismes feedback-log / usage-log / contract-freeze de `set-skill` pour l'auto-évolution des subagents.

## Prérequis

- Un hôte d'agents capable d'exécuter des subagents et de lire des fichiers — natif sur DSH ; Codex CLI / Claude Code via l'adaptateur.
- Une recherche web pour la recherche communautaire (optionnelle ; se dégrade proprement en cas d'indisponibilité).

## Avertissement

> **Cette compétence est conçue à 100 % par l'IA.** Les problèmes sont inévitables — les discussions et les pull requests sont les bienvenues. L'auteur l'itère activement en fonction de l'usage réel et continuera à la peaufiner au fil du temps.

## Licence

[MIT](./LICENSE)
