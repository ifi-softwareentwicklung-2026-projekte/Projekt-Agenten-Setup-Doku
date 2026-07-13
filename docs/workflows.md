# Workflow-Cookbook

Dieses Cookbook beschreibt konkrete Situationen und die passenden Schritte.

## 1. Projekt-Agent aktivieren

1. Issue `Projekt-Agent aktivieren` erstellen.
2. Kommentar schreiben:

```text
/init
```

3. Setup-PR pruefen.
4. Setup-PR mergen.

Mit CI-Workflow in derselben Setup-PR:

```text
/init --with-dotnet-build
```

Ergebnis:

- `.github/classroom-agent.json` aktiviert den Projektworkflow.
- `agent-config.json` enthaelt sichere Defaults.
- Bei `/init --with-dotnet-build` wird zusaetzlich `.github/workflows/dotnet-build.yml` vorgeschlagen.
- Agent-Labels sind angelegt.

## 2. Feature sauber umsetzen

Issue-Beispiel:

```markdown
# SaveGameDto vorbereiten

Akzeptanzkriterien:
- Klasse `SaveGameDto` liegt unter `src/Game/Storage/`.
- Properties: `PlayerName`, `Score`, `CreatedAt`.
- Keine Datei-I/O-Logik.
- Kleiner Serialisierungs-Test oder begruendete Entscheidung dagegen.
```

Branch:

```bash
git switch -c feat/save-game-dto
```

PR-Body:

```markdown
## Was wurde geaendert?
- `SaveGameDto` ergaenzt.

## Tests
- [x] `dotnet build ...`
- [x] `dotnet test ...`

Closes #12
```

## 3. Review anfordern

Auf der PR:

```text
/review
```

Oder Label setzen:

```text
agent:review
```

Erwartung:

- Lisa erstellt einen Statuskommentar.
- Lisa sammelt Kontext.
- Lisa postet ein Review oder einen Fallback-Kommentar.

## 4. Aufgabe in kleinere Issues zerlegen

Auf einem Issue:

```text
/breakdown
```

Danach:

- 3 bis 5 kleine Teilziele formulieren.
- Pro Teilziel ein Issue anlegen.
- Akzeptanzkriterien notieren.

## 5. Setup und CI klaeren

Auf Issue oder PR:

```text
/setup
```

README sollte danach mindestens enthalten:

- Zweck des Projekts
- Build-Befehl
- Test-Befehl
- Start-Befehl
- bekannte Grenzen

## 6. Architekturfrage stellen

```text
/architecture Wir sind unsicher, ob ScoreCalculator und GameSession getrennte Klassen sein sollten. Betroffen sind src/Game/GameSession.cs und src/Game/ScoreCalculator.cs.
```

Je konkreter der Kontext, desto hilfreicher die Antwort.

## 7. Tests planen

```text
/tests
```

Guter Kontext:

- Welche Klasse oder Funktion ist betroffen?
- Welche Randfaelle sind unklar?
- Gibt es bereits ein Testprojekt?

## 8. Doku verbessern

```text
/docs Plane HTML und PDF fuer Pull Requests und manuellen Start. Kein taeglicher Lauf, maximal 20 Minuten, Artefakte 7 Tage aufbewahren.
```

Sinnvolle Dateien:

- `README.md`
- `docs/reflexionsbericht.md`
- `docs/architektur.md`
- XML-Kommentare fuer oeffentliche APIs

Gebt Artefakte, Ausloeser, Zeitplan, Laufzeit, Aufbewahrung und optional den Ausgabeordner direkt hinter `/docs` an. Beispiel fuer einen schnellen, einmaligen HTML-only-Vorschlag:

```text
/docs Erzeuge nur HTML, laufe ausschliesslich manuell, maximal 8 Minuten, bewahre das Artefakt 3 Tage auf und verwende site/api.
```

Ohne Zusatz gelten HTML + PDF, PR + manueller Start, kein Zeitplan, 20 Minuten und 7 Tage. Maria zeigt die effektive Konfiguration und bei Abweichungen einen Patch fuer `/documentation/...` in `agent-config.json`.

Soll Kevin dafuer einen GitHub-Actions-Workflow aendern, braucht ihr zusaetzlich `policies.allowKevinWorkflowChanges=true`. Dieser Opt-in kommt zu `features.kevinSmallImplementations=true` und `policies.allowFunctionalCodeGeneration=true` hinzu.

Prueft einen vorgeschlagenen Dokumentationsworkflow vor dem Merge:

- [ ] nur konfigurierte Trigger; Cron-Zeit ist UTC
- [ ] `permissions: contents: read`
- [ ] Checkout mit `persist-credentials: false`
- [ ] `concurrency.cancel-in-progress: true`
- [ ] `timeout-minutes` nicht groesser als konfiguriert
- [ ] `retention-days` nicht groesser als konfiguriert
- [ ] `apt-get --no-install-recommends`, kein `texlive-full`
- [ ] LaTeX nur fuer PDF/LaTeX
- [ ] `USE_MDFILE_AS_MAINPAGE` zeigt auf die echte README
- [ ] `EXTRACT_ALL = NO`, `WARN_IF_UNDOCUMENTED = YES`, `WARN_AS_ERROR = YES`
- [ ] alle oeffentlichen C#-APIs sind inhaltlich dokumentiert

Wenn ein passender Check wie `build-docs` noch laeuft, wartet Lisa mit dem automatischen Review. Ein erfolgreicher `dotnet-build` ist kein Ersatz fuer den Dokumentationscheck.

## 9. Kevin fuer kleine Implementierung nutzen

Voraussetzung: Kevin-Opt-in in `agent-config.json`.

Gutes Issue:

```markdown
# Eingabenamen validieren

Akzeptanzkriterien:
- Leere Namen werden abgelehnt.
- Fehlermeldung ist fuer CLI-Nutzer verstaendlich.
- Keine neue Dependency.
- Maximal eine Produktivdatei und eine Testdatei.
```

Kommentar:

```text
/implement-small Bitte ergaenze nur die Validierung fuer leere Namen und einen kleinen Unit-Test. Keine weitere Funktionalitaet.
```

## 10. Ready-Check vor Demo oder Abgabe

```text
/ready-check
```

Nutzen vor:

- Zwischenpraesentation
- finalem Merge
- Projektabgabe
- Tutor-Review

Der Ready-Check ersetzt keine Bewertung, erinnert aber an Build, Tests, README, Reflexion und Reviewstatus.
