# Enforcer-Vorbestand: divergierende Abhängigkeitsversionen

Ausgangsstand für das statische Tor `maven-enforcer-plugin` /
`dependencyConvergence` in der `pom.xml` (Phase `validate`). Herkunft:
[WeierE1/CRA-Private Issue #40](https://github.com/WeierE1/CRA-Private/issues/40),
`docs/spec.md` §25. Das Tor autorisiert **kein** Automerge — es gibt dem
Menschen etwas Belastbares zu lesen.

**Die Regel** (analog zur Vorbestandsregel des Testtors, spec.md §24.4 Nr. 3):
Vorbestand zählen und berichten, nicht wachsen lassen. Ausgeschlossen wird
nie eine Koordinate, sondern genau der vorbestehende **Knoten** — in
Bereichsform `[version]`, weil eine nackte Version im enforcer-Muster
`>= version` bedeutet und damit jede spätere Divergenz derselben Koordinate
mit schlucken würde (gemessen 04.09.2026: mit `org.webjars:jquery:1.11.1`
lief der Bump jquery-ui → 1.12.1 grün durch, mit `[1.11.1]` schlägt er an).

## Vorbestand, gemessen am 04.09.2026

Stand `12a83c2` (Zweig `1.5.x`), JDK 8 (Temurin 1.8.0_504), Maven 3.9.12,
`maven-enforcer-plugin` 3.5.0 und 3.6.3 (gleiches Ergebnis):

```
mvn -B -ntp org.apache.maven.plugins:maven-enforcer-plugin:3.6.3:enforce -Denforcer.rules=dependencyConvergence
EXIT-CODE: 1
```

**Eine** divergierende Koordinate, zwei Versionen, drei Pfade:

```
Dependency convergence error for org.webjars:jquery:jar:2.2.4 paths to dependency are:
+-org.springframework.samples:spring-petclinic:jar:1.5.1
  +-org.webjars:jquery:jar:2.2.4:compile
and
+-org.springframework.samples:spring-petclinic:jar:1.5.1
  +-org.webjars:jquery-ui:jar:1.11.4:compile
    +-org.webjars:jquery:jar:1.11.1:compile
and
+-org.springframework.samples:spring-petclinic:jar:1.5.1
  +-org.webjars:bootstrap:jar:3.3.6:compile
    +-org.webjars:jquery:jar:1.11.1:compile
```

| Koordinate | gewinnt (nearest) | verdrängt | über | Ausschluss in `pom.xml` |
|---|---|---|---|---|
| `org.webjars:jquery` | 2.2.4 (direkt) | 1.11.1 | `jquery-ui:1.11.4`, `bootstrap:3.3.6` | `org.webjars:jquery:[1.11.1]` |

Das ist ein **Befund**, kein Grund, das Tor zu schwächen: jQuery 1.11.1 wird
im Betrieb durch 2.2.4 ersetzt, ohne dass irgendjemand das je geprüft hat.

## Was das Tor damit fängt und was nicht

- **Fängt:** jede weitere Version von `org.webjars:jquery` im Baum (gemessen:
  Bump `jquery-ui` 1.11.4 → 1.12.1 bringt `jquery:1.12.0` → `BUILD FAILURE`),
  und jede Divergenz auf jeder anderen Koordinate.
- **Fängt nicht:** einen weiteren Pfad, der ebenfalls genau `jquery:1.11.1`
  mitbringt (gemessen: Bump `bootstrap` 3.3.6 → 3.4.1 bringt weiter 1.11.1 →
  `passed`). Das ist der Vorbestand selbst, nicht sein Wachstum.

## Änderungsprotokoll

Einträge werden **annotiert, nie umgeschrieben** (spec.md §20.4). Wer den
Vorbestand auflöst (z. B. `webjars-jquery.version` auf die transitiv
verlangte Version zieht oder umgekehrt), streicht den `<exclude>` in der
`pom.xml` und trägt es hier mit Datum ein.

- 2026-09-04 — Ausgangsstand gemessen und Tor scharf gestellt (Issue #40).
