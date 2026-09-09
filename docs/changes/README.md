# Change catalogue

Ein Eintrag je gemergtem Pull Request **dieses Forks**. Warum es das gibt: ein
PR-Text sagt, was der Autor vorhatte — nicht, was verifiziert wurde und was offen
blieb. Konvention und Begründung: `docs/changes/` im Automatisierungs-Repository
[`WeierE1/CRA-Private`](https://github.com/WeierE1/CRA-Private/tree/main/docs/changes).

**Was dieses Repository ist:** ein Fork von `spring-projects/spring-petclinic`,
eingefroren auf dem Zweig `1.5.x` (Stand 03.11.2017). Er ist **Testobjekt** des
CRA-Piloten — er liefert echte Advisories eines echten historischen Standes und
eine echte Testsuite. Die Upstream-Historie gehört nicht zu diesem Katalog.

<!-- INDEX:BEGIN -->

| PR | Merged (UTC) | Title | Issues | Size | Detail |
|---|---|---|---|---|---|
| [pr-023](https://github.com/WeierE1/spring-petclinic/pull/23) | pending-datum-023 | Katalogrückstand aufholen und das Tor mitziehen | CRA-Private#44 | pending-size-023 | [→](pr-023-katalog-und-tor.md) |
| [pr-013](https://github.com/WeierE1/spring-petclinic/pull/13) | 2026-09-09 06:01 | PR-PROFILE.md aus echtem Lauf | CRA-Private#35 | +88/−0 · 1 | [→](pr-013-pr-profile-aus-echtem-lauf.md) |
| [pr-021](https://github.com/WeierE1/spring-petclinic/pull/21) | 2026-09-08 07:33 | Ausnahme 28.2: actuator als Risikoübernahme | CRA-Private#33 | +14/−2 · 1 | [→](pr-021-ausnahme-actuator.md) |
| [pr-019](https://github.com/WeierE1/spring-petclinic/pull/19) | 2026-09-04 13:30 | maven-enforcer Dependency-Convergence als statisches Tor | CRA-Private#40 | +108/−0 · 2 | [→](pr-019-enforcer-tor.md) |
| [pr-009](https://github.com/WeierE1/spring-petclinic/pull/9) | 2026-08-27 09:40 | Katalog: Doku-PRs nachgetragen | — | +72/−0 · 3 | [→](pr-009-katalog-doku-prs-nachgetragen.md) |
| [pr-003](https://github.com/WeierE1/spring-petclinic/pull/3) | 2026-08-27 09:37 | CLAUDE.md: Rolle im CRA-Piloten, Regeln, Katalog-Pflicht | — | +31/−0 · 1 | [→](pr-003-claude-md-rolle-regeln-katalog.md) |
| [pr-002](https://github.com/WeierE1/spring-petclinic/pull/2) | 2026-08-27 09:30 | Change catalogue anlegen | — | +81/−0 · 2 | [→](pr-002-change-catalogue-anlegen.md) |
| [pr-001](https://github.com/WeierE1/spring-petclinic/pull/1) | 2026-08-27 07:05 | Renovate-Konfiguration: description statt // | CRA-Private#24 | +7/−9 · 1 | [→](pr-001-renovate-konfiguration-description.md) |

<!-- INDEX:END -->

## Was der Katalog nicht abdeckt

**Fünf Commits erreichten den Standardzweig direkt, ohne PR und ohne Review** —
per Contents-API, bevor das Ruleset `kein-merge-durch-automatik` (27.08.2026)
den Direktpush unterband:

| Commit | Datum | Was |
|---|---|---|
| `85187b43` | 26.08. 08:28 | `nullbedingung.yml` — CI-Workflow nach spec.md §34, JDK 8 festgenagelt |
| `90e79b0b` | 26.08. 08:31 | `fetch-depth: 0` — `git-commit-id-plugin` 2.2.2 scheitert an flachen Klonen |
| `d816f8a7` | 26.08. 08:31 | Beschriftung an die JDK-Version angeglichen |
| `fca0cf19` | 26.08. 08:59 | SBOM-Schritt (CycloneDX 2.7.11 — letzte Fassung für JDK 8), V8-Messung, Integritätslog |
| `ac3c0f52` | 27.08. 07:02 | erste `renovate.json` — trug noch `//`-Schlüssel, korrigiert in pr-001 |

Sie waren Arbeitsschritte des Piloten; ihre Begründung und Verifikation stehen in
den Katalog-Einträgen pr-051, pr-053 und pr-058 des Automatisierungs-Repositories.
