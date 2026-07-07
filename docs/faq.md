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
- Nur die Workflow-Datei wurde angefragt, aber GitHub erlaubt der App kein Schreiben von Workflow-Dateien.

Wenn keine Workflow-Datei geschrieben werden darf, funktioniert der Projekt-Agent trotzdem. Legt CI bei Bedarf manuell an.

## `/init --with-dotnet-build` fuegt keine Workflow-Datei hinzu

Moegliche Ursachen:

- Workflow existiert bereits.
- GitHub App hat keine `Workflows: Read and write` Permission.
- Die geaenderte App-Permission wurde in der Organisation noch nicht akzeptiert.

Loesung fuer Studierende:

- Vorlage [`../vorlagen/dotnet-build.yml`](../vorlagen/dotnet-build.yml) manuell nach `.github/workflows/dotnet-build.yml` kopieren.
- Tutor/Admin informieren.

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
