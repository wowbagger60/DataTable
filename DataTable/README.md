# DataTable

[Deutsch](README.de.md) | English

Module for the IP-Symcon tile visualization by Ueli Schweizer / Cassanis Automation.
Prefix: `DATAT`.

DataTable manages a small structured data set, for example info texts,
employees or absences. The fields are defined in the instance configuration;
the records are edited in an HTML tile. Several clients can work at the same
time.

## Table of contents

1. [Requirements](#1-requirements)
2. [Installation and first test](#2-installation-and-first-test)
3. [Fields](#3-fields)
4. [Record title and sort order](#4-record-title-and-sort-order)
5. [Tile](#5-tile)
6. [Changing the field definition](#6-changing-the-field-definition)
7. [Script functions](#7-script-functions)
8. [Limits](#8-limits)
9. [Support](#9-support)
10. [Changelog](#10-changelog)

## 1. Requirements

- IP-Symcon 8.1 or later.
- DataTable is displayed as a regular tile. From version 9.1, it can also be
  shown in fullscreen mode.

## 2. Installation and first test

1. Install the library via the Module Store or Module Control and create a
   DataTable instance.
2. Under «Key fields», add at least one field, e.g. name `code`, type String.
3. Under «Data fields», add further fields, e.g. `text`, type String, display
   multi-line.
4. Apply. Show the instance as a tile in the visualization.
5. Create a record with «New» and save it.

## 3. Fields

The configuration has two lists:

- **Key fields** identify a record. Their order is the order in the key; two
  records cannot have the same key.
- **Data fields** hold the remaining values.

Each field has a stable ID (`f1`, `f2`, …) that is assigned automatically and
cannot be changed. Records store their values under this ID, so a field can be
renamed without losing data.

### General settings

| Setting | Meaning |
|---|---|
| Name | Technical name for the record title, the sort order and the script functions. Starts with a letter; letters, digits, `_` and `-` are allowed. Must be unique. |
| Type | String, Integer, Float or Boolean |
| Required | Data fields only: the field must not be empty. Key fields are always required. |
| Default value | Value of a new record. Boolean: `true`/`false` (also `1`/`0`, `yes`/`no`, `on`/`off`). |
| Label | Text in the tile. Empty: the name. |
| Tab | Data fields only: fields with the same tab are shown together. |
| Separator above | Data fields only: line above the field |
| In list | The field is a column in the list. Without any list field, the key fields are shown. |
| Column title in list | Shorter title for the list column. Empty: the label. |

### Advanced settings

The edit dialog first shows the general settings. «Show advanced settings» or a
change of type shows the settings that apply to the type:

| Setting | Applies to |
|---|---|
| Display | String: single line, multi-line or date |
| Lines | Data fields, multi-line String: initial height |
| Options | String (single line), Integer, Boolean: makes the field a selection |
| Minimum, maximum | Float, Integer without options |
| Decimals | Float |
| Prefix, suffix | Integer, Float, e.g. `CHF` or `%` |
| Case-sensitive key | Key fields of type String |
| Filter in list | Fields with options and Boolean fields |

Options are entered as `value=Label; value2=Label 2` or one per line. Without
`=`, value and label are the same. The values must match the type, e.g.
`1=Low; 2=High` for an Integer or `true=Done; false=Open` for a Boolean.

Only one field can be the filter. Boolean fields without options are filtered
by yes/no.

By default, String keys do not distinguish upper and lower case: `Paris` and
`paris` are the same key.

To move a field between key and data fields, delete it in one list and add it
with the same name in the other. Its values are kept.

## 4. Record title and sort order

**Record title:** the title of a record in the tile, in the delete
confirmation and in «Copy of …». Fields are inserted with `{name}`, for example
`{lastname} {firstname} ({pnr})`. Unknown placeholders are shown as text, so a
typing error is visible. Empty: the values of the key fields, separated by
« · ».

**Sort order:** field names separated by commas, e.g. `chapter, order, tag`. A
`-` before the name sorts descending: `-from, employee`. Any field can be used.
The key fields not listed are appended in ascending order, so the order is
always unique. Empty: the key fields, ascending. An unknown name puts the
instance into error state.

In the tile, a click on a column header sorts by that column until the tile is
reloaded.

## 5. Tile

The layout follows the size of the tile:

| Size | Display |
|---|---|
| Large | List and record side by side |
| Medium | List or record, with «‹» back to the list |
| Small | Title, number of records and, if a filter field exists, the number per value |

Functions:

- Search in the list columns and in all text fields.
- Filter by the filter field. «New» takes over the selected filter value.
- New, copy, delete; previous and next record within the current list.
- Save with «Save» or Ctrl+S / Cmd+S. Unsaved input is not discarded
  without confirmation.

Several clients can edit at the same time. Each saved record has a revision. If
a record has been changed or deleted by another client in the meantime,
saving is not performed silently: the tile offers to reload, overwrite or
recreate the record.

The tile uses the colors of the visualization, including dark mode. Texts
follow the IP-Symcon language (English, German). Numbers and dates are shown in
the format of the system on which the tile is displayed. Option labels are
user data and are shown as entered.

## 6. Changing the field definition

When the field definition is applied, the existing records are converted:

- A type change converts the values, e.g. Integer to String.
- New fields get the default value.

The change is rejected if a value cannot be converted, a key field would be
empty or two records would get the same key. The instance then shows status
203 and the reason at the top of the configuration form; the data stays
unchanged. Correct the definition, e.g. with a default value for a new key
field.

| Status | Meaning |
|---|---|
| 102 | Active |
| 201 | Field definition is invalid (the reason is shown in the form) |
| 202 | No fields defined yet |
| 203 | Data does not match the new field definition |

## 7. Script functions

```php
$records = DATAT_Export($id);
```

Returns all records as an array. The array key is the internal key of the
record; each record contains the values by field name. Values have the type of
the field; dates are strings `YYYY-MM-DD`, empty numbers are `null`.

The export always returns all records, regardless of a filter in the tile.

## 8. Limits

The records are stored in the instance as one JSON value; every save writes it
completely, and the tile receives the whole table. Several thousand records
work well; the practical limit depends on the length of the records and on the
device showing the tile. DataTable is not a replacement for a database.

## 9. Support

This module is free of charge. Donations to support the author are gratefully
accepted: PayPal to ueli.schweizer@cassanis.fr

## 10. Changelog

**1.2 Build 1:** Initial release as DataTable, successor of RecordManager.
