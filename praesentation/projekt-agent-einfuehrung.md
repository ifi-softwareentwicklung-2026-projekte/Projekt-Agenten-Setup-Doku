<!--

author:   Softwareentwicklung TUBAF
email:    sebastian.zug@informatik.tu-freiberg.de
version:  1.1.0
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
| **Thema**          | Agenten-Unterstuetzung fuer freie C#-Projekte |
| **Zielgruppe**     | Studierende in Projekt-Repositories |
| **Dauer**          | ca. 10 bis 12 Minuten, maximal 15 Minuten mit kurzen Rueckfragen |
| **Ziel**           | Agent aktivieren, sinnvoll um Hilfe bitten und Feedback verantwortlich nutzen |
| **Doku**           | https://github.com/ifi-softwareentwicklung-2026-projekte/Projekt-Agenten-Setup-Doku |

> Der Agent begleitet euren normalen GitHub-Workflow. Er ersetzt weder eure fachlichen Entscheidungen noch eure Verantwortung fuer Code und Tests.

---------------------------------------------------------------------

## 1. Ein Agenten-Team begleitet euren Projektalltag

Der Projekt-Agent hilft direkt in Issues und Pull Requests:

- Ideen in kleine Aufgaben zerlegen
- Setup, Architektur, Tests und Dokumentation klaeren
- Pull Requests mit Repository- und CI-Kontext reviewen
- optional kleine, klar begrenzte Aenderungen als PR vorbereiten

Dabei bleibt der normale Entwicklungsprozess erhalten:

```text
Issue -> Branch -> Pull Request -> CI + Review -> Merge
```

> Der Agent liefert Orientierung und ein zweites Paar Augen. Entscheiden und verantworten muesst ihr selbst.

---------------------------------------------------------------------

## 2. Vier Rollen, ein gemeinsamer Repository-Kontext

| Agent | Schwerpunkt | Typische Commands |
| ----- | ----------- | ----------------- |
| **Maria** | Planung, Setup und Dokumentation | `/breakdown`, `/setup`, `/docs` |
| **Juergen** | Architektur und Abhaengigkeiten | `/architecture` |
| **Lisa** | Reviews, Tests und Readiness | `/review`, `/tests`, `/ready-check` |
| **Kevin** | Setup, Gerueste und kleine Opt-in-PRs | `/init`, `/scaffold`, `/implement-small` |

Die Antworten beziehen Issue-Text, Command-Zusatz und passende Repository-Dateien ein. Je konkreter eure Frage, desto hilfreicher das Ergebnis.

---------------------------------------------------------------------

## 3. Aktivierung: Ein Issue und `/init`

1. Erstellt ein Issue `Projekt-Agent aktivieren`.
2. Kommentiert:

```text
/init
```

3. Kevin erstellt eine Setup-PR mit:

```text
.github/classroom-agent.json
agent-config.json
```

4. Prueft und merged die PR nach `main`.

Optional kann direkt der empfohlene .NET-Build-Workflow mitkommen:

```text
/init --with-dotnet-build
```

> Wenn `/init` nicht reagiert, prueft die App-Installation oder fragt Tutor/Admin.

---------------------------------------------------------------------

## 4. Gute Agentenarbeit beginnt mit einem kleinen Issue

Schlecht:

```text
Mach die Benutzerverwaltung fertig.
```

Besser:

```markdown
# Leere Anzeigenamen ablehnen

Akzeptanzkriterien:
- PlayerNameValidator lehnt leere Namen ab.
- Die Fehlermeldung ist fuer CLI-Nutzer verstaendlich.
- Ein Unit-Test deckt den Fehlerfall ab.
- Keine neue Dependency.
```

Ein kleines Issue macht Planung, Implementierung, Tests und Review fuer Menschen und Agenten nachvollziehbar.

---------------------------------------------------------------------

## 5. Beratung zuerst, Code nur wenn wirklich noetig

Auf Issues oder PRs koennt ihr gezielt fragen:

```text
/breakdown    grosse Idee in naechste Issues zerlegen
/setup        Build, Struktur und CI klaeren
/architecture Verantwortlichkeiten und Schnittstellen pruefen
/tests        konkrete Testfaelle und Testebenen planen
/docs         README, API-Doku und Doku-Artefakte planen
/scaffold     kleinstes Geruest ohne Kernlogik vorschlagen
/ready-check  belegte Staerken, Luecken und Blocker sammeln
```

