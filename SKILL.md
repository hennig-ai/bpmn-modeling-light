# BPMN Modeling Light — Skill

## Skill-Beschreibung (für Claude Code)

Skill-Name: `bpmn-modeling`

Trigger: Verwende diesen Skill **ausschließlich** wenn der Nutzer explizit ein **BPMN-Modell** erstellen, erweitern oder modifizieren möchte. Das Wort "BPMN" (oder eindeutige BPMN-Fachbegriffe wie "Start Event", "Sequence Flow", "Gateway", "User Task") muss im Auftrag vorkommen.

Beispiele die triggern:
- "Erstelle ein BPMN-Modell für einen Bestellprozess"
- "Modelliere das als BPMN"
- "Füge ein Exclusive Gateway hinzu" (im Kontext eines bestehenden BPMN-Modells)
- "Lass uns das BPMN-Modell bestellprozess.md anpassen" (Modifikation einer bestehenden BPMN-Datei)

**NICHT triggern bei:**
- "Lass uns einen Prozess beschreiben" (kein BPMN-Bezug)
- "Modelliere einen Bestellprozess" (kein expliziter BPMN-Bezug)
- "Erstelle ein Datenmodell" (anderer Modelltyp)
- Allgemeine Prozessbeschreibungen ohne BPMN-Kontext
- Allgemeine BPMN-Fragen ohne Modellierungsauftrag

---

## Zweck

Dieser Skill erzeugt und modifiziert BPMN-Prozessmodelle im Markdown-Tabellenformat gemäß dem BPMN Database Schema v3.0. Die Ausgabe ist ein vollständiges, konsistentes Set von Markdown-Tabellen, das alle BPMN-Elemente des modellierten Prozesses enthält.

---

## Referenz-Dateien

Dieser Skill arbeitet mit folgenden Referenzdokumenten im selben Repository:

| Datei | Zweck |
|-------|-------|
| `references/bpmn-instance-example.md` | Referenz-Instanz: verbindliches Tabellenformat und Beispieldaten |
| `references/bpmn-hierarchy.md` | Vererbungshierarchie der BPMN-Elemente, `element_type`-Übersicht |
| `references/event-detail-tables.md` | Event-Definition-Tabellen und deren Verhältnis zu `element_type` |
| `references/bpmn-schema.md` | Vollständige Schema-Dokumentation aller Tabellen, Spalten, Constraints, Value Domains |

### Lade-Strategie

**Vor jeder Modellierung PFLICHT laden:**
- `references/bpmn-instance-example.md` — verbindliches Tabellenformat und FK-Beziehungsmuster
- `references/bpmn-hierarchy.md` — `element_type`-Zuordnung
- `references/event-detail-tables.md` — Event-Definition-Abgrenzung

**Bei Bedarf nachschlagen:**
- `references/bpmn-schema.md` — nur wenn eine spezifische Tabellen-Definition unklar ist (z.B. Spalten, Constraints, Value Domains einer bestimmten Tabelle)

---

## Modus-Erkennung

Zu Beginn jeder Interaktion den Modus bestimmen:

### Modus A: Neumodellierung
- Nutzer beschreibt einen Prozess ohne bestehendes Modell
- Nutzer möchte "ein neues Modell erstellen"
- → Vollständiges neues Modell von Grund auf erzeugen

### Modus B: Modifikation
- Nutzer liefert ein bestehendes Modell (Markdown-Tabellen) und beschreibt Änderungen
- Nutzer möchte Elemente hinzufügen, entfernen oder ändern
- → Bestehendes Modell übernehmen, gezielt modifizieren, vollständiges Modell ausgeben

**Bei Unklarheit**: Explizit fragen ob ein bestehendes Modell vorliegt.

---

## Ausgabe-Format

- **Ausschließlich** das Markdown-Tabellenformat aus `references/bpmn-instance-example.md` verwenden
- Alle Tabellen mit `## Tabellenname` als Überschrift
- Vollständiges Modell ausgeben (nicht nur geänderte Tabellen)
- Keine zusätzlichen Erläuterungen zwischen den Tabellen
- Erläuterungen zu Designentscheidungen **vor** oder **nach** dem Modell, nicht darin

---

## Regeln

### 1. Format-Konformität
- **EXAKT** das Markdown-Tabellenformat aus `references/bpmn-instance-example.md` verwenden
- Keine Abweichungen in Formatierung, Spaltenreihenfolge oder Struktur
- Spaltennamen exakt wie im Schema definiert
- Leere/nicht befüllte optionale Felder: `##!empty!##` als Platzhalterwert

