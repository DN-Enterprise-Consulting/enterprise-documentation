# M8.6 – Persistent Identity Repository

## Ziel

Integration der bestehenden `IdentityRepository`-Abstraktion mit der
in M8.5 implementierten dateibasierten Persistence.

## Architektur

`IdentityRepository`
↓
`FileIdentityRepository`
↓
`FileIdentityPersistence`
↓
Filesystem

Das Repository kennt keine anderen Persistenztechnologien.

## Verhalten

- `save()` persistiert die Identity und gibt das gespeicherte Objekt zurück.
- `findById()` lädt eine Identity direkt über ihre ID.
- `findAll()` lädt alle `.identity`-Dateien und liefert sie deterministisch nach ID sortiert.
- `findByType()` filtert die persistierten Objekte nach `IdentityType`.
- `findByStatus()` filtert nach `IdentityStatus`.
- `existsById()` delegiert an die Persistence.
- fehlende IDs liefern bei `findById()` `null`.
- Null-Argumente werden abgelehnt.

## Restart-Verhalten

Repository-Instanzen können verworfen und mit derselben Storage-Directory
neu aufgebaut werden. Persistierte Identities bleiben dabei verfügbar.

## Technische Abgrenzung

Der bestehende `IdentityPersistence`-SPI bleibt unverändert. Die konkrete
Datei-Persistence stellt für den Repository-Bulkzugriff zusätzlich
`loadAll()` bereit. Die Integration ist damit bewusst auf die bestehende
File-Implementierung begrenzt.

## Nächster Schritt

M8.7 – Identity Service & Lifecycle.
