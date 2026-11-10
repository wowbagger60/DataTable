# DataTable

Deutsch | [English](README.md)

Modul für die IP-Symcon Kachel-Visualisierung von Ueli Schweizer / Cassanis Automation.
Präfix: `DATAT`.

DataTable verwaltet einen kleinen strukturierten Datenbestand, zum Beispiel
Infotexte, Mitarbeitende oder Absenzen. Die Felder werden in der
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
| Reiter | Nur Datenfelder: Felder mit demselben Reiter werden zusammen angezeigt. |
| Trennlinie oberhalb | Nur Datenfelder: Linie über dem Feld |
| In Liste | Das Feld ist eine Spalte der Liste. Ohne Listenfeld werden die Schlüsselfelder gezeigt. |
| Spaltentitel in der Liste | Kürzerer Titel für die Listenspalte. Leer: die Beschriftung. |

### Erweiterte Einstellungen

Der Bearbeiten-Dialog zeigt zuerst die allgemeinen Einstellungen. «Erweiterte
Einstellungen anzeigen» oder ein Wechsel des Typs blendet die Einstellungen
ein, die zum Typ passen:

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
zum Beispiel `{lastname} {firstname} ({pnr})`. Unbekannte Platzhalter
erscheinen als Text, ein Tippfehler bleibt also sichtbar. Leer: die Werte der
Schlüsselfelder, getrennt durch « · ».

**Sortierung:** Feldnamen durch Kommas getrennt, z. B. `chapter, order, tag`.
Ein `-` vor dem Namen sortiert absteigend: `-from, employee`. Es sind alle
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

- Suche in den Listenspalten und in allen Textfeldern.
- Filter nach dem Filterfeld. «Neu» übernimmt den gewählten Filterwert.
- Neu, Kopieren, Löschen; vorheriger und nächster Datensatz innerhalb der
  aktuellen Liste.
- Speichern mit «Speichern» oder Ctrl+S / Cmd+S. Ungespeicherte Eingaben
  werden nicht ohne Rückfrage verworfen.

Mehrere Clients können gleichzeitig bearbeiten. Jeder gespeicherte Datensatz
hat eine Revision. Wurde ein Datensatz inzwischen von einem anderen Client
geändert oder gelöscht, wird nicht stillschweigend gespeichert: Die Kachel
bietet an, neu zu laden, zu überschreiben oder den Datensatz neu anzulegen.

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

```php
$records = DATAT_Export($id);
```

Liefert alle Datensätze als Array. Der Array-Schlüssel ist der interne
Schlüssel des Datensatzes; jeder Datensatz enthält die Werte nach Feldname. Die
Werte haben den Typ des Feldes; ein Datum ist ein String `YYYY-MM-DD`, leere
Zahlen sind `null`.

Der Export liefert immer alle Datensätze, unabhängig von einem Filter in der
Kachel.

## 8. Grenzen

Die Datensätze liegen als ein JSON-Wert in der Instanz; jedes Speichern
schreibt ihn vollständig, und die Kachel erhält die ganze Tabelle. Mehrere
tausend Datensätze sind problemlos; die praktische Grenze hängt von der Länge
der Datensätze und vom Gerät ab, auf dem die Kachel läuft. DataTable ist kein
Ersatz für eine Datenbank.

## 9. Unterstützung

Dieses Modul ist kostenlos. Schenkungen als Unterstützung für den Autor werden
gerne angenommen: PayPal an ueli.schweizer@cassanis.fr

## 10. Änderungen

**1.2 Build 1:** Erstveröffentlichung als DataTable, Nachfolger von RecordManager.
