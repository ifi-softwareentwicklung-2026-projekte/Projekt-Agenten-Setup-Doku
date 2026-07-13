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
    "allowKevinWorkflowChanges": false,
    "maxKevinChangedFiles": 4,
    "maxKevinDiffLines": 250,
    "directBranchUpdates": false
  },
  "documentation": {
    "generator": "doxygen",
    "artifacts": ["html", "pdf"],
    "artifactPaths": {
      "html": "docs/api/html/index.html",
      "pdf": "docs/api/latex/refman.pdf",
      "xml": "docs/api/xml/index.xml",
      "latex": "docs/api/latex/refman.tex"
    },
    "outputDirectory": "docs/api",
    "mainPage": "README.md",
    "documentPublicApi": true,
    "failOnWarnings": true,
    "triggers": {
      "pullRequest": true,
      "manual": true,
      "pushBranches": [],
      "schedule": null
    },
    "maxRuntimeMinutes": 20,
    "retentionDays": 7
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
- `allowKevinWorkflowChanges`: Default `false`; nur fuer bewusst beauftragte Aenderungen unter `.github/workflows/` aktivieren

Kevin fuehrt vor einem Push einen Restore-/Build-/Test-Preflight aus. Schlaegt dieser fehl, erstellt er keine PR.

Workflow-Aenderungen benoetigen zusaetzlich `allowKevinWorkflowChanges=true`. Dieser Schalter bleibt auch in der Kevin-Opt-in-Vorlage sicherheitshalber aus, bis ein konkreter Workflow-Auftrag vorliegt.

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

## 6. Dokumentationsworkflow

Der Root-Schluessel heisst `documentation`. Der interne Name `documentation_config` ist kein gueltiger Schluessel fuer `agent-config.json`.

| Feld | Default | Bedeutung |
|---|---|---|
| `generator` | `doxygen` | bevorzugter Dokumentationsgenerator |
| `artifacts` | `html`, `pdf` | erlaubte Werte: `html`, `pdf`, `xml`, `latex` |
| `artifactPaths` | siehe Beispiel | kanonische repository-relative Zielpfade |
| `outputDirectory` | `docs/api` | gemeinsamer Generator-Ausgabeordner |
| `mainPage` | `README.md` | echte HTML-Startseite |
| `documentPublicApi` | `true` | oeffentliche Typen und Member muessen inhaltlich dokumentiert sein |
| `failOnWarnings` | `true` | Generatorwarnungen lassen CI fehlschlagen |
| `triggers.pullRequest` | `true` | Lauf bei Pull Requests |
| `triggers.manual` | `true` | manueller `workflow_dispatch` |
| `triggers.pushBranches` | `[]` | optionale Push-Branches, zum Beispiel `["main"]` |
| `triggers.schedule` | `null` | optionaler Cron-Ausdruck in UTC, zum Beispiel `"0 3 * * *"` |
| `maxRuntimeMinutes` | `20` | erlaubter Bereich `5` bis `30` Minuten |
| `retentionDays` | `7` | erlaubter Bereich `1` bis `30` Tage |

Absolute Pfade, `../`-Traversal, unbekannte Artefakte, unsichere Branchnamen und ungueltige Cron-Felder werden auf sichere Werte zurueckgesetzt. Sind alle Ausloeser deaktiviert, bleibt der manuelle Start als Fallback aktiv.

Text hinter `/docs` darf diese Werte fuer die aktuelle Antwort ueberschreiben. Die konfigurierte maximale Laufzeit darf dabei nur verkuerzt, nicht erhoeht werden. Bei Abweichungen zeigt Maria einen JSON-Patch mit Pfaden wie `/documentation/artifacts`.

Sicherheitsvertrag fuer erzeugte Workflows:

- `permissions: contents: read`
- Checkout mit `persist-credentials: false`
- abbrechbare Concurrency
- explizites `timeout-minutes`
- begrenztes `retention-days`
- `apt-get --no-install-recommends`, kein `texlive-full`
- keine LaTeX-Installation fuer HTML-only
- Doxygen mit README-Hauptseite, `EXTRACT_ALL = NO`, Warnungen fuer undokumentierte APIs und `WARN_AS_ERROR`

## 7. Labels

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

## 8. CI-Check `dotnet-build`

Der Server erwartet standardmaessig den Check-Namen `dotnet-build`, wenn automatische Reviews auf CI warten sollen.

Vorlage: [`../vorlagen/dotnet-build.yml`](../vorlagen/dotnet-build.yml)

Wenn ihr eigene CI nutzt, dokumentiert sie im README. Fuer Agent-Gating muss der Checkname zur Serverkonfiguration passen.
