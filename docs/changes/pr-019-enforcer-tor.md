# pr-019 — maven-enforcer Dependency-Convergence als statisches Tor

| | |
|---|---|
| PR | [WeierE1/spring-petclinic#19](https://github.com/WeierE1/spring-petclinic/pull/19) |
| Branch | `cra/enforcer-tor` → `1.5.x` |
| Merged | 2026-09-04 13:30 UTC |
| Size | +108 / −0 über 2 Dateien |
| Issues | CRA-Private#40 (`spec.md` §25) |
| Review | Mensch |

## 1. Why

`spec.md` §25 verlangt **statische Tore**, die dem Menschen etwas Belastbares zu
lesen geben. Divergierende transitive Versionen sind die übliche Ursache eines
`NoSuchMethodError` im Betrieb — und sie sind maschinell erkennbar, ohne dass
irgendjemand ein Urteil fällen muss.

**Dieses Tor autorisiert kein Automerge.** Es liefert einen Befund; den Merge
entscheidet ein Mensch (§20.4).

## 2. What changed

`maven-enforcer-plugin` **3.6.3** (festgenagelt) mit `dependencyConvergence`,
gebunden an `validate`.

**Der Vorbestand wurde gemessen, bevor das Tor scharf wurde** — sonst wäre beim
ersten roten Lauf nicht unterscheidbar, was das Tor gefunden und was es geerbt
hat. Stand `12a83c2`, JDK 8, mit enforcer 3.5.0 **und** 3.6.3 (gleiches
Ergebnis): **eine** divergierende Koordinate, zwei Versionen, drei Pfade —
`org.webjars:jquery` in `2.2.4` gegen `1.11.1` über `jquery-ui` und `bootstrap`.
Der Befund liegt als `.cra/enforcer-vorbestand.md` im Repository, nicht nur im
PR-Text.

## 3. Files

| Path | Change |
|---|---|
| `pom.xml` | +42/−0 |
| `.cra/enforcer-vorbestand.md` | +66/−0 |

## 4. Verification

Der Vorbestand wurde mit **zwei** Plugin-Versionen gemessen (3.5.0 und 3.6.3) und
lieferte dasselbe Ergebnis — das schließt aus, dass der Befund eine Eigenschaft
der Plugin-Version ist. Ausgabe wörtlich in `.cra/enforcer-vorbestand.md`.

`nullbedingung` auf `1.5.x` nach dem Merge: **success**.

**Ehrlich gesagt:** ob der Lauf des Pull Requests selbst grün war und wie lange
er brauchte, ist hier nicht festgehalten — der Eintrag entstand fünf Tage später.

## 5. Known gaps

- **Ein Tor, das den Vorbestand nicht schließt.** `org.webjars:jquery` divergiert
  weiterhin; das Tor macht die Divergenz sichtbar, es behebt sie nicht. Ob und
  wie sie aufgelöst wird, ist eine eigene Entscheidung.
- **Nur auf `1.5.x`.** Die anderen Forks haben dieses Tor nicht.

## 6. Provenance

**Rekonstruiert am 09.09.2026.** Abschnitte 1 bis 3 folgen dem zeitgleich
geschriebenen PR-Text (einschließlich der zitierten Messung); Abschnitt 4 ist
nachträglich zusammengetragen und deshalb unvollständig — was am 04.09.2026 im
PR-Lauf stand, war nicht mehr greifbar.
