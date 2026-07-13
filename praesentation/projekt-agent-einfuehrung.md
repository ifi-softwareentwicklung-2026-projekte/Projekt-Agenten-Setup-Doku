<!--

author:   Softwareentwicklung TUBAF
email:    sebastian.zug@informatik.tu-freiberg.de
version:  1.2.0
language: de
narrator: Deutsch Female
comment:  Kurze Einführung in den Projekt-Agenten für freie C#-Projekt-Repositories

import: https://raw.githubusercontent.com/liascript-templates/plantUML/master/README.md
        https://github.com/liascript/CodeRunner

-->

[![LiaScript](https://raw.githubusercontent.com/LiaScript/LiaScript/master/badges/course.svg)](https://liascript.github.io/course/?https://raw.githubusercontent.com/ifi-softwareentwicklung-2026-projekte/Projekt-Agenten-Setup-Doku/main/praesentation/projekt-agent-einfuehrung.md#1)

# Projekt-Agenten-Setup

| Parameter          | Kursinformationen |
| ------------------ | ----------------- |
| **Thema**          | Agenten-Unterstützung für freie C#-Projekte |
| **Zielgruppe**     | Studierende in Projekt-Repositories |
| **Dauer**          | ca. 12 bis 14 Minuten, maximal 15 Minuten mit kurzen Rückfragen |
| **Ziel**           | Agent aktivieren, sinnvoll um Hilfe bitten und Feedback verantwortlich nutzen |
| **Doku**           | https://github.com/ifi-softwareentwicklung-2026-projekte/Projekt-Agenten-Setup-Doku |

> Der Agent begleitet euren normalen GitHub-Workflow. Er ersetzt weder eure fachlichen Entscheidungen noch eure Verantwortung für Code und Tests.

---------------------------------------------------------------------

## 1. Ein Agenten-Team begleitet euren Projektalltag

Der Projekt-Agent hilft direkt in Issues und Pull Requests:

- Ideen in kleine Aufgaben zerlegen
- Setup, Architektur, Tests und Dokumentation klären
- Pull Requests mit Repository- und CI-Kontext reviewen
- optional kleine, klar begrenzte Änderungen als PR vorbereiten

Dabei bleibt der normale Entwicklungsprozess erhalten:

```text
Issue -> Branch -> Pull Request -> CI + Review -> Merge
```

> Der Agent liefert Orientierung und ein zweites Paar Augen. Entscheiden und verantworten müsst ihr selbst.

---------------------------------------------------------------------

## 2. Vier Rollen teilen die Unterstützung auf

| Agent | Hauptaufgabe |
| ----- | ------------ |
| **Maria** | Projekt planen und organisieren |
| **Jürgen** | Architektur und Abhängigkeiten klären |
| **Lisa** | Qualität, Tests und Reviews prüfen |
| **Kevin** | Setup und kleine, klar begrenzte PRs vorbereiten |

Alle vier arbeiten mit Issue-Text, Command-Zusatz und passendem Repository-Kontext. Je konkreter eure Frage, desto hilfreicher das Ergebnis.

---------------------------------------------------------------------

## 3. Maria und Jürgen: Erst planen, dann bauen

**Maria** hilft dabei, aus einer Idee einen umsetzbaren nächsten Schritt zu machen:

```text
/breakdown   Projektidee in kleine Issues zerlegen
/setup       Struktur, Build und CI klären
/docs        README, Reflexion und Doku-Artefakte planen
```

**Jürgen** hilft bei technischen Entscheidungen vor der Implementierung:

```text
/architecture Verantwortlichkeiten, Schnittstellen und Abhängigkeiten prüfen
```

> Beide geben Orientierung. Sie nehmen euch keine fachlichen Entscheidungen ab.

---------------------------------------------------------------------

## 4. Lisa und Kevin: Qualität sichern und gezielt unterstützen

**Lisa** prüft Pull Requests, Tests und Abgabebereitschaft:

```text
/review
/tests
/ready-check
```

**Kevin** bereitet das Setup, kleine Gerüste und – nur nach Opt-in – sehr kleine PRs vor:

```text
/init
/scaffold
/implement-small
```

> Kevin erstellt keine Komplettlösung und pusht nie direkt auf `main`.

---------------------------------------------------------------------

## 5. `/init` aktiviert und konfiguriert das Setup im Repository

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

Diese Konfiguration bestimmt anschließend, wie euer Setup arbeitet, zum Beispiel:

- ob Lisa PRs automatisch reviewed,
- ob Labels Commands auslösen,
- ob Kevin Code erzeugen darf und wie groß seine Änderungen sein dürfen,
- ob Kevin überhaupt Workflow-Dateien ändern darf.

4. Prüft die Setup-PR und merged sie nach `main`.

Optional kann direkt der empfohlene .NET-Build-Workflow mitkommen:

```text
/init --with-dotnet-build
```

> Wenn `/init` nicht reagiert, prüft die App-Installation oder fragt Tutor/Admin.

---------------------------------------------------------------------

## 6. Danach folgt euer normaler Projektablauf

```text
Kleines Issue -> Branch -> Pull Request -> CI + Lisa-Review -> Merge
```

Ein Issue gibt Menschen und Agenten den nötigen Kontext. Es sollte ein kleines, überprüfbares Ziel beschreiben.

Schlecht:

```text
Mach die Benutzerverwaltung fertig.
```

Besser:

```markdown
# Leere Anzeigenamen ablehnen

Akzeptanzkriterien:
- PlayerNameValidator lehnt leere Namen ab.
- Die Fehlermeldung ist für CLI-Nutzer verständlich.
- Ein Unit-Test deckt den Fehlerfall ab.
- Keine neue Dependency.
```

Ein kleines Issue macht Planung, Implementierung, Tests und Review für Menschen und Agenten nachvollziehbar.

---------------------------------------------------------------------

## 7. Beratung zuerst, Code nur wenn wirklich nötig

Auf Issues oder PRs könnt ihr gezielt fragen:

```text
/breakdown    große Idee in nächste Issues zerlegen
/setup        Build, Struktur und CI klären
/architecture Verantwortlichkeiten und Schnittstellen prüfen
/tests        konkrete Testfälle und Testebenen planen
/docs         README, API-Doku und Doku-Artefakte planen
/ready-check  belegte Stärken, Lücken und Blocker sammeln
```

Zusatztext gehört direkt hinter den Command:

```text
/docs Plane HTML und PDF für Pull Requests und manuellen Start. Kein täglicher Lauf.
```

Diese Beratung erzeugt keine komplette Projektlösung.

---------------------------------------------------------------------

## 8. Lisa verbindet Anforderungen, Diff und passende Checks

Lisa reviewed Pull Requests automatisch oder auf Wunsch mit:

```text
/review
```

Sie berücksichtigt unter anderem:

- geänderte und gelöschte Dateien
- PR-Beschreibung und verknüpfte Issue-Akzeptanzkriterien
- Code, Tests, README, Konfiguration und Workflows
- vorhandene relevante CI-Checks und Fehlermeldungen

Verlinkt das Issue im PR-Body:

```text
Closes #12
```

> Ein grüner Check ist Evidenz, aber kein Ersatz für euer fachliches Urteil.

---------------------------------------------------------------------

## 9. Kevin arbeitet nur innerhalb enger Grenzen

Funktionale Änderungen sind standardmäßig deaktiviert. Für kleine Implementierungs-PRs braucht ihr den bewussten doppelten Opt-in:

```json
{
  "features": { "kevinSmallImplementations": true },
  "policies": { "allowFunctionalCodeGeneration": true }
}
```

Danach zum Beispiel:

```text
/implement-small Ergänze nur die Validierung aus diesem Issue und den beschriebenen Unit-Test. Keine neue Dependency.
```

Kevin prüft vor dem Push Scope, erlaubte Pfade, Datei-/Diff-Grenzen sowie `dotnet build` und `dotnet test`. Bei einem Fehler entsteht keine PR.

Kevin pusht nie direkt auf `main`.

---------------------------------------------------------------------

## 10. Dokumentation ist ein Workflow, nicht nur eine Datei

Mit `/docs` könnt ihr beschreiben, was ihr braucht, zum Beispiel:

- README und Reflexionsbericht
- XML-Kommentare für öffentliche APIs
- HTML-, PDF-, XML- oder LaTeX-Artefakte
- Ausführung bei PRs, manuell oder nach einem gewünschten Zeitplan

Dokumentations-Workflows sollen begrenzt, reproduzierbar und mit minimalen Rechten laufen. Kevin darf Dateien unter `.github/workflows/` nur mit einem zusätzlichen Policy-Opt-in ändern.

> Die konkrete Konfiguration und Beispiele stehen im Doku-Repo. Für den Vortrag reicht: Sagt hinter `/docs`, welches Ergebnis und welchen Auslöser ihr braucht.

---------------------------------------------------------------------

## 11. Heute starten und verantwortlich abschließen

1. Doku-Repo und Quickstart öffnen.
2. Aktivierungs-Issue erstellen.
3. `/init` kommentieren und Setup-PR prüfen.
4. Erstes kleines Arbeits-Issue formulieren.
5. PR mit CI und Lisa-Review abschließen.

Vor jedem Merge kurz prüfen:

- Issue und PR beschreiben dasselbe Ziel.
- Build und Tests wurden wirklich ausgeführt.
- Lisa-Feedback und Kevin-Änderungen wurden verstanden.
- README oder Reflexionsbericht sind bei Bedarf aktualisiert.
- Keine Secrets oder unnötigen personenbezogenen Daten sind enthalten.

| Wenn ihr ... | dann lest ... |
| ------------ | ------------- |
| schnell starten wollt | `docs/quickstart.md` |
| einen konkreten Ablauf sucht | `docs/workflows.md` |
| Policies und Opt-ins braucht | `docs/konfiguration.md` |
| einen Fehler sucht | `docs/faq.md` |

Repo:

https://github.com/ifi-softwareentwicklung-2026-projekte/Projekt-Agenten-Setup-Doku

> Ziel ist nicht, den Agenten möglichst viel machen zu lassen. Ziel ist, mit seiner Hilfe bewusster und sauberer zu arbeiten.

---------------------------------------------------------------------

## 12. Fragen, Probleme oder Feedback?

Meldet euch sehr gerne bei mir, wenn ihr:

- Fragen zum Projekt-Agenten-Setup habt,
- auf ein Problem stoßt,
- Anmerkungen oder Verbesserungsvorschläge habt oder
- Feedback aus der praktischen Nutzung geben möchtet.

**Simon Hörtzsch**

[Simon.Hoertzsch@student.tu-freiberg.de](mailto:Simon.Hoertzsch@student.tu-freiberg.de)

> Eure Rückmeldungen helfen dabei, das Setup für alle Studierenden verständlicher und zuverlässiger zu machen.
