# Quickstart: Projekt-Agent in 10 Minuten nutzen

Dieser Quickstart ist fuer Studierende gedacht, die ein freies C#-Projekt in der Projekte-Organisation bearbeiten.

## 1. Was macht der Projekt-Agent?

- Lisa reviewed Pull Requests und gibt Feedback zu Code, Tests, Architektur, Doku und CI.
- Maria hilft bei Setup, Projektstruktur, Breakdown und Dokumentation.
- Juergen gibt Architekturhinweise.
- Kevin kann kleine, klar begrenzte Aenderungen als Pull Request vorbereiten, aber nur nach bewusstem Opt-in.

Der Agent bewertet nicht eure Pruefungsleistung. Er liefert Feedback und hilft euch, eure Projektarbeit nachvollziehbarer und robuster zu organisieren.

## 2. Projekt aktivieren

Wenn euer Repository noch keine Agent-Dateien enthaelt:

1. Erstellt ein Issue mit dem Titel `Projekt-Agent aktivieren`.
2. Kommentiert im Issue:

```text
/init
```

3. Kevin erstellt eine Setup-PR.
4. Prueft die Dateien in der PR.
5. Merged die PR nach `main`.

Die Setup-PR enthaelt normalerweise:

- `.github/classroom-agent.json`
- `agent-config.json`

Voraussetzung: `/init` in einem noch unkonfigurierten Repository funktioniert nur, wenn die GitHub App installiert ist und die Repository-Organisation fuer Projekt-Bootstrap freigegeben ist. Wenn `/init` nicht reagiert, fragt Tutor/Admin oder legt `.github/classroom-agent.json` manuell an.

## 3. Optional: CI anlegen lassen

Standardmaessig legt `/init` keine GitHub-Actions-Datei an. Wenn ihr den vorgeschlagenen Dotnet-Build-Workflow wollt:

```text
/init --with-dotnet-build
```

Der Workflow heisst `dotnet-build` und laeuft bei Pull Requests nach `main`. Falls GitHub der App das Schreiben von Workflow-Dateien nicht erlaubt, kommentiert Kevin das und ihr koennt die Vorlage manuell kopieren: [`../vorlagen/dotnet-build.yml`](../vorlagen/dotnet-build.yml).

## 4. Normaler Arbeitsablauf

1. Issue fuer eine konkrete Aufgabe erstellen.
2. Branch anlegen, zum Beispiel `feat/save-games`.
3. Kleine, reviewbare Aenderung implementieren.
4. Lokal bauen und testen.
5. Pull Request nach `main` oeffnen.
6. Issue im PR-Body verlinken, zum Beispiel `Closes #12`.
7. CI und Lisa-Review abwarten.
8. Feedback einarbeiten.
9. Erst mergen, wenn ihr fachlich zufrieden seid.

Typische lokale Befehle mit explizitem Projekt- oder Solution-Pfad:

```bash
dotnet restore <pfad-zur-sln-oder-csproj>
dotnet build <pfad-zur-sln-oder-csproj> --configuration Release --no-restore
dotnet test <pfad-zum-test-csproj> --configuration Release --no-build
```

## 5. Wichtige Commands

Auf PRs:

```text
/review
```

Startet ein Lisa-Review.

```text
/ready-check
```

Gibt eine kurze Checkliste fuer Projekt- und Abgabebereitschaft.

Auf Issues:

```text
/breakdown
```

Hilft, grosse Aufgaben in kleinere Issues zu zerlegen.

```text
/setup
```

Hilft bei Repository-Struktur, Build und CI.

```text
/architecture
```

Gibt Architekturhinweise.

```text
/tests
```

Gibt Testideen.

```text
/docs
```

Gibt Hinweise zu README, XML-Kommentaren und Reflexionsbericht.

## 6. Kevin nur bewusst aktivieren

Kevin darf standardmaessig keinen funktionalen Code erzeugen. Fuer kleine Hilfsarbeiten braucht ihr in `agent-config.json` beide Schalter:

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

Dann koennt ihr auf einem Issue schreiben:

```text
/implement-small Bitte ergaenze nur eine Guard-Clause in PlayerNameValidator und einen kleinen Unit-Test. Keine neue Dependency.
```

Kevin erstellt nur Pull Requests, nie direkte `main`-Commits. Ihr muesst Kevins PR selbst fachlich pruefen.

## 7. Reflexionsbericht

Legt frueh `docs/reflexionsbericht.md` an und notiert kurz:

- Welche Agenten ihr genutzt habt.
- Welche Vorschlaege ihr uebernommen habt.
- Welche Vorschlaege ihr abgelehnt habt.
- Welche Tests und Checks ihr selbst ausgefuehrt habt.

## 8. Wenn etwas nicht funktioniert

Schaut zuerst in die [FAQ](faq.md). Die haeufigsten Ursachen sind:

- Setup-PR wurde noch nicht gemerged.
- `.github/classroom-agent.json` fehlt.
- `agent-config.json` hat ungueltiges JSON.
- GitHub App hat keine Workflow-Write-Permission.
- Kevin-Opt-in fehlt.
