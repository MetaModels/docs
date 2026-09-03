# Workflow: DE → EN Doku-Sync

Dieses Repo (`docs`, Branch `en-2.0`) ist die englische Übersetzung von
[MetaModels/docs-de](https://github.com/MetaModels/docs-de) (Branch `de-2.0`).
`letzter-de-commit.md` im Repo-Root hält den Hash des zuletzt übersetzten
DE-Commits fest. Diese Datei beschreibt, wie ein neuer Sync-Lauf abläuft, damit
eine neue Claude-Session direkt damit gestartet werden kann.

## Prompt zum Starten

> "Wir haben hier `letzter-de-commit.md` den letzten Commit von DE festgehalten -
> bitte die Änderungen seit dem übernehmen, übersetzen und pushen."

## Voraussetzungen

- Lokaler Klon von `docs-de` existiert bereits unter
  `/home/lenovo/Projects/Firma/e-spin/MetaModels/docs-de` (git-Remote:
  `https://github.com/MetaModels/docs-de.git`, Branch `de-2.0`).
  - Dieser Klon hat oft unbezogene, unversionierte lokale Änderungen
    (z. B. `reference/api.rst`, `reference/events/`) — **nicht anfassen**,
    nur per `git show`/`git diff` gegen Refs lesen, nicht das Working Tree
    dort verändern oder committen.
- `gh auth status` ist eingeloggt (nötig, falls `docs-de` neu geklont werden
  muss oder GitHub-Zugriff sonst limitiert ist — unauthentifizierte
  `git clone` auf github.com schlägt in dieser Umgebung fehl).

## Ablauf

1. **Letzten übersetzten Commit ermitteln**: Hash aus `letzter-de-commit.md`
   lesen (erste Zeile).
2. **DE-Repo aktualisieren**: `git -C .../docs-de fetch origin` — danach
   `origin/de-2.0` als "neuer" Ref verwenden (nicht den lokalen `de-2.0`
   Branch, der kann veraltet sein).
3. **Geänderte Dateien ermitteln**:
   `git -C .../docs-de diff --name-status <alter-hash> origin/de-2.0`
4. **Binärdateien/Assets direkt kopieren** (keine Übersetzung nötig):
   Bilder (`_img/**`), PDFs (`_download/**`) etc. per
   `git -C .../docs-de show origin/de-2.0:<path> > <ziel-pfad-im-en-repo>`
   für jede M/A-Datei, die keine `.rst`/`.php`/`.yaml`-Textdatei ist.
   Reine Code-Beispieldateien (`.php`, `.yaml` in Cookbook-Beispielen) auch
   direkt kopieren, aber auf deutsche Kommentare im Code prüfen (`grep -P
   '[äöüÄÖÜß]'`) und die von Hand übersetzen.
5. **Textdateien (`.rst`) übersetzen**: Das ist der Hauptaufwand. Bei einer
   größeren Anzahl Dateien (siehe letzter Lauf: ~100 `.rst`-Dateien) lohnt es
   sich, mehrere `general-purpose`-Subagenten parallel zu starten, aufgeteilt
   nach Verzeichnis (z. B. `manual/component/attribute/*`,
   `manual/component/filter/*`, restliche `manual/component/*`,
   `manual/extended/*` + `manual/install.rst` + `manual/metamodel-first/*`,
   große neue Seiten wie `manual/new-in-mm-2X.rst` separat, `cookbook/*` +
   `reference/*`). Jeder Subagent bekommt:
   - Pfad zu beiden Repos + alten/neuen Ref-Hash
   - Seine Dateiliste mit Status (M = vorhandene EN-Datei anpassen, A = neue
     Datei komplett neu übersetzen und anlegen)
   - Anweisung: pro Datei `git diff <alt>..<neu> -- <pfad>` (bzw. bei A
     `git show <neu>:<pfad>` für den kompletten Inhalt) ansehen, die
     bestehende EN-Datei für Terminologie/Stil lesen, nur die geänderte/neue
     deutsche Prosa übersetzen, RST-Syntax/Code-Blöcke/Bildpfade/`:ref:`-Ziele
     unverändert lassen.
   - Hinweis: viele Diffs in diesem Projekt sind rein mechanisch (neues Icon
     am Seitenanfang + `.. |icon_x| image:: ...svg`-Definition am Ende) —
     das ist Markup, nicht Prosa, einfach unverändert übernehmen.
   - Explizit: kein `git add`/`commit`/`push`, nur Dateien im Working Tree
     ändern/anlegen.
6. **Neue Toctree-Einträge prüfen**: Wenn komplett neue Seiten dazukommen
   (z. B. `manual/new-in-mm-25.rst`), im DE-Repo prüfen, ob eine
   Index-/Toctree-Datei geändert wurde, die diese Seite einbindet, und den
   entsprechenden EN-Toctree-Eintrag ergänzen.
7. **Review**: `git status` / `git diff --stat` im EN-Repo gegenprüfen, dass
   die Anzahl der geänderten/neuen Dateien plausibel zur DE-Diffliste passt.
   Stichprobenartig ein paar größere Dateien lesen.
8. **`letzter-de-commit.md` aktualisieren**: Hash von `origin/de-2.0`
   (`git -C .../docs-de rev-parse origin/de-2.0`) reinschreiben.
9. **Committen & pushen** im EN-Repo (`en-2.0`-Branch), z. B.:
   `git add -A && git commit -m "Translate from german" && git push`.
   (Frühere Commits in diesem Repo für DE-Syncs heißen einfach
   "Translate from german" — Konvention beibehalten.)

## Stolpersteine aus dem letzten Lauf

- `git clone`/`git ls-remote` auf `github.com` ohne Auth schlägt in dieser
  Sandbox mit "GitHub is temporarily limiting some unauthenticated
  downloads" fehl — `gh` (bereits eingeloggt) oder den vorhandenen lokalen
  Klon unter `/home/lenovo/Projects/Firma/e-spin/MetaModels/docs-de`
  verwenden statt neu zu klonen.
- Der lokale `docs-de`-Klon war ~1600 Commits hinter `origin/de-2.0` (lokaler
  Branch veraltet) — deshalb immer `origin/de-2.0` nach einem `fetch`
  verwenden, nicht den lokalen Branch-Pointer.
- Der lokale `docs-de`-Klon hat ein dreckiges Working Tree mit unbezogenen
  Änderungen — nicht `git checkout`/`reset` darin ausführen, nur lesend via
  `git show`/`git diff` gegen Commit-Refs arbeiten.
- Manche Code-Beispieldateien (`.php`) enthalten trotzdem deutsche
  Kommentare — nicht blind als "Code, keine Übersetzung nötig" behandeln,
  kurz grep'en.