### 2. Tabellenreihenfolge
Die Tabellen müssen in der folgenden kanonischen Reihenfolge ausgegeben werden. Tabellen ohne Datenzeilen werden weggelassen (siehe Regel 3).

1. `bpmn_model`
2. `bpmn_process`
3. `bpmn_element`
4. `message_definition`
5. `signal_definition`
6. `error_definition`
7. `process_element`
8. `activity`
9. `task`
10. `service_task`
11. `user_task`
12. `script_task`
13. `business_rule_task`
14. `subprocess`
15. `call_activity`
16. `event`
17. `message_event_definition`
18. `timer_event_definition`
19. `error_event_definition`
20. `signal_event_definition`
21. `sequence_flow`
22. `gateway`
23. `message_flow`
24. `association`
25. `pool`
26. `lane`
27. `lane_element`
28. `data_object`
29. `data_store`
30. `text_annotation`
31. `data_input`
32. `data_output`
33. `data_association`

### 3. Tabellengenerierung — KRITISCH
- **NIEMALS leere Tabellen ausgeben**
- Eine Tabelle wird NUR dann in die Ausgabe aufgenommen, wenn sie mindestens eine Datenzeile (nicht nur Header) enthält
- **VERBOTEN**: Tabellen mit nur Header-Zeile ohne Datenzeilen
- **ERLAUBT**: Nur Tabellen mit Header + mindestens einer Datenzeile
- Die Reihenfolge der tatsächlich generierten Tabellen muss der obigen Liste (Regel 2) entsprechen

### 4. Primary Keys (PK)
- Format: dreistellige Integer mit führenden Nullen (z.B. `001`, `002`)
- Ausnahme: `bpmn_model_id` — Format `YYYYMMDD_HHMM_SS_NNN` (z.B. `20251026_1730_56_001`)
- Eindeutig innerhalb jeder Tabelle
- Bei neuen Einträgen: Höchste vorhandene PK + 1
- Bestehende PKs werden NIE geändert

### 5. Foreign Keys (FK) — Referenzielle Integrität
- FK-Werte MÜSSEN in der referenzierten Tabelle existieren
- Schema-Notation: `<tabelle>.<spalte>` zeigt Ziel der Referenz
- FK-Werte müssen fachlich sinnvoll sein (nicht willkürlich)
- Bei Modifikationen: Prüfe alle bestehenden FKs auf Konsistenz nach der Änderung

### 6. KRITISCH: element_type Befüllung

#### Grundregel
`element_type` enthält den SPEZIFISCHSTEN Typ der Vererbungshierarchie, **der eine eigene `bpmn_element_id` besitzt**.

#### Bestimmungs-Algorithmus
1. Identifiziere alle Tabellen, in denen das Element Einträge hat
2. Finde die TIEFSTE Tabelle, die `bpmn_element_id` als FK referenziert
3. Der Tabellenname (ohne Prä-/Suffix) = element_type

#### Spezialisierung über Subtyp-Tabellen vs. Typ-Spalten

Das Schema verwendet zwei Mechanismen zur Spezialisierung:

| Mechanismus | element_type | Spezialisierung über |
|-------------|--------------|---------------------|
| **Subtyp-Tabelle** | Tabellenname | Separate Tabelle mit eigener PK |
| **Typ-Spalte** | Eltern-Tabellenname | Spalte in der Eltern-Tabelle |

#### Übersicht aller Element-Typen und ihrer Spezialisierung

| element_type | Weitere Spezialisierung | Spalte/Tabelle |
|--------------|------------------------|----------------|
| `event` | event_type (start, end, intermediate_catch, intermediate_throw, boundary) | `event.event_type` |
| `event` | event_definition_type (message, timer, error, signal, none, ...) | `event.event_definition_type` |
| `gateway` | gateway_type (exclusive, inclusive, parallel, event_based, complex) | `gateway.gateway_type` |
| `gateway` | gateway_direction (converging, diverging, mixed) | `gateway.gateway_direction` |
| `subprocess` | is_transaction, triggered_by_event | `subprocess.*` |
| `service_task` | - | Subtyp-Tabelle von `task` |
| `user_task` | - | Subtyp-Tabelle von `task` |
| `script_task` | - | Subtyp-Tabelle von `task` |
| `business_rule_task` | - | Subtyp-Tabelle von `task` |
| `call_activity` | - | Subtyp-Tabelle von `activity` |
| `sequence_flow` | is_default, condition_expression | `sequence_flow.*` |
| `data_object` | is_collection, state | `data_object.*` |

