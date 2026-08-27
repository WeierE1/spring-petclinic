# Agent context

## Was dieses Repository ist

**Testobjekt des CRA-Piloten** — ein Fork von `spring-projects/spring-petclinic`,
eingefroren auf Zweig `1.5.x` (Stand 03.11.2017). Es liefert echte Advisories
eines echten historischen Standes plus eine echte Testsuite (41 Tests).

Gesteuert wird alles aus [`WeierE1/CRA-Private`](https://github.com/WeierE1/CRA-Private)
— Spec, Issues, Presets, Learnings liegen **dort**. Hier liegt nur, was zwingend
im Zielrepo liegen muss: CI-Workflow, `renovate.json`, dieser Katalog.

## Regeln

- **Der Stand von 2017 ist der Zweck, kein Mangel.** Keine Abhängigkeit von Hand
  anheben, nichts von upstream nachziehen — jede „Modernisierung" zerstört die
  Fallmenge des Piloten. Versionen hebt ausschließlich die Engine (Renovate)
  bzw. später der Anhebungs-Agent, per PR.
- **Kein Direktpush auf `1.5.x`** — Ruleset `kein-merge-durch-automatik` ist
  aktiv und lehnt ihn mit 409 ab. Alles per PR.
- **JDK 8 ist festgenagelt** (Workflow). JDK ≥20 hat `-source 7` entfernt;
  `git-commit-id-plugin` braucht `fetch-depth: 0`.
- **Kein Test wird abgeschwächt, gelöscht oder übersprungen, um grün zu werden**
  (spec.md §24.1 — das Tor vergleicht Test-Identitäten).

## Dokumentation

**Nach jedem gemergten PR:** Eintrag in [`docs/changes/`](docs/changes/README.md)
— Konvention (sechs Abschnitte, Index-Regeln, Ehrlichkeitsregeln) steht im Index
von `CRA-Private/docs/changes/` und im Skill `change-catalogue`. Learnings, die
über dieses Repo hinausgehen, gehören nach `CRA-Private/LEARNINGS.md`.
