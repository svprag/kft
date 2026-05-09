# Turnierumstellung: 20 Mannschaften (4 Gruppen × 5 Teams)

## Hintergrund

Das Turnier hat nur 20 statt 24 Mannschaften. Statt 6 Gruppen à 4 Teams gibt es nun **4 Gruppen à 5 Teams**. Die **ersten 4 jeder Gruppe** qualifizieren sich fürs Achtelfinale (4×4=16 Teams → 8 AF-Spiele).

## Vorgeschlagene Änderungen

### Spielplan-Struktur (neuer Modus)

**Gruppenphase:**
- 4 Gruppen (A, B, C, D) mit je 5 Teams
- Jedes Team spielt gegen jedes andere in der Gruppe → **10 Spiele pro Gruppe = 40 Gruppenspiele**
- 2 Plätze parallel → 20 Zeitslots für Gruppenspiele

**Achtelfinale (8 Spiele):**
- Direkte Qualifikation der Plätze 1-4 jeder Gruppe
- Paarungen:
  - AF41: 1.A vs 4.B
  - AF42: 1.B vs 4.A
  - AF43: 1.C vs 4.D
  - AF44: 1.D vs 4.C
  - AF45: 2.A vs 3.B
  - AF46: 2.B vs 3.A
  - AF47: 2.C vs 3.D
  - AF48: 2.D vs 3.C

**Viertelfinale (4 Spiele):**
- VF49: Sieger AF41 vs Sieger AF45
- VF50: Sieger AF42 vs Sieger AF46
- VF51: Sieger AF43 vs Sieger AF47
- VF52: Sieger AF44 vs Sieger AF48

**Halbfinale (2 Spiele):**
- HF53: Sieger VF49 vs Sieger VF50
- HF54: Sieger VF51 vs Sieger VF52

**Spiel um Platz 3:**
- Spiel 55: Verlierer HF53 vs Verlierer HF54

**Finale:**
- Spiel 56: Sieger HF53 vs Sieger HF54

> [!IMPORTANT]
> Die Achtelfinale-Paarungen (Kreuzsetzung) sind oben als Standard-Vorschlag. Bitte bestätige ob diese Paarungen so gewünscht sind.

---

### Google Spreadsheet

#### [MODIFY] Blatt "Spiele"
- Gruppenspiele von 36 auf 40 ändern (je 10 pro Gruppe statt 6)
- Gruppen E und F entfernen
- Pro 5er-Gruppe 10 Spiele statt 6 (5C2 = 10 Kombinationen)
- Achtelfinale-Paarungen anpassen: Platz 1-4 jeder Gruppe qualifizieren sich direkt
- "Beste Dritte" Logik komplett entfernen
- Uhrzeiten und SpielIDs anpassen

#### [MODIFY] Blatt "Mannschaften"
- Gruppen E und F entfernen  
- Gruppen A-D jeweils auf 5 Teams erweitern

---

### Applikation

#### [MODIFY] [index.html](file:///c:/repos/kft/index.html)

**Demo-Daten (`loadDemoData`):**
- Teams: 4 Gruppen × 5 = 20 Teams (statt 6×4=24)
- Gruppenspiele: 40 Spiele (statt 36)
- Achtelfinale: 8 Spiele mit neuer Paarungslogik
- VF/HF/Finale: SpielIDs anpassen

**Gruppen-Rendering (`renderGruppen`):**
- Gruppen-Array von `['A','B','C','D','E','F']` auf `['A','B','C','D']` ändern
- Qualifikationsanzeige: Platz 1-4 = blau (Achtelfinale direkt), Platz 5 = keine Markierung
- "Bester Dritter"-Legende entfernen

**KO-Auflösung (`resolveKOTeams`):**
- "Beste Dritte"-Logik komplett entfernen
- Neue Mapping-Logik: Platz 1-4 jeder Gruppe direkt ins Achtelfinale
- `resolveMap` anpassen für `Platz X.Y` Format (z.B. "1.A", "4.B")

**Gruppen-Berechnung (`getQualifiedThirdPlaced`):**
- Diese Funktion komplett entfernen (nicht mehr benötigt)

**Regeln-Sektion:**
- Modus-Text aktualisieren: "Vorrunde in 4 Gruppen à 5 Teams. Die Plätze 1-4 jeder Gruppe erreichen das Achtelfinale."

## Open Questions

> [!IMPORTANT]
> **Achtelfinale-Kreuzsetzung:** Sollen die Paarungen wie oben vorgeschlagen sein (1.A vs 4.B, 1.B vs 4.A, etc.) oder gibt es ein anderes gewünschtes Paarungsschema?

> [!IMPORTANT]
> **Uhrzeiten:** Die aktuellen Uhrzeiten im Spreadsheet beginnen um 10:00 mit 15min-Intervallen. Bei 40 Gruppenspielen (20 Slots à 2 Plätze) und 15min-Takt würde die Gruppenphase von 10:00 bis 14:45 dauern. Sollen die gleichen Zeitintervalle beibehalten werden?

> [!IMPORTANT]
> **Spreadsheet-Update:** Das Spreadsheet enthält bereits echte Ergebnisse (beendet/läuft). Die bestehenden Mannschaftsnamen passen nicht zum neuen 4×5 Format. Soll ich die Mannschaften neu auf 4 Gruppen aufteilen? Oder wirst du das manuell im Google Sheet machen und ich passe nur die Applikation an?

## Verifikation

### Automatisierte Tests
- Öffne die Anwendung im Browser und prüfe:
  - Alle 4 Gruppen werden korrekt angezeigt (je 5 Teams)
  - Spielplan zeigt 40 Gruppenspiele + 15 KO-Spiele
  - Achtelfinale-Paarungen werden korrekt aufgelöst
  - Gruppenlegende zeigt keine "Bester Dritter"-Info mehr

### Manuelle Verifikation
- Spreadsheet-Struktur visuell prüfen
- KO-Bracket-Logik durchspielen
