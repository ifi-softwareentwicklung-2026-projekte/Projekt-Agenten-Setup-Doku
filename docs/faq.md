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
