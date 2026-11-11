# DataTable

Deutsch | [English](README.md)

Modul für die IP-Symcon Kachel-Visualisierung von Ueli Schweizer / Cassanis Automation.
Präfix: `DATAT`.

DataTable verwaltet einen kleinen strukturierten Datenbestand, zum Beispiel
Kundendaten, eine Geburtstagsliste oder ein Fahrtenbuch. Die Felder werden in der
Instanzkonfiguration definiert, die Datensätze in einer HTML-Kachel bearbeitet.
Mehrere Clients können gleichzeitig arbeiten.

## Inhaltsverzeichnis

1. [Voraussetzungen](#1-voraussetzungen)
2. [Installation und erster Test](#2-installation-und-erster-test)
3. [Felder](#3-felder)
4. [Datensatz-Titel und Sortierung](#4-datensatz-titel-und-sortierung)
5. [Kachel](#5-kachel)
6. [Felddefinition ändern](#6-felddefinition-ändern)
7. [Scriptfunktionen](#7-scriptfunktionen)
8. [Grenzen](#8-grenzen)
9. [Unterstützung](#9-unterstützung)
10. [Änderungen](#10-änderungen)

## 1. Voraussetzungen

- IP-Symcon ab Version 8.1.
- DataTable wird als normale Kachel angezeigt. Ab Version 9.1 kann es
  zusätzlich im Vollbild-Modus angezeigt werden.

## 2. Installation und erster Test

1. Bibliothek über den Module Store oder das Module Control installieren und
   eine DataTable-Instanz anlegen.
2. Unter «Schlüsselfelder» mindestens ein Feld anlegen, z. B. Name `code`,
   Typ String.
3. Unter «Datenfelder» weitere Felder anlegen, z. B. `text`, Typ String,
   Darstellung mehrzeilig.
4. Übernehmen. Instanz in der Visualisierung als Kachel anzeigen.
5. Mit «Neu» einen Datensatz anlegen und speichern.

## 3. Felder

Die Konfiguration hat zwei Listen:

- **Schlüsselfelder** identifizieren einen Datensatz. Ihre Reihenfolge ist die
  Reihenfolge im Schlüssel; zwei Datensätze können nicht denselben Schlüssel
  haben.
- **Datenfelder** enthalten die übrigen Werte.

Jedes Feld hat eine feste ID (`f1`, `f2`, …), die automatisch vergeben wird und
nicht änderbar ist. Die Datensätze speichern ihre Werte unter dieser ID; ein
Feld lässt sich deshalb umbenennen, ohne dass Daten verloren gehen.

### Allgemeine Einstellungen

| Einstellung | Bedeutung |
|---|---|
| Name | Technischer Name für Datensatz-Titel, Sortierung und Scriptfunktionen. Beginnt mit einem Buchstaben; erlaubt sind Buchstaben, Ziffern, `_` und `-`. Muss eindeutig sein. |
| Typ | String, Integer, Float oder Boolean |
| Pflicht | Nur Datenfelder: Das Feld darf nicht leer sein. Schlüsselfelder sind immer Pflicht. |
| Standardwert | Wert eines neuen Datensatzes. Boolean: `true`/`false` (auch `1`/`0`, `ja`/`nein`, `on`/`off`). |
| Beschriftung | Text in der Kachel. Leer: der Name. |
| Eingabemaske | Nur Datenfelder: «Editierbar», «Nur lesen» oder «Versteckt». Nur-lesen- und versteckte Felder sind für Werte gedacht, die ein Script setzt, z. B. Statistiken oder generierte Werte; die Kachel zeigt ein Nur-lesen-Feld an, ändert es aber nicht, ein verstecktes Feld erscheint nicht im Datensatz (als Listenspalte ist es weiterhin möglich). Beim Speichern aus der Kachel bleibt ihr gespeicherter Wert erhalten; neue Datensätze aus der Kachel erhalten den Standardwert, «Pflicht» wird dort nicht geprüft. |
| Reiter | Nur Datenfelder: Felder mit demselben Reiter werden zusammen angezeigt. |
| Trennlinie oberhalb | Nur Datenfelder: Linie über dem Feld |
| In Liste | Das Feld ist eine Spalte der Liste. Ohne Listenfeld werden die Schlüsselfelder gezeigt. |
| Spaltentitel in der Liste | Kürzerer Titel für die Listenspalte. Leer: die Beschriftung. |

### Typabhängige Einstellungen

Der Bearbeiten-Dialog zeigt nur die Einstellungen, die zum Typ des Feldes
passen; ein Wechsel von Typ, Darstellung oder Optionen im Dialog passt sie an:

| Einstellung | Gilt für |
|---|---|
| Darstellung | String: einzeilig, mehrzeilig oder Datum |
| Zeilen | Datenfelder, String mehrzeilig: anfängliche Höhe |
| Optionen | String (einzeilig), Integer, Boolean: macht das Feld zur Auswahl |
| Minimum, Maximum | Float, Integer ohne Optionen |
| Nachkommastellen | Float |
| Präfix, Suffix | Integer, Float, z. B. `CHF` oder `%` |
| Schlüssel unterscheidet Gross/Klein | Schlüsselfelder vom Typ String |
| Filter in der Liste | Felder mit Optionen und Boolean-Felder |

Optionen werden als `Wert=Bezeichnung; Wert2=Bezeichnung 2` oder eine pro Zeile
erfasst. Ohne `=` sind Wert und Bezeichnung gleich. Die Werte müssen zum Typ
passen, z. B. `1=Tief; 2=Hoch` bei einem Integer oder `true=Fertig; false=Offen`
bei einem Boolean.

Nur ein Feld kann als Filter dienen. Boolean-Felder ohne Optionen werden nach
Ja/Nein gefiltert.

String-Schlüssel unterscheiden standardmässig nicht zwischen Gross- und
Kleinschreibung: `Paris` und `paris` sind derselbe Schlüssel.

Um ein Feld zwischen Schlüssel- und Datenfeldern zu verschieben, in der einen
Liste löschen und in der anderen mit demselben Namen neu anlegen. Seine Werte
bleiben erhalten.

## 4. Datensatz-Titel und Sortierung

**Datensatz-Titel:** der Titel eines Datensatzes in der Kachel, in der
Löschbestätigung und in «Kopie von …». Felder werden mit `{name}` eingefügt,
zum Beispiel `{lastname} {firstname} ({customer-no})`. Unbekannte Platzhalter
erscheinen als Text, ein Tippfehler bleibt also sichtbar. Leer: die Werte der
Schlüsselfelder, getrennt durch « · ».

**Sortierung:** die Reihenfolge der Liste in der Kachel; sie wirkt sich nicht
auf die gespeicherten Daten oder die Scriptfunktionen aus. Feldnamen durch
Kommas getrennt, z. B. `city, lastname`. Ein `-` vor dem Namen sortiert
absteigend: `-date, vehicle`. Es sind alle
Felder erlaubt. Nicht aufgeführte Schlüsselfelder werden aufsteigend angehängt,
die Reihenfolge ist also immer eindeutig. Leer: die Schlüsselfelder,
aufsteigend. Ein unbekannter Name setzt die Instanz in den Fehlerstatus.

In der Kachel sortiert ein Klick auf eine Spaltenüberschrift nach dieser Spalte,
bis die Kachel neu geladen wird.

## 5. Kachel

Die Darstellung richtet sich nach der Grösse der Kachel:

| Grösse | Anzeige |
|---|---|
| Gross | Liste und Datensatz nebeneinander |
| Mittel | Liste oder Datensatz, mit «‹» zurück zur Liste |
| Klein | Titel, Anzahl Datensätze und, falls ein Filterfeld besteht, die Anzahl pro Wert |

Funktionen:

- Suche in den Listenspalten und in allen nicht versteckten Textfeldern.
- Die Liste zeigt zuerst 200 Zeilen; «200 weitere anzeigen» ergänzt weitere.
- Filter nach dem Filterfeld. «Neu» übernimmt den gewählten Filterwert.
- Neu, Kopieren, Löschen; vorheriger und nächster Datensatz innerhalb der
  aktuellen Liste.
- Speichern mit «Speichern» oder Ctrl+S / Cmd+S. Ungespeicherte Eingaben
  werden nicht ohne Rückfrage verworfen.

Mehrere Clients können gleichzeitig bearbeiten. Jeder gespeicherte Datensatz
hat eine Revision. Wurde ein Datensatz inzwischen von einem anderen Client
geändert oder gelöscht, wird nicht stillschweigend gespeichert: Die Kachel
bietet an, neu zu laden, zu überschreiben oder den Datensatz neu anzulegen.
Ändert ein Script nur Nur-lesen- oder versteckte Felder, gilt das nicht als
Konflikt: Die Kachel übernimmt die neuen Werte, und die Eingaben in den
editierbaren Feldern bleiben erhalten.

Die Kachel übernimmt die Farben der Visualisierung, auch im Dunkelmodus. Die
Texte folgen der Spracheinstellung von IP-Symcon (Deutsch, Englisch). Zahlen
und Datum erscheinen im Format des Systems, auf dem die Kachel angezeigt wird.
Die Bezeichnungen der Optionen sind Benutzerdaten und erscheinen wie erfasst.

## 6. Felddefinition ändern

Beim Übernehmen der Felddefinition werden die bestehenden Datensätze
umgewandelt:

- Ein Typwechsel wandelt die Werte um, z. B. Integer in String.
- Neue Felder erhalten den Standardwert.

Die Änderung wird abgelehnt, wenn sich ein Wert nicht umwandeln lässt, ein
Schlüsselfeld leer wäre oder zwei Datensätze denselben Schlüssel erhielten. Die
Instanz zeigt dann den Status 203 und den Grund oben im
Konfigurationsformular; die Daten bleiben unverändert. Die Definition
korrigieren, z. B. mit einem Standardwert für ein neues Schlüsselfeld.

| Status | Bedeutung |
|---|---|
| 102 | Aktiv |
| 201 | Felddefinition ist ungültig (der Grund steht im Formular) |
| 202 | Noch keine Felder definiert |
| 203 | Daten passen nicht zur neuen Felddefinition |

## 7. Scriptfunktionen

Alle Funktionen liefern ein Array mit mindestens `Status` und `Message`.
`Status === 0` bedeutet Erfolg, sonst nennt `Message` den Grund. Felder werden
über den Namen angesprochen (Gross-/Kleinschreibung spielt keine Rolle). Die
Werte werden wie in der Kachel geprüft und umgewandelt: Typ, Pflicht,
Minimum/Maximum und Optionen.

```php
$r = DATAT_AddRecord($id, ['customer-no' => 1042, 'name' => 'Muster', 'city' => 'ZH']);
$key = $r['Key'];

$r = DATAT_GetRecord($id, ['customer-no' => 1042]);
if ($r['Status'] === 0) {
    $record = $r['Record'];          // ['customer-no' => 1042, 'name' => 'Muster', ...]
}

$r = DATAT_ModifyRecord($id, ['customer-no' => 1042, 'city' => 'BE']);
$r = DATAT_ModifyRecord($id, ['customer-no' => 1043], $key);   // Schlüssel ändern
$r = DATAT_DeleteRecord($id, ['customer-no' => 1043]);
```

| Funktion | Ergebnis |
|---|---|
| `DATAT_GetRecord($id, $KeyValues)` | `Key`, `Record` (`false`, wenn nicht gefunden) |
| `DATAT_AddRecord($id, $Record)` | `Key`. Nicht übergebene Felder erhalten den Standardwert. |
| `DATAT_ModifyRecord($id, $Changes, $OriginalKey = '')` | `Key` (der neue). Nur die übergebenen Felder ändern sich. |
| `DATAT_DeleteRecord($id, $KeyValues)` | – |
| `DATAT_Export($id)` | `Records`: interner Schlüssel => Datensatz |
| `DATAT_Import($id, $Records)` | `Added`, `Updated`, `Unchanged` |

`ModifyRecord()` findet den Datensatz über `$OriginalKey`, den internen
Schlüssel aus `Key`, oder, wenn dieser leer ist, über die Schlüsselfelder in
`$Changes`. Mit `$OriginalKey` lassen sich auch Schlüsselfelder ändern.

`Export()` liefert immer alle Datensätze, unabhängig von einem Filter in der
Kachel. Die Werte haben den Typ des Feldes; ein Datum ist ein String
`YYYY-MM-DD`, leere Zahlen sind `null`.

`Import()` ist ein atomarer Merge: Neue Datensätze werden ergänzt, bestehende
mit gleichem Schlüssel erhalten die übergebenen Felder, identische bleiben
unverändert. Nicht enthaltene Datensätze werden nie gelöscht. Ist ein Datensatz
ungültig, wird nichts importiert, und `Message` nennt den Datensatz. Die
`Records` von `DATAT_Export()` lassen sich direkt importieren, sofern die
Feldnamen übereinstimmen.

Jede Änderung erscheint sofort in allen offenen Kacheln.

| Status | Bedeutung |
|---:|---|
| 0 | OK |
| 1 | Ungültige Konfiguration |
| 2 | Schlüsselfeld fehlt oder ist leer |
| 3 | Ungültiger Wert (Typ, Bereich oder Option) |
| 4 | Datensatz nicht gefunden |
| 5 | Schlüssel bereits vorhanden |
| 6 | Unbekanntes Feld |
| 7 | Pflichtfeld leer |
| 10 | Datenspeicher blockiert |
| 12 | Ungültige Importdaten |
| 13 | Doppelter Schlüssel im Import |

Die Integer-Variable `DataRevision` unterhalb der Instanz wird um eins erhöht,
sobald sich die gespeicherten Daten ändern, sei es über die Kachel, ein Script
oder eine Änderung der Felddefinition. Ein Ereignis auf diese Variable reagiert
auf jede Änderung.

## 8. Grenzen

Die Datensätze liegen als ein JSON-Wert in der Instanz; jedes Speichern
schreibt ihn vollständig, und die Kachel erhält die ganze Tabelle (in Teilen,
weil Symcon eine einzelne Kachelmeldung auf 1 MB begrenzt). Mehrere
tausend Datensätze sind problemlos; die praktische Grenze hängt von der Länge
der Datensätze und vom Gerät ab, auf dem die Kachel läuft. DataTable ist kein
Ersatz für eine Datenbank.

## 9. Unterstützung

Dieses Modul ist kostenlos. Schenkungen als Unterstützung für den Autor werden
gerne angenommen: PayPal an ueli.schweizer@cassanis.fr

## 10. Änderungen

**1.2 Build 2:** Erstveröffentlichung.
