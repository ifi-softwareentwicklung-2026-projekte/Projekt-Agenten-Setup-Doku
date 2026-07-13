# FAQ und Troubleshooting

## Der Agent reagiert nicht

Prueft:

- Ist die Setup-PR von `/init` gemerged?
- Existiert `.github/classroom-agent.json` auf `main`?
- Ist die GitHub App im Repository installiert?
- Ist das Repository in einer erlaubten Organisation?

Minimaldatei:

```json
{
  "agent_workflow": {
    "workflow": "project-agent-workflows",
    "exercise": "project"
  }
}
```

## `/init` erstellt keine PR

Moegliche Ursachen:

- Repository ist bereits vorbereitet.
- Es gibt keine Aenderung mehr fuer Kevin.
- Ihr habt `/init` ohne `--with-dotnet-build` genutzt und erwartet trotzdem eine CI-Workflow-Datei.

Plain `/init` erstellt nur das Basis-Setup. Wenn ihr den CI-Workflow direkt in der Setup-PR wollt, nutzt `/init --with-dotnet-build`.

## `/init --with-dotnet-build` fuegt keine Workflow-Datei hinzu

Moegliche Ursachen:

- Workflow existiert bereits.
- Es wurde plain `/init` statt `/init --with-dotnet-build` genutzt.
- Die Datei wurde spaeter manuell entfernt oder umbenannt.

Loesung fuer Studierende:

- `/init --with-dotnet-build` nutzen, wenn das Repository noch nicht fertig eingerichtet ist.
- Vorlage [`../vorlagen/dotnet-build.yml`](../vorlagen/dotnet-build.yml) manuell nach `.github/workflows/dotnet-build.yml` kopieren, wenn ihr CI spaeter nachruesten wollt.
- Tutor/Admin informieren, falls trotz Projekte-Org-Rechten weiterhin ein Permission-Fehler kommentiert wird.

## Lisa reviewed nicht automatisch

Moegliche Ursachen:

- `features.autoPrReview=false`
- PR ist geschlossen oder gemerged
- gleicher Commit wurde bereits reviewed
- CI laeuft noch
- LLM-Review war nicht verfuegbar

Manuell starten:

```text
/review
```

## PR ohne Issue-Link wird markiert

Das blockiert nicht. Besser ist:

```markdown
Closes #12
```

Wenn es kein Issue gab, schreibt kurz warum.

## PR wird als Large PR markiert

Das ist ein Hinweis auf Review-Risiko. Besser:

- Formatierung getrennt von Logik.
- Doku getrennt von Feature-Code.
- Architekturumbau getrennt von Tests.
- Ein Issue pro sichtbarem Ziel.

## Kevin sagt, kleine Implementierungen sind nicht aktiviert

Das ist der Default. Aktiviert Kevin nur bewusst:

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

## Kevin erstellt trotz Opt-in keine PR

Moegliche Ursachen:

- Auftrag ist zu unkonkret.
- Aenderung waere zu gross.
- Pfad ist unsicher.
- Dotnet-Preflight ist fehlgeschlagen.
- LLM-Antwort war nicht belastbar.

Guter Auftrag:

```text
/implement-small Bitte ergaenze in PlayerNameValidator nur die Pruefung auf leere Namen und einen kleinen Unit-Test. Keine neue Dependency.
```

## `/docs` verwendet nicht meine gewuenschten Artefakte oder Ausloeser

Schreibt die Abweichung direkt hinter den Command, zum Beispiel:

```text
/docs Erzeuge nur HTML, laufe ausschliesslich manuell, maximal 8 Minuten, und bewahre das Artefakt 3 Tage auf.
```

Diese Angabe gilt nur fuer die aktuelle Antwort. Maria nennt zuerst die effektive Konfiguration und zeigt bei einer Abweichung einen kleinen JSON-Patch. Dauerhafte Werte gehoeren unter den Root-Schluessel `documentation` in `agent-config.json` (nicht unter `documentation_config`).

## Warum plant `/docs` standardmaessig HTML und PDF?

HTML ist schnell durchsuchbar, PDF eignet sich fuer Abgabe und Archivierung. Die sicheren Defaults sind:

- HTML und PDF
- Pull Requests und manueller Start
- kein Push- oder Cron-Lauf
- maximal 20 Minuten
- 7 Tage Artefaktaufbewahrung
- README als Startseite
- vollstaendige oeffentliche API-Dokumentation
- Fehler bei Doxygen-Warnungen

Wenn PDF zu langsam oder nicht erforderlich ist, koennt ihr einmalig oder dauerhaft auf HTML-only umstellen. LaTeX darf bei HTML-only nicht installiert werden.

## Der Dokumentationsworkflow ist gruen, aber die Doku ist leer

Ein vorhandenes Artefakt allein beweist keine inhaltliche Vollstaendigkeit. Prueft insbesondere:

- `USE_MDFILE_AS_MAINPAGE = README.md`
- `EXTRACT_ALL = NO`
- `WARN_IF_UNDOCUMENTED = YES`
- `WARN_AS_ERROR = YES` oder `FAIL_ON_WARNINGS`
- inhaltliche XML-Kommentare fuer alle oeffentlichen Typen und Member

`EXTRACT_ALL = YES` macht zwar Signaturen sichtbar, deaktiviert aber Warnungen zu undokumentierten Membern. Lisa beanstandet diese Konfiguration, wenn `documentPublicApi=true` ist.

## Kevin darf `Doxyfile`, aber keinen GitHub-Actions-Workflow aendern

Workflow-Dateien sind ausfuehrbarer CI-Code und benoetigen einen separaten Opt-in:

```json
{
  "policies": {
    "allowKevinWorkflowChanges": true
  }
}
```

Zusaetzlich muessen Kevins funktionale Small-Implementation-Schalter aktiv sein. Falls GitHub der App trotzdem keine Workflows-Schreibrechte gibt, laesst die Middleware nur die Workflow-Datei aus und nennt die Auslassung transparent.

## Lisa reviewed eine Doku-PR noch nicht

Wenn `Doxyfile` oder ein Dokumentationsworkflow geaendert wurde und ein passender Check wie `build-docs` noch laeuft, wartet Lisa gezielt auf dessen Abschluss. Ein erfolgreicher `dotnet-build` ersetzt keinen Doxygen-/Dokumentationsnachweis. Nach Abschluss kann `/review` einen erneuten manuellen Lauf starten.

## `agent-config.json` ist kaputt

Pruefen:

```bash
python3 -m json.tool agent-config.json >/dev/null
```

Haeufige Fehler:

- fehlendes Komma
- Kommentare in JSON
- einfache statt doppelte Anfuehrungszeichen
- trailing comma nach letztem Feld
- Root-Schluessel `documentation_config` statt `documentation`
- absolute oder traversierende Pfade wie `/tmp/docs` oder `../docs`
- ungueltiger Cron-Ausdruck; GitHub-Actions-Zeitplaene verwenden UTC

## GitHub Review erscheint nur als Kommentar

Das kann passieren, wenn GitHub Apps kein formales Review erlauben. Lisa faellt dann auf einen normalen Kommentar zurueck. Das ist erwartetes Verhalten.

## Was darf nicht gepostet werden?

Nicht posten:

- API-Keys
- Tokens
- Passwoerter
- private Keys
- nicht notwendige personenbezogene Daten

Wenn versehentlich ein Secret gepostet wurde: sofort rotieren und Tutor/Admin informieren.
