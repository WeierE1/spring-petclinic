# PR-PROFILE — WeierE1/spring-petclinic

Profil für den Skill `pr-check` und die §22.1-Vorprüfung des Migrations-Agenten
(WeierE1/CRA-Private, Issue #35). **Alle Zahlen stammen aus einem echten Lauf,
nicht aus der Konfiguration** — Beleg ist jeweils die Lauf-ID.

Default-Branch dieses Forks: **`1.5.x`** (nicht `main`).

## Welche Branches die CI auslöst

Ein einziger Workflow: `.github/workflows/nullbedingung.yml` (Name: `nullbedingung`).

```yaml
on:
  push:
  pull_request:
  workflow_dispatch:
```

Also: **jeder Push auf jeden Branch, jeder Pull Request, dazu manuell.**
Es gibt keinen Branch, der nicht baut, und keine Pfad-Ausnahmen.

## Die echten Build- und Testkommandos

Wörtlich aus dem Workflow (nicht aus dem README):

```
mvn -B -ntp verify
```

Danach (auch bei rotem Lauf, `if: always()`): Surefire-XML als Artefakt
`surefire-reports`, SBOM-Erzeugung per
`mvn -B -ntp org.cyclonedx:cyclonedx-maven-plugin:2.7.11:makeAggregateBom`,
Artefakt `sbom`.

Der Checkout läuft mit `fetch-depth: 0` — das `git-commit-id-plugin` 2.2.2
scheitert an flachen Klonen (`Missing commit …`); das ist eine Eigenschaft des
Klons, kein Projektbefund.

## Festgenagelte JDK-Version

**JDK 8** (Temurin), im Workflow festgenagelt. 2.7.11 ist die letzte
cyclonedx-Plugin-Fassung, die auf JDK 8 läuft.

## Altlasten (Vorbestand)

Aus dem grünen Lauf [33059641631](https://github.com/WeierE1/spring-petclinic/actions/runs/33059641631)
(Push auf `1.5.x`, 2026-08-27):

```
Tests run: 41, Failures: 0, Errors: 0, Skipped: 1
```

- **0 vorbestehend rote Tests.**
- **1 vorbestehend abgeschalteter Test** — in
  `org.springframework.samples.petclinic.system.CrashControllerTests`
  (`Tests run: 1, Failures: 0, Errors: 0, Skipped: 1`).

Dieselbe Zahl hat Issue #10 in CRA-Private festgehalten („41, davon 1
vorbestehend abgeschaltet"). Wer nach einer Änderung mehr als 1 übersprungenen
oder irgendeinen roten Test sieht, schaut auf die Änderung, nicht auf den
Vorbestand.

## Laufzeit eines vollständigen Laufs

**1 min 51 s** (Lauf 33059641631, inkl. SBOM-Schritte). Ein früherer grüner
Lauf lag bei 48 s (32948174322) — die Spanne kommt vom Maven-Cache des Runners.
Für §22.4: eine Dreierschleife kostet hier unter 6 Minuten und ist bezahlbar.

## Bekannte Flakes

**Keine bekannt** (Stand 2026-09-01). Kein dokumentierter Fall von rot→grün auf
demselben Commit.
