# Konfiguration

Diese Datei beschreibt `.github/classroom-agent.json`, `agent-config.json`, Labels, Commands und CI.

## 1. `.github/classroom-agent.json`

Minimal:

```json
{
  "agent_workflow": {
    "workflow": "project-agent-workflows",
    "exercise": "project"
  }
}
```

Empfohlene Vorlage: [`../vorlagen/classroom-agent.project.json`](../vorlagen/classroom-agent.project.json)

## 2. `/init` und CI-Opt-in

`/init` legt bewusst nur das Basis-Setup an. Wenn der Agent den Dotnet-Build-Workflow in die Setup-PR aufnehmen soll, nutzt in der Projekte-Organisation:

```text
/init --with-dotnet-build
```

Alternativ dauerhaft in `.github/classroom-agent.json`:

```json
{
  "agent_workflow": {
    "workflow": "project-agent-workflows",
    "exercise": "project",
    "init": {
      "includeDotnetBuildWorkflow": true
    }
  }
}
```

Die GitHub App hat in der Projekte-Organisation die noetigen Rechte, um Workflow-Dateien anzulegen. Ohne `--with-dotnet-build` oder JSON-Opt-in bleibt CI trotzdem freiwillig, weil manche Teams eigene Workflows nutzen.

## 3. `agent-config.json`

Ohne Datei nutzt der Agent sichere Defaults. Mit kaputter JSON-Syntax nutzt er ebenfalls Defaults und kommentiert einen Hinweis.

Review-only-Startkonfiguration:

```json
{
  "schemaVersion": 1,
  "features": {
    "autoPrReview": true,
    "labelTriggers": true,
    "kevinSmallImplementations": false
  },
  "policies": {
    "allowFunctionalCodeGeneration": false,
    "maxKevinChangedFiles": 4,
    "maxKevinDiffLines": 250,
    "directBranchUpdates": false
  }
}
```

Vorlage: [`../vorlagen/agent-config.review-only.json`](../vorlagen/agent-config.review-only.json)

## 4. Kevin-Opt-in

Kevin braucht zwei Schalter:

```json
{
  "features": {
    "kevinSmallImplementations": true
  },
  "policies": {
    "allowFunctionalCodeGeneration": true
  }
}
```

Vorlage: [`../vorlagen/agent-config.kevin-opt-in.json`](../vorlagen/agent-config.kevin-opt-in.json)

Weitere Grenzen:

- `maxKevinChangedFiles`: Default `4`
- `maxKevinDiffLines`: Default `250`
- `directBranchUpdates`: muss `false` bleiben

## 5. Wichtige Features

`features.autoPrReview`

- Default: `true`
- Automatische Lisa-Reviews bei PR-Events und Check Completion.
- Wenn `false`, funktionieren manuelle `/review` Commands weiter.

`features.labelTriggers`

- Default: `true`
- `agent:*` Labels starten passende Workflows.

`features.kevinSmallImplementations`

- Default: `false`
- Muss fuer Kevin-Implementierungs-PRs aktiv sein.

## 6. Labels

Issue-Labels:

- `agent:init`
- `agent:breakdown`
- `agent:setup`
- `agent:architecture`
- `agent:tests`
- `agent:docs`
- `agent:ready-check`
- `agent:scaffold`
- `agent:implement-small`

PR-Labels:

- `agent:review`
- `agent:large-review`
- `agent:ready-check`
- `agent:architecture`
- `agent:tests`
- `agent:docs`
- `agent:setup`
- `agent:scaffold`

## 7. CI-Check `dotnet-build`

Der Server erwartet standardmaessig den Check-Namen `dotnet-build`, wenn automatische Reviews auf CI warten sollen.

Vorlage: [`../vorlagen/dotnet-build.yml`](../vorlagen/dotnet-build.yml)

Wenn ihr eigene CI nutzt, dokumentiert sie im README. Fuer Agent-Gating muss der Checkname zur Serverkonfiguration passen.
