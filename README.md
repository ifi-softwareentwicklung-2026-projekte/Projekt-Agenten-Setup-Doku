# Projekt-Agenten-Setup Doku

Dieses Repository erklaert das Projekt-Agenten-Setup fuer freie C#-Projekt-Repositories in der Projekte-Organisation.

Der Projekt-Agent ist eine Lern- und Qualitaetshilfe. Er hilft bei Pull-Request-Reviews, Projektstruktur, CI, Tests, Dokumentation und optional bei kleinen Kevin-Implementierungs-PRs. Er ersetzt keine eigene Planung, Implementierung, Tests oder fachliche Verantwortung.

## Schnellstart

Wenn ihr direkt loslegen wollt:

1. Lest den [Quickstart](docs/quickstart.md).
2. Erstellt im Projekt-Repository ein Issue `Projekt-Agent aktivieren`.
3. Kommentiert im Issue:

```text
/init
```

4. Prueft und merged die Setup-PR.
5. Arbeitet danach mit Issues, Branches, Pull Requests und `/review`.

## Wichtigste Links

- [Quickstart fuer Studierende](docs/quickstart.md)
- [Ausfuehrliches Handbuch](docs/handbuch.md)
- [Workflow-Cookbook](docs/workflows.md)
- [Konfigurationsreferenz](docs/konfiguration.md)
- [FAQ und Troubleshooting](docs/faq.md)
- [LiaScript-Praesentation](praesentation/projekt-agent-einfuehrung.md)

## LiaScript-Praesentation

[![LiaScript](https://raw.githubusercontent.com/LiaScript/LiaScript/master/badges/course.svg)](https://liascript.github.io/course/?https://raw.githubusercontent.com/ifi-softwareentwicklung-2026-projekte/Projekt-Agenten-Setup-Doku/main/praesentation/projekt-agent-einfuehrung.md#1)

Die Praesentation ist fuer etwa 15 Minuten geplant und gibt Studierenden direkt umsetzbare erste Schritte an die Hand.

## Vorlagen

- [`vorlagen/classroom-agent.project.json`](vorlagen/classroom-agent.project.json)
- [`vorlagen/agent-config.review-only.json`](vorlagen/agent-config.review-only.json)
- [`vorlagen/agent-config.kevin-opt-in.json`](vorlagen/agent-config.kevin-opt-in.json)
- [`vorlagen/dotnet-build.yml`](vorlagen/dotnet-build.yml)

## Was gehoert in euer Projekt-Repository?

Minimal empfohlen:

```text
.
|-- .github/
|   `-- classroom-agent.json
|-- src/
|-- tests/
|-- docs/
|   `-- reflexionsbericht.md
|-- README.md
`-- agent-config.json
```

Der Agent funktioniert auch mit anderen Strukturen. Je klarer README, Build-Befehle, Tests und Projektdateien dokumentiert sind, desto hilfreicher sind Reviews.

## Sicherheitsregel

Schreibt keine Secrets, Tokens, Passwoerter, private Keys oder nicht notwendige personenbezogene Daten in Issues, Pull Requests, Kommentare oder Dateien. Agentenkommentare und technische Ereignisse koennen fuer Betreuung und Auswertung protokolliert werden.
