# Handbuch: Projekt-Agent fuer freie C#-Projekte

Dieses Handbuch erklaert das Projekt-Agenten-Setup vollstaendig aus Studierendensicht.

## 1. Grundidee

Freie Projektarbeiten sind weniger starr als klassische Uebungsaufgaben. Es gibt nicht immer eine feste Teilaufgabe oder ein vorbereitetes Assignment-Issue. Der Projekt-Agent begleitet deshalb eure normale GitHub-Projektarbeit:

- Issues fuer Aufgaben und Fragen
- Branches fuer Aenderungen
- Pull Requests fuer Reviews
- CI fuer Build-Feedback
- Agentenkommentare als Lern- und Qualitaetshilfe

Der Agent blockiert nicht jeden unperfekten Workflow. Fehlende Issue-Links oder grosse PRs werden als Risiko markiert, aber nicht automatisch verhindert.

## 2. Agentenrollen

### Lisa

Lisa ist die Review-Persona. Sie schaut auf Pull Requests und gibt Feedback zu:

- Bugs und Randfaellen
- Build- und CI-Problemen
- Teststrategie
- Architektur- und Wartbarkeitsrisiken
- README, Doku und Reflexionsbericht

### Maria

Maria hilft bei Orientierung und Setup:

- Projektstruktur
- Build-Reproduzierbarkeit
- Issue-Breakdown
- README, Reflexionsbericht und reproduzierbare Dokumentationsworkflows

### Juergen

Juergen hilft bei Architekturfragen:

- Verantwortlichkeiten von Klassen
- Schnittstellen
- Abhaengigkeiten
- Aufteilung grosser Aenderungen

Juergen trennt dabei beobachteten Repository-Stand und Empfehlungen. Neue Projekte, Typen oder Dependencies sind Vorschlaege und werden nicht als bereits vorhanden dargestellt.

### Kevin

Kevin kann Setup-PRs erstellen und nach Opt-in kleine Implementierungs-PRs vorbereiten. Kevin ist bewusst begrenzt und ersetzt keine eigene Projektarbeit.

## 3. Aktivierung

Der Projektworkflow wird ueber `.github/classroom-agent.json` aktiviert:

```json
{
  "agent_workflow": {
    "workflow": "project-agent-workflows",
    "exercise": "project"
  }
}
```

Einfacher ist meist `/init` auf einem Issue. Kevin erstellt dann eine Setup-PR. Erst nach dem Merge dieser PR ist der Projekt-Agent dauerhaft aktiv.

## 4. `agent-config.json`

`agent-config.json` ist optional. Ohne Datei nutzt der Agent sichere Defaults:

- automatische PR-Reviews aktiv
- Label-Trigger aktiv
- Kevin-Implementierungen deaktiviert
- keine direkten `main`-Aenderungen durch Kevin
- Dokumentation als HTML und PDF bei PRs sowie manuell
- kein Dokumentations-Zeitplan, maximal 20 Minuten Laufzeit und 7 Tage Aufbewahrung
- README als Startseite sowie vollstaendige oeffentliche API-Dokumentation

Startempfehlung: Nutzt die Review-only-Vorlage [`../vorlagen/agent-config.review-only.json`](../vorlagen/agent-config.review-only.json).

## 5. PR-Reviews

Lisa reviewed automatisch, wenn `features.autoPrReview=true` ist. Das ist der Default.

Lisa sammelt Kontext aus:

- geaenderten Dateien
- README und Projektdokumentation
- Projektdateien und Workflows
- CI-Status und CI-Logs, wenn vorhanden
- PR-Body und Issue-Link

Bei Dokumentationsaenderungen prueft Lisa zusaetzlich den konfigurierten Dokumentationsvertrag. Ein gruenes `dotnet-build` reicht nicht als Nachweis fuer Doxygen-Artefakte. Laeuft ein passender Dokumentationscheck noch, wartet das automatische Review bis zu dessen Abschluss.

Wenn ihr manuell ein Review wollt:

```text
/review
```

Oder per Label:

```text
agent:review
```

## 6. CI und `dotnet-build`

Empfohlen ist ein PR-only Workflow mit dem Check-Namen `dotnet-build`. Der Server ist darauf ausgelegt, automatische Reviews nach Abschluss dieses Checks zu starten.

Wenn ihr den Workflow direkt bei der Aktivierung anlegen lassen wollt, nutzt `/init --with-dotnet-build`. Die GitHub App hat in der Projekte-Organisation die dafuer noetigen Rechte. Plain `/init` bleibt das bewusst schlanke Basis-Setup ohne CI-Datei.

Ihr koennt eine andere CI verwenden. Dann ist sie fuer Menschen weiter nuetzlich, aber Agent-Gating passt nur, wenn der Server entsprechend konfiguriert ist.

Vorlage: [`../vorlagen/dotnet-build.yml`](../vorlagen/dotnet-build.yml)

