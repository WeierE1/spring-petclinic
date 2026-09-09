# pr-021 — Ausnahme §28.2: actuator als Risikoübernahme

| | |
|---|---|
| PR | [WeierE1/spring-petclinic#21](https://github.com/WeierE1/spring-petclinic/pull/21) |
| Branch | `cra/ausnahme-actuator` → `1.5.x` |
| Merged | 2026-09-08 07:33 UTC |
| Size | +14 / −2 über 1 Datei |
| Issues | CRA-Private#33, Abnahmeprüfung §32/21 |
| Review | Mensch — die Entscheidung selbst ist die eines Menschen (07.09.2026) |

## 1. Why

`spring-boot-starter-actuator` (`GHSA-mgvc-8q2h-5pgc`) hat für die
`1.5.x`-Linie **keine schließende Version**: die Fixes liegen in Spring Boot
3.5.12 und 4.0.4, also jenseits der Framework-Grenze dieser Linie (gemessen
31.08.2026). Nach `spec.md` §17.1 wird ein Framework-Sprung **nie** im Zuge einer
Sicherheitsanhebung durchgeführt — bleibt die zweigeteilte Ausnahme aus §28.2.

Dieser Pull Request ist die Hälfte, die **wirkt**. Die Hälfte, die
**dokumentiert**, bucht der Reconciler nach Dependency-Track.

## 2. What changed

`renovate.json` bekommt den Ausnahme-Eintrag: `enabled: false` für die Koordinate,
mit der Marke `cra-ausnahme` und einem `description`-Feld, das **nur Gemessenes**
nennt — keine Erreichbarkeits- oder Betroffenheitsaussage (§21.2, §37.1).

**Warum das zusammenhängt:** der Reconciler liest genau diesen Eintrag und bucht
daraufhin `EXPLOITABLE / NOT_SET / WILL_NOT_FIX` nach Dependency-Track. Die
Ausnahme wirkt also nicht, weil sie hier steht, sondern weil beide Hälften
zusammenkommen — und §28.2 verlangt beide.

**Review-Datum 2026-12-06.** Es gibt keine unbefristeten Ausnahmen (§28.3); nach
diesem Datum wirft der Verfallsmelder den Fall wieder auf.

## 3. Files

| Path | Change |
|---|---|
| `renovate.json` | +14/−2 |

## 4. Verification

```
renovate-config-validator → Config validated successfully
```

Diese Prüfung ist Pflicht vor jedem Scharfstellen: Renovate lehnt eine ungültige
Konfiguration **still** ab, und eine still abgelehnte Ausnahme sieht aus wie eine
wirkende.

`nullbedingung` auf `1.5.x` nach dem Merge: **success**.

**Nicht in diesem Eintrag belegt:** die zweite Hälfte (die DT-Buchung). Sie ist
in `CRA-Private` nachgewiesen — Abnahmeprüfung §32/21, Probelauf vom 08.09.2026:
`state=EXPLOITABLE justification=NOT_SET response=WILL_NOT_FIX suppressed=true`.

## 5. Known gaps

- **Der Eintrag entstand einen Tag nach dem Merge**, obwohl der PR-Text ihn
  ausdrücklich ankündigte: *„Katalogeintrag nach dem Merge, wie die CLAUDE.md
  dieses Forks es vorsieht."* Er kam nicht — bis zu diesem Nachtrag. Das ist der
  Beleg dafür, dass die Ankündigung allein nicht trägt.
- **Eine Ausnahme mit Verfallsdatum ist eine Zusage an die Zukunft.** Am
  06.12.2026 muss sie neu bewertet werden; ob das geschieht, hängt am
  Verfallsmelder und nicht an diesem Dokument.

## 6. Provenance

**Rekonstruiert am 09.09.2026.** Abschnitte 1 bis 3 folgen dem zeitgleich
geschriebenen PR-Text; Abschnitt 4 ist aus dem PR-Text und dem
CRA-Private-Katalog (`pr-122`) zusammengetragen.
