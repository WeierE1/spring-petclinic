# pr-013 — PR-PROFILE.md aus echtem Lauf

| | |
|---|---|
| PR | [WeierE1/spring-petclinic#13](https://github.com/WeierE1/spring-petclinic/pull/13) |
| Branch | `cra/pr-profile` → `1.5.x` |
| Merged | 2026-09-09 06:01 UTC |
| Size | +88 / −0 über 1 Datei |
| Issues | CRA-Private#35 |
| Review | Mensch |

## 1. Why

Der Skill `pr-check` und die Vorprüfung aus `spec.md` §22.1 brauchen je
Repository ein Profil: welche Prüfungen laufen, was der **Vorbestand** ist, wie
lange ein Lauf dauert, welches JDK. Ohne diese Angaben liest ein Reviewer jeden
vorhandenen Fehlschlag als neu.

## 2. What changed

`.claude/PR-PROFILE.md`, **aus einem echten Lauf** und nicht aus der
Konfiguration abgeschrieben — die Regel aus `spec.md` §32: *ausführen und die
Ausgabe zitieren.*

- Grüner Referenzlauf:
  [33059641631](https://github.com/WeierE1/spring-petclinic/actions/runs/33059641631)
  (Push auf `1.5.x`, 27.08.2026)
- `Tests run: 41, Failures: 0, Errors: 0, Skipped: 1` — **Vorbestand: 0 rot,
  1 abgeschaltet** (`CrashControllerTests`), deckungsgleich mit CRA-Private#10
- Laufzeit 1 min 51 s, Kommando `mvn -B -ntp verify`, JDK 8 (Temurin,
  festgenagelt)

Der PR-Text hielt fest, dass **nicht automatisch gemergt** wird — Abnahme durch
einen Menschen (`mvp.md` §10 Nr. 3).

## 3. Files

| Path | Change |
|---|---|
| `.claude/PR-PROFILE.md` | +88/−0 |

## 4. Verification

CI dieses Pull Requests: `build` **pass**, zweimal (der Workflow läuft aus `push`
und aus `pull_request`). `nullbedingung` auf `1.5.x` nach dem Merge: **success**.

## 5. Known gaps

- **Das Profil altert.** Vorbestand, Laufzeit und JDK stammen vom 27.08.2026; es
  trägt sein Messdatum, damit die Alterung auffällt statt still zu wirken.
- **JDK 8.** Dieses Fork-Repository ist Stand 2017; ein Agent, der es baut, muss
  das passende JDK wählen — dieselbe Weiche, die für WebGoat (Java 25) in
  CRA-Private#44 gebaut wurde.

## 6. Provenance

**Teilweise rekonstruiert am 09.09.2026.** Abschnitte 1 bis 3 stammen aus dem
zeitgleich geschriebenen PR-Text; Abschnitt 4 ist aus `gh pr checks 13` und der
Lauf-Liste nachgetragen.
