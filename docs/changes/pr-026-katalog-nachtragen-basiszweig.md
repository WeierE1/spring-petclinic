# pr-026 — katalog nachtragen: Zielzweig aus dem Ereignis, Ruleset-Fall eindeutig

| | |
|---|---|
| PR | [WeierE1/spring-petclinic#26](https://github.com/WeierE1/spring-petclinic/pull/26) |
| Branch | `fix/katalog-nachtragen-basiszweig` → `1.5.x` |
| Merged | pending-datum-026 |
| Size | pending-size-026 |
| Issues | — |
| Review | offen (Mensch) |

## 1. Why

Der Job `nachtragen` checkte fest `main` aus — auf diesem Fork der
Upstream-Zweig, dem `infra/katalog-nachtragen.sh` fehlt; gearbeitet wird auf
`1.5.x`. Gemessen 24.09.2026, Lauf 35985887614: `bash:
infra/katalog-nachtragen.sh: No such file or directory`, Exit 127. Derselbe
Fehler seit dem 09.09.2026 — die Marken von pr-023 standen seither offen.

Entscheidung des Menschen vom 24.09.2026: der Job wird repariert, das
Ruleset `kein-merge-durch-automatik` bekommt **keine** Ausnahme für GitHub
Actions (§32/16 bleibt ohne Ausnahme). Nach der Reparatur des Zweigs scheitert
der Push hier also am Ruleset — der Job bleibt rot, aber mit einer Meldung,
die sagt, was zu tun ist, statt mit drei sinnlosen Rebase-Versuchen.

## 2. What changed

`.github/workflows/katalog.yml`, nur Job `nachtragen` (Job `pruefen` und
`permissions` unverändert; die Datei ist in allen drei Forks byte-gleich):

- Zielzweig aus dem Ereignis statt Literal `main`: `env: BASIS:
  ${{ github.event.pull_request.base.ref }}` auf Jobebene;
  `actions/checkout` mit `ref: ${{ github.event.pull_request.base.ref }}`;
  Push als `git push origin "HEAD:refs/heads/$BASIS"`, Rebase gegen
  `"$BASIS"`. In `run:` steht der Zweigname nur als Umgebungsvariable, nie
  als Ausdruck (Skript-Injektion über Zweignamen).
- Ruleset-Fall: enthält die Push-Ausgabe `GH013` oder `Repository rule
  violations`, wird nicht erneut versucht. Meldung in `$GITHUB_STEP_SUMMARY`
  und stderr („Ruleset verlangt einen Pull Request auf <Zweig> -- Marken
  dieses PR im naechsten PR von Hand fuellen: bash
  infra/katalog-nachtragen.sh <nr> …") und Exit 1. Andere Push-Fehler
  behalten Rebase und bis zu drei Versuche.
- PR-Nummer für die Meldung über `env: NUMMER` des Commit-Schritts.

`infra/katalog-nachtragen.sh` enthält kein fest verdrahtetes `main` und ist
unverändert.

Offene Marken gefüllt, beide mit
`GH_REPO=WeierE1/spring-petclinic bash infra/katalog-nachtragen.sh <nr>`:

- pr-023 → Merged `2026-09-09 09:08 UTC`, Size `+823 / −3 über 10 Dateien`.
- pr-025 → Merged `2026-09-24 10:12 UTC`, Size `+54 / −1 über 3 Dateien`.

## 3. Files

| Path | Change |
|---|---|
| `.github/workflows/katalog.yml` | Job `nachtragen`: Zielzweig, Ruleset-Fall |
| `docs/changes/pr-023-katalog-und-tor.md` | Marken gefüllt |
| `docs/changes/pr-025-renovate-preset-umzug.md` | Marken gefüllt |
| `docs/changes/README.md` | Indexzeilen pr-023/pr-025 gefüllt, Indexzeile pr-026 |
| `docs/changes/pr-026-katalog-nachtragen-basiszweig.md` | neu |

## 4. Verification

- Commit-Schritt aus der Datei ausgeschnitten und mit einem Stub für `git`
  gefahren (`BASIS=1.5.x`, `NUMMER=104`): GH013 → Meldung auf stderr und in
  der Zusammenfassung, **ein** Push-Versuch, Exit 1; Ablehnung ohne GH013 →
  drei Rebase-Versuche gegen `origin 1.5.x`, dann „Push nach drei Versuchen
  nicht moeglich.", Exit 1; Erfolg → `HEAD:refs/heads/1.5.x`, Exit 0.
- `grep -n main .github/workflows/katalog.yml` trifft nur noch Kommentare.
- Mojibake-Prüfung (`grep -c` auf U+00C3 über `docs/changes/*.md`): 0.
- `bash infra/katalog-pruefen.sh 26`: Katalogpflicht erfüllt (lokal vor dem
  Push).
- `actionlint` liegt nicht vor und wurde nicht installiert. Dass GitHub die
  Datei parst, belegt der Lauf von `katalog / pruefen` an diesem PR.

## 5. Known gaps

- **`nachtragen` bleibt nach dem Merge rot** — jetzt am Ruleset
  `kein-merge-durch-automatik` (`bypass_actors: []`, GH013), mit der Meldung,
  welche Marken im nächsten PR von Hand zu füllen sind. Entschieden
  24.09.2026, kein neuer Befund. Die Marken **dieses** PR füllt also der
  nächste PR: `bash infra/katalog-nachtragen.sh 26`.
- Dass die Ruleset-Meldung im echten Lauf erscheint, ist nur simuliert, nicht
  gemessen — Beleg ist der erste Post-Merge-Lauf.
- `infra/katalog-nachtragen.sh` erkennt eine spotless-ausgerichtete
  Kopfzeile (`| Size   |`) nicht und meldet trotzdem Exit 0 (gemessen in
  WebGoat). Hier ohne Folge, weil die Tabellen nicht ausgerichtet sind; nicht
  in diesem PR behoben.

## 6. Provenance

Geschrieben am 2026-09-24, im selben Zug wie der PR, von der Arbeitssitzung,
die den Job in allen drei Forks nach der Entscheidung des Menschen repariert
hat. Die Marken-Werte kommen aus `gh pr view`, nicht abgetippt.
