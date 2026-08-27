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