## 7. Kevin sicher nutzen

Kevin ist fuer kleine Hilfsaufgaben gedacht. Gute Aufgaben:

- DTO vorbereiten
- kleinen Unit-Test ergaenzen
- README-Abschnitt ergaenzen
- Guard-Clause in klar benannter Klasse einfuegen
- XML-Kommentar fuer oeffentliche API ergaenzen

Schlechte Aufgaben:

- `Mach das Feature fertig`
- komplette Architektur umbauen
- neue Frameworks einfuehren
- mehrere Features gleichzeitig
- Kernlogik der Projektarbeit schreiben lassen

Kevin braucht doppelten Opt-in:

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

Fuer Aenderungen unter `.github/workflows/` kommt ein dritter, separater Schalter hinzu:

```json
{
  "policies": {
    "allowKevinWorkflowChanges": true
  }
}
```

Lasst diesen Schalter `false`, solange Kevin keinen konkreten Workflow-Auftrag erhalten soll.

Danach:

```text
/implement-small <konkreter kleiner Auftrag>
```

Kevin prueft Scope, sichere Pfade, Datei-/Diff-Grenzen und Dotnet-Preflight. Wenn eine Grenze verletzt wird, erstellt Kevin keine PR.

## 8. Reproduzierbare Projektdokumentation

`/docs` erstellt einen projektspezifischen Plan. Der Text direkt hinter dem Command darf fuer diese Antwort Artefakte, Ausloeser, Zeitplan, Ausgabeordner, Aufbewahrung und eine kuerzere Laufzeit festlegen:

```text
/docs Erzeuge nur HTML, laufe ausschliesslich manuell, maximal 8 Minuten, und bewahre das Artefakt 3 Tage auf.
```

Ohne Zusatz gelten:

- Generator: Doxygen
- Artefakte: HTML und PDF
- Pfade: `docs/api/html/index.html` und `docs/api/latex/refman.pdf`
- Startseite: `README.md`
- Ausloeser: Pull Requests und `workflow_dispatch`
- kein Push- oder Cron-Lauf
- Laufzeitgrenze: 20 Minuten
- Aufbewahrung: 7 Tage
- vollstaendige Dokumentation oeffentlicher APIs
- Generatorwarnungen als Fehler

Eine einmalige `/docs`-Anweisung aendert das Repository nicht. Wenn der Wunsch von `agent-config.json` abweicht, zeigt Maria den kleinsten Patch unter dem echten Root-Schluessel `documentation`. Fuer eine dauerhafte Konfiguration uebernehmt und reviewed ihr diesen Patch selbst.

Ein sicherer Workflow enthaelt mindestens:

- `permissions: contents: read`
- `actions/checkout` mit `persist-credentials: false`
- `concurrency` mit `cancel-in-progress: true`
- `timeout-minutes` nicht groesser als konfiguriert
- `retention-days` nicht groesser als konfiguriert
- `apt-get --no-install-recommends`; niemals pauschal `texlive-full`
- LaTeX-Pakete nur fuer PDF oder LaTeX

Bei Doxygen gilt fuer den Vollstaendigkeitscheck:

```ini
USE_MDFILE_AS_MAINPAGE = README.md
EXTRACT_ALL = NO
WARN_IF_UNDOCUMENTED = YES
WARN_AS_ERROR = YES
```

Alle oeffentlichen C#-Typen und Member brauchen inhaltliche XML-Kommentare. Eine automatisch sichtbare Signaturliste ist keine vollstaendige API-Dokumentation.

## 9. Reflexionsbericht

Empfohlene Datei:

```text
docs/reflexionsbericht.md
```

Moegliche Struktur:

```markdown
# Reflexionsbericht

## Agentennutzung
- Datum, Agent, Kontext, Ergebnis

## Uebernommene Vorschlaege
- ...

## Abgelehnte Vorschlaege
- ...

## Eigene Entscheidungen
- ...

## Tests und Qualitaetssicherung
- ...
```

## 10. Datenschutz und Grenzen

Nicht posten:

- API-Keys
- Tokens
- Passwoerter
- private Keys
- nicht notwendige personenbezogene Daten

Agentenkommentare und technische Events koennen fuer Betreuung und Auswertung protokolliert werden.

## 11. Gute Praxis

- Kleine Issues statt grosser Sammelaufgaben.
- Kleine PRs statt Mega-PRs.
- `Closes #...` im PR-Body, wenn es ein Issue gibt.
- Build- und Testbefehle im README dokumentieren.
- Agentenfeedback pruefen, nicht blind uebernehmen.
- Kevin nur fuer kleine, klar begrenzte Hilfsarbeiten nutzen.
- Dokumentationsumfang und Workflow frueh festlegen; generierte Ausgaben als CI-Artefakte statt Git-Inhalt behandeln.
