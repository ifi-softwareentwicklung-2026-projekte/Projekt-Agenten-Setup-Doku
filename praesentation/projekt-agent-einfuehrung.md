<!--

author:   Softwareentwicklung TUBAF
email:    sebastian.zug@informatik.tu-freiberg.de
version:  1.0.0
language: de
narrator: Deutsch Female
comment:  Kurze Einfuehrung in den Projekt-Agenten fuer freie C#-Projekt-Repositories

import: https://raw.githubusercontent.com/liascript-templates/plantUML/master/README.md
        https://github.com/liascript/CodeRunner

-->

[![LiaScript](https://raw.githubusercontent.com/LiaScript/LiaScript/master/badges/course.svg)](https://liascript.github.io/course/?https://raw.githubusercontent.com/ifi-softwareentwicklung-2026-projekte/Projekt-Agenten-Setup-Doku/main/praesentation/projekt-agent-einfuehrung.md#1)

# Projekt-Agenten-Setup

| Parameter          | Kursinformationen |
| ------------------ | ----------------- |
| **Thema**          | Projekt-Agent fuer freie C#-Projekte |
| **Zielgruppe**     | Studierende in Projekt-Repositories |
| **Dauer**          | ca. 15 Minuten |
| **Ziel**           | Erste eigene Schritte mit `/init`, Issues, PRs, Reviews und Kevin-Opt-in |
| **Doku**           | https://github.com/ifi-softwareentwicklung-2026-projekte/Projekt-Agenten-Setup-Doku |

> Nach dieser kurzen Einfuehrung koennt ihr euer Projekt-Repository aktivieren, einen sinnvollen PR-Workflow nutzen und gezielt Hilfe vom Agenten anfordern.

---------------------------------------------------------------------

## 1. Was ist der Projekt-Agent?

Der Projekt-Agent ist ein Agenten-Team in GitHub.

Er hilft euch bei:

- Pull-Request-Reviews
- Projektstruktur und Setup
- Architekturfragen
- Tests und CI
- Dokumentation und Reflexion
- optional kleinen Kevin-Implementierungs-PRs

Wichtig:

> Der Agent ist eine Lern- und Qualitaetshilfe. Er ersetzt nicht eure eigene Verantwortung fuer Code, Tests und fachliche Entscheidungen.

---------------------------------------------------------------------

## 2. Die vier Rollen

| Agent | Rolle | Wann nutzen? |
| ----- | ----- | ------------ |
| Lisa | Reviewerin | PR pruefen lassen |
| Maria | Setup und Struktur | Projekt ordnen, Issues schneiden, Doku verbessern |
| Juergen | Architektur | Verantwortlichkeiten und Schnittstellen klaeren |
| Kevin | kleine PRs | nur nach Opt-in und nur fuer klar begrenzte Aufgaben |

Merksatz:

> Lisa reviewed, Maria sortiert, Juergen hinterfragt, Kevin baut nur kleine Hilfen.

---------------------------------------------------------------------

## 3. Schritt 1: Projekt aktivieren

Wenn euer Projekt-Repository noch keinen Agenten hat:

1. Erstellt ein Issue mit dem Titel `Projekt-Agent aktivieren`.
2. Kommentiert im Issue:

```text
/init
```

3. Kevin erstellt eine Setup-PR.
4. Ihr prueft die PR.
5. Ihr merged die PR nach `main`.

Danach ist der Projekt-Agent dauerhaft aktiv.

> Wenn `/init` nicht reagiert, fragt Tutor/Admin. Die GitHub App muss installiert sein und Bootstrap muss fuer die Organisation freigegeben sein.

---------------------------------------------------------------------

## 4. Was macht die Setup-PR?

Die Setup-PR fuegt normalerweise zwei Dateien hinzu:

```text
.github/classroom-agent.json
agent-config.json
```

Minimaler Inhalt von `.github/classroom-agent.json`:

```json
{
  "agent_workflow": {
    "workflow": "project-agent-workflows",
    "exercise": "project"
  }
}
```

Die Datei sagt dem Server: Dieses Repository ist ein Projekt-Repository.

---------------------------------------------------------------------

## 5. Optional: CI direkt mit anlegen

Standardmaessig legt `/init` keine GitHub-Actions-Datei an.

Wenn ihr den vorgeschlagenen Dotnet-Build-Workflow wollt:

```text
/init --with-dotnet-build
```

Der Check heisst:

```text
dotnet-build
```

Warum ist das wichtig?

- Lisa kann auf CI warten.
- Rote Builds werden im Review sichtbar.
- Ihr habt direkt Feedback, ob euer PR baut.

Wenn die App keine Workflow-Dateien schreiben darf, koennt ihr die Vorlage aus `vorlagen/dotnet-build.yml` manuell kopieren.

---------------------------------------------------------------------

## 6. Euer normaler Projekt-Workflow

So soll ein kleiner Arbeitsschritt aussehen:

```text
Issue -> Branch -> Pull Request -> Review -> Merge
```

Konkret:

1. Issue fuer eine kleine Aufgabe erstellen.
2. Branch anlegen, z.B. `feat/save-game-dto`.
3. Kleine Aenderung implementieren.
4. Lokal bauen und testen.
5. PR nach `main` oeffnen.
6. Im PR-Body `Closes #12` eintragen.
7. Lisa-Review und CI abwarten.
8. Feedback einarbeiten.
9. Erst dann mergen.

---------------------------------------------------------------------

## 7. Beispiel fuer ein gutes Issue

```markdown
# SaveGameDto vorbereiten

Akzeptanzkriterien:
- Klasse `SaveGameDto` liegt unter `src/Game/Storage/`.
- Properties: `PlayerName`, `Score`, `CreatedAt`.
- Keine Datei-I/O-Logik.
- Ein kleiner Serialisierungs-Test ist vorhanden oder begruendet nicht vorhanden.
```

Warum ist das gut?

- klein genug fuer einen PR
- klare Akzeptanzkriterien
- kein komplettes Feature-Paket
- gut reviewbar

---------------------------------------------------------------------

## 8. Review anfordern

Auf einer Pull Request:

```text
/review
```

Lisa schaut dann auf:

- geaenderte Dateien
- README und Projektdoku
- Projektdateien und Workflows
- CI-Status und Logs
- PR-Body und Issue-Link

Wenn ein Issue-Link fehlt, reviewed Lisa trotzdem, markiert es aber als Workflow-Hinweis.

---------------------------------------------------------------------

## 9. Hilfe ohne Codegenerierung

Diese Commands koennt ihr jederzeit nutzen:

```text
/breakdown
/setup
/architecture
/tests
/docs
/ready-check
```

Beispiele:

- `/breakdown`: Aus grosser Idee kleine Issues machen.
- `/setup`: Build, README und CI klaeren.
- `/architecture`: Klassen und Verantwortlichkeiten diskutieren.
- `/tests`: Testideen sammeln.
- `/docs`: README und Reflexionsbericht verbessern.

Das erzeugt keine komplette Loesung, sondern hilft euch beim Arbeiten.

---------------------------------------------------------------------

## 10. Kevin: nur mit Opt-in

Kevin darf standardmaessig keinen funktionalen Code erzeugen.

Fuer kleine Implementierungs-PRs braucht ihr beide Schalter in `agent-config.json`:

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

Dann koennt ihr schreiben:

```text
/implement-small Bitte ergaenze nur die Validierung fuer leere Namen in PlayerNameValidator und einen kleinen Unit-Test. Keine neue Dependency.
```

Kevin erstellt nur Pull Requests, nie direkte `main`-Commits.

---------------------------------------------------------------------

## 11. Was Kevin nicht machen soll

Nicht geeignet:

- `Mach das Feature fertig`
- komplette Persistenz bauen
- Architektur umbauen
- neue Frameworks einfuehren
- mehrere Features gleichzeitig
- Code uebernehmen, den ihr nicht versteht

Geeignet:

- kleines DTO
- ein kleiner Test
- Guard-Clause in benannter Klasse
- README-Abschnitt
- XML-Kommentar

> Ihr bleibt verantwortlich fuer jeden Commit, den ihr merged.

---------------------------------------------------------------------

## 12. Reflexionsbericht und Datenschutz

Legt frueh an:

```text
docs/reflexionsbericht.md
```

Notiert kurz:

- Welche Agenten habt ihr genutzt?
- Was habt ihr uebernommen?
- Was habt ihr abgelehnt?
- Welche Tests habt ihr selbst ausgefuehrt?

Nicht posten:

- Tokens
- Passwoerter
- API-Keys
- private Keys
- unnoetige personenbezogene Daten

---------------------------------------------------------------------

## 13. Erste Schritte nach dieser Session

Macht heute noch diese 5 Schritte:

1. Doku-Repo oeffnen.
2. Quickstart lesen.
3. Im Projekt-Repo Issue `Projekt-Agent aktivieren` erstellen.
4. `/init` kommentieren.
5. Setup-PR pruefen und mergen.

Danach fuer euren ersten echten Arbeitsschritt:

1. kleines Issue schreiben
2. Branch erstellen
3. PR oeffnen
4. `/review` nutzen

---------------------------------------------------------------------

## 14. Wo finde ich Hilfe?

| Frage | Dokument |
| ----- | -------- |
| Schnell starten | `docs/quickstart.md` |
| Alles verstehen | `docs/handbuch.md` |
| Konkrete Ablaufe | `docs/workflows.md` |
| Config erklaeren | `docs/konfiguration.md` |
| Fehler beheben | `docs/faq.md` |

Repo:

https://github.com/ifi-softwareentwicklung-2026-projekte/Projekt-Agenten-Setup-Doku

---------------------------------------------------------------------

## 15. Mini-Checkliste

Vor jedem Merge kurz pruefen:

- [ ] PR ist klein genug.
- [ ] Issue ist verlinkt oder bewusst nicht vorhanden.
- [ ] Build/Test-Befehle wurden ausgefuehrt.
- [ ] Lisa-Feedback wurde gelesen.
- [ ] README oder Reflexionsbericht sind aktualisiert, wenn noetig.
- [ ] Kevin-Code wurde verstanden, falls Kevin beteiligt war.

> Ziel ist nicht, den Agenten moeglichst viel machen zu lassen. Ziel ist, mit seiner Hilfe bewusster und sauberer zu arbeiten.
