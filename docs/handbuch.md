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
- README und Reflexionsbericht

### Juergen

Juergen hilft bei Architekturfragen:

- Verantwortlichkeiten von Klassen
- Schnittstellen
- Abhaengigkeiten
- Aufteilung grosser Aenderungen

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

Startempfehlung: Nutzt die Review-only-Vorlage [`../vorlagen/agent-config.review-only.json`](../vorlagen/agent-config.review-only.json).

## 5. PR-Reviews

Lisa reviewed automatisch, wenn `features.autoPrReview=true` ist. Das ist der Default.

Lisa sammelt Kontext aus:

- geaenderten Dateien
- README und Projektdokumentation
- Projektdateien und Workflows
- CI-Status und CI-Logs, wenn vorhanden
- PR-Body und Issue-Link

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

Danach:

```text
/implement-small <konkreter kleiner Auftrag>
```

Kevin prueft Scope, sichere Pfade, Datei-/Diff-Grenzen und Dotnet-Preflight. Wenn eine Grenze verletzt wird, erstellt Kevin keine PR.

## 8. Reflexionsbericht

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

## 9. Datenschutz und Grenzen

Nicht posten:

- API-Keys
- Tokens
- Passwoerter
- private Keys
- nicht notwendige personenbezogene Daten

Agentenkommentare und technische Events koennen fuer Betreuung und Auswertung protokolliert werden.

## 10. Gute Praxis

- Kleine Issues statt grosser Sammelaufgaben.
- Kleine PRs statt Mega-PRs.
- `Closes #...` im PR-Body, wenn es ein Issue gibt.
- Build- und Testbefehle im README dokumentieren.
- Agentenfeedback pruefen, nicht blind uebernehmen.
- Kevin nur fuer kleine, klar begrenzte Hilfsarbeiten nutzen.
