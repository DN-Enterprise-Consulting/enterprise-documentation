# M8.5 – File-Based Identity Persistence

## Ziel

Konkrete Dateisystem-Implementierung des in M8.4 definierten
`IdentityPersistence`-Vertrags.

## Implementierung

`FileIdentityPersistence`

Package:

`de.dn.enterprise.platform.identity.persistence`

Technische Fehler werden als `IdentityPersistenceException` gekapselt.

## Dateiformat

Pro Identity wird eine Datei mit der Endung `.identity` verwendet.

Die Datei enthält fünf UTF-8-Zeilen:

1. Identity-ID
2. Identity-Type
3. Identity-Status
4. Metadata-Name
5. Metadata-Description

Freitextwerte werden Base64-kodiert gespeichert. Dadurch bleiben Zeilenumbrüche,
Unicode-Zeichen und sonstige freie Textinhalte vom zeilenorientierten Format
getrennt.

## Verhalten

- `save()` legt das Storage-Verzeichnis bei Bedarf an.
- `save()` ersetzt eine vorhandene Identity mit gleicher ID.
- `load()` liefert `null`, wenn keine Datei existiert.
- `exists()` prüft das Vorhandensein der Datei.
- `delete()` ist für nicht vorhandene Dateien ein No-Op.
- Ungültige Dateien führen zu `IdentityPersistenceException`.
- Null-Argumente werden abgelehnt.

## Abgrenzung

M8.5 implementiert ausschließlich Persistence. Repository-Integration,
Service-Layer und Runtime-Komposition folgen in späteren Milestones.
