# pr-023 — Katalogrückstand aufholen und das Tor mitziehen

| | |
|---|---|
| PR | [WeierE1/spring-petclinic#23](https://github.com/WeierE1/spring-petclinic/pull/23) |
| Branch | `docs/katalog-und-tor` → `1.5.x` |
| Merged | pending-datum-023 |
| Size | pending-size-023 |
| Issues | CRA-Private#44 (Nachlauf) |
| Review | Mensch |

## 1. Why

Vier gemergte Pull Requests dieses Forks hatten keinen Katalogeintrag: #9, #13,
#19 und #21. Die Pflicht steht seit dem 27.08.2026 09:37 UTC in der `CLAUDE.md`
(PR #3) — der früheste fehlende PR wurde **drei Minuten später** gemergt. Es lag
also nicht am Alter, sondern am Mechanismus: es gab **kein Tor**, und der
Wortlaut sagte *„nach jedem gemergten PR"* statt *„im PR"*.

Zwei der vier sind Nachweismaterial, kein Doku-Rauschen: #19 (maven-enforcer als
statisches Tor, §25) und #21 (die §28.2-Risikoübernahme für `actuator`, deren
zweite Hälfte in Dependency-Track gebucht ist). Der PR-Text von #21 kündigte den
Katalogeintrag sogar ausdrücklich an — er kam nicht.

## 2. What changed

- **Die vier fehlenden Einträge** (`pr-009`, `pr-013`, `pr-019`, `pr-021`), jeder
  mit den sechs Pflichtabschnitten und einem ehrlichen Abschnitt 6: sie sind
  **rekonstruiert**, nicht zeitgleich geschrieben.
- **Das Tor:** `.github/workflows/katalog.yml` plus `infra/katalog-pruefen.sh`
  und `infra/katalog-nachtragen.sh`, wörtlich aus `CRA-Private`. Kein Secret,
  GitHub-gehostet, fasst den Bau nicht an.
- **Der Wortlaut in `CLAUDE.md`:** der Eintrag gehört ab jetzt **in** den Pull
  Request; die Marken `pending-datum-NNN` / `pending-size-NNN` füllt die Pipeline
  beim Merge.
- **Ein toter Verweis behoben:** `CRA-Private/LEARNINGS.md` gibt es seit
  `a0db55a` nicht mehr — es ist `docs/solutions/`.

**Die Ausnahme, ohne die das Tor mehr kaputtmacht als es hilft:** ein Engine-PR
*kann* keinen Katalogeintrag tragen — er darf nach `spec.md` §18.3
**ausschließlich Manifeste** berühren. Ohne Ausnahme wäre jeder Renovate- und
Agenten-PR hier rot, §18.4 nie erfüllt und die Zahl aus §30.3 dauerhaft 0.
Branches `renovate/**`, `cra/anhebung/**`, `cra/migration/**` sind deshalb
ausgenommen — am Branch erkannt, laut protokolliert, Exit 0.

## 3. Files

| Path | Change |
|---|---|
| `docs/changes/pr-009-…`, `pr-013-…`, `pr-019-…`, `pr-021-…`, `pr-023-…` | neu |
| `docs/changes/README.md` | fünf Indexzeilen |
| `.github/workflows/katalog.yml` | neu |
| `infra/katalog-pruefen.sh`, `infra/katalog-nachtragen.sh` | neu |
| `CLAUDE.md` | Katalogabsatz ersetzt |

## 4. Verification

```
bash infra/katalog-pruefen.sh 9|13|19|21|23  → Katalogpflicht erfuellt
PR_BRANCH=renovate/x            … 999        → AUSGENOMMEN: Engine-Vorgang (Exit 0)
PR_BRANCH=docs/katalog-und-tor  … 999        → Exit 1   (richtig: rot ohne Eintrag)
```

**Das Tor hat sich beim ersten Lauf selbst bewiesen.** Der erste Push dieses Pull
Requests enthielt die Einträge für #9, #13, #19 und #21 — aber keinen für **sich
selbst**, und `pruefen` wurde rot. Genau die Lücke, die dieser PR beschreibt
(PR #9 hatte sie im eigenen Text vorhergesagt und nie geschlossen), fing das Tor
in dem Moment, in dem es scharf war. Dieser Eintrag ist die Antwort darauf.

## 5. Known gaps

- **Drei Dateien liegen jetzt doppelt** (`katalog.yml` und die zwei Skripte, hier
  und in `CRA-Private`). Bewusste Doppelung: `CRA-Private` ist privat, ein Fork
  kann das Skript nicht holen. Sie driftet, wenn jemand nur eine Kopie ändert.
- **Ein Workflow mehr im Testobjekt.** Er prüft nur unsere eigenen Dateien, aber
  der Fork entfernt sich damit ein Stück weiter vom Upstream.
- **Die Ausnahme kennt drei Branch-Präfixe.** Ein vierter (ein neuer Agent) fiele
  als *rotes Tor* auf, nicht still — die sichere Richtung, aber sie kostet einen
  Handgriff.

## 6. Provenance

**Zeitgleich geschrieben**, im Pull Request und vor dem Merge — anders als die
vier Einträge, die er nachträgt.