#### WICHTIG: Event Definitions sind KEINE bpmn_elements
Die Tabellen `message_event_definition`, `timer_event_definition`, `error_event_definition`, `signal_event_definition` haben KEINE `bpmn_element_id` — sie referenzieren `event_id`. Daher ist für Events mit Definition der `element_type` immer `event`, die Spezialisierung erfolgt über `event.event_definition_type`.

#### WICHTIG: Gateways haben KEINE Subtyp-Tabellen
Es gibt keine separaten Tabellen für `exclusive_gateway`, `parallel_gateway` etc. Der `element_type` ist immer `gateway`, die Spezialisierung erfolgt über `gateway.gateway_type`.

#### Häufige Fehler vermeiden

**Tasks:**
❌ FALSCH: element_type='task' für einen User Task
✅ RICHTIG: element_type='user_task'

❌ FALSCH: element_type='activity' für einen Service Task
✅ RICHTIG: element_type='service_task'

**Events:**
❌ FALSCH: element_type='message_event_definition' für ein Message Event
✅ RICHTIG: element_type='event' (Spezialisierung über event.event_definition_type='message')

**Gateways:**
❌ FALSCH: element_type='exclusive_gateway' für ein XOR-Gateway
✅ RICHTIG: element_type='gateway' (Spezialisierung über gateway.gateway_type='exclusive')

❌ FALSCH: element_type='parallel_gateway' für ein AND-Gateway
✅ RICHTIG: element_type='gateway' (Spezialisierung über gateway.gateway_type='parallel')

### 7. Datenintegrität
- Beachte alle im Schema definierten Constraints
- Verwende nur Werte aus definierten Value Domains (siehe `references/bpmn-schema.md`)
- Stelle sicher, dass Vererbungsbeziehungen konsistent sind
- Jeder ausführbare Prozess (`is_executable=true`) muss mindestens ein Start- und ein End-Event haben

---

## Arbeitsweise

### Modus A: Neumodellierung

1. Fachliche Anforderungen vollständig verstehen — bei Unklarheiten nachfragen
2. BPMN-Konzepte auf Datenbankstrukturen mappen (Referenz: `references/bpmn-hierarchy.md`)
3. `bpmn_model_id` nach dem Format `YYYYMMDD_HHMM_SS_001` generieren (aktuelles Datum)
4. Alle Tabellen von oben nach unten befüllen (kanonische Reihenfolge, Regel 2)
5. Vollständiges Modell ausgeben

### Modus B: Modifikation

1. Bestehendes Modell analysieren — alle PKs, FKs und Beziehungen erfassen
2. Gewünschte Änderungen mit dem Nutzer klären falls nötig
3. Nur die betroffenen Tabellen modifizieren
4. Neue Einträge: PK = bisheriges Maximum + 1
5. Gelöschte Elemente: alle abhängigen FK-Referenzen prüfen und bereinigen
6. **Vollständiges** Modell ausgeben (nicht nur geänderte Tabellen)

### Bei Unklarheiten
- Frage spezifisch nach fehlenden Details
- Kläre Mehrdeutigkeiten vor der Ausgabe
- Bestätige kritische Designentscheidungen (z.B. Transaktions-Subprocess, Event-Based Gateway)
- Weise auf potenzielle BPMN-Konformitätsprobleme hin

---

## Qualitätssicherung

Vor jeder Ausgabe MÜSSEN folgende Prüfungen durchgeführt werden:

1. **Element-Type Korrektheit**: Prüfe `element_type` für JEDEN Eintrag in `bpmn_element`
2. **FK-Validierung**: Validiere alle Foreign Key Beziehungen tabellenübergreifend
3. **BPMN-Konformität**: Prüfe alle Validierungsregeln aus `references/bpmn-schema.md`
4. **Prozess-Vollständigkeit**: Bei `is_executable=true` — mindestens ein Start- und ein End-Event vorhanden?
5. **Konnektivität**: Sind alle Flow Objects über Sequence Flows erreichbar?
6. **Keine leeren Tabellen**: Entferne alle Tabellen ohne Datenzeilen aus der Ausgabe
7. **Tabellenreihenfolge**: Entspricht die Reihenfolge der kanonischen Ordnung (Regel 2)?
8. **PK-Eindeutigkeit**: Sind alle PKs innerhalb ihrer Tabelle eindeutig?

---

## Fehlerbehandlung

Bei Verletzung von BPMN-Regeln:
- Informiere den Nutzer über die spezifische Regelverletzung
- Schlage eine konforme Alternative vor
- Erstelle KEINE nicht-konformen Modelle

Bei widersprüchlichen Anforderungen:
- Benenne den Widerspruch explizit
- Frage welche Anforderung Vorrang hat
- Modelliere erst nach Klärung