Zusatztext gehoert direkt hinter den Command:

```text
/docs Plane HTML und PDF fuer Pull Requests und manuellen Start. Kein taeglicher Lauf.
```

Diese Beratung erzeugt keine komplette Projektloesung.

---------------------------------------------------------------------

## 6. Lisa verbindet Anforderungen, Diff und passende Checks

Lisa reviewed Pull Requests automatisch oder auf Wunsch mit:

```text
/review
```

Sie beruecksichtigt unter anderem:

- geaenderte und geloeschte Dateien
- PR-Beschreibung und verknuepfte Issue-Akzeptanzkriterien
- Code, Tests, README, Konfiguration und Workflows
- vorhandene relevante CI-Checks und Fehlermeldungen

Verlinkt das Issue im PR-Body:

```text
Closes #12
```

> Ein gruener Check ist Evidenz, aber kein Ersatz fuer euer fachliches Urteil.

---------------------------------------------------------------------

## 7. Kevin arbeitet nur innerhalb enger Grenzen

Funktionale Aenderungen sind standardmaessig deaktiviert. Fuer kleine Implementierungs-PRs braucht ihr den bewussten doppelten Opt-in:

```json
{
  "features": { "kevinSmallImplementations": true },
  "policies": { "allowFunctionalCodeGeneration": true }
}
```

Danach zum Beispiel:

```text
/implement-small Ergaenze nur die Validierung aus diesem Issue und den beschriebenen Unit-Test. Keine neue Dependency.
```

Kevin prueft vor dem Push Scope, erlaubte Pfade, Datei-/Diff-Grenzen sowie `dotnet build` und `dotnet test`. Bei einem Fehler entsteht keine PR.

Kevin pusht nie direkt auf `main`.

---------------------------------------------------------------------

## 8. Dokumentation ist ein Workflow, nicht nur eine Datei

Mit `/docs` koennt ihr beschreiben, was ihr braucht, zum Beispiel:

- README und Reflexionsbericht
- XML-Kommentare fuer oeffentliche APIs
- HTML-, PDF-, XML- oder LaTeX-Artefakte
- Ausfuehrung bei PRs, manuell oder nach einem gewuenschten Zeitplan

Dokumentations-Workflows sollen begrenzt, reproduzierbar und mit minimalen Rechten laufen. Kevin darf Dateien unter `.github/workflows/` nur mit einem zusaetzlichen Policy-Opt-in aendern.

> Die konkrete Konfiguration und Beispiele stehen im Doku-Repo. Fuer den Vortrag reicht: Sagt hinter `/docs`, welches Ergebnis und welchen Ausloeser ihr braucht.

---------------------------------------------------------------------

## 9. Eure Verantwortung bleibt sichtbar

Vor dem Merge:

- [ ] Issue und PR beschreiben dasselbe Ziel.
- [ ] Build und Tests wurden wirklich ausgefuehrt.
- [ ] Lisa-Feedback wurde geprueft, nicht blind uebernommen.
- [ ] Kevin-Aenderungen wurden verstanden.
- [ ] README oder Reflexionsbericht sind bei Bedarf aktualisiert.
- [ ] Keine Secrets oder unnoetigen personenbezogenen Daten sind enthalten.

Legt frueh an:

```text
docs/reflexionsbericht.md
```

Notiert dort kurz, was ihr uebernommen, abgelehnt und selbst geprueft habt.

---------------------------------------------------------------------

## 10. Heute starten, Details spaeter nachlesen

1. Doku-Repo und Quickstart oeffnen.
2. Aktivierungs-Issue erstellen.
3. `/init` kommentieren und Setup-PR pruefen.
4. Erstes kleines Arbeits-Issue formulieren.
5. PR mit CI und Lisa-Review abschliessen.

| Wenn ihr ... | dann lest ... |
| ------------ | ------------- |
| schnell starten wollt | `docs/quickstart.md` |
| einen konkreten Ablauf sucht | `docs/workflows.md` |
| Policies und Opt-ins braucht | `docs/konfiguration.md` |
| einen Fehler sucht | `docs/faq.md` |

Repo:

https://github.com/ifi-softwareentwicklung-2026-projekte/Projekt-Agenten-Setup-Doku

> Ziel ist nicht, den Agenten moeglichst viel machen zu lassen. Ziel ist, mit seiner Hilfe bewusster und sauberer zu arbeiten.
