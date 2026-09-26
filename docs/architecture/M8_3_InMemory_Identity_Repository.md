# M8.3 – In-Memory Identity Repository

## Ziel

Konkrete, technologieunabhängige In-Memory-Implementierung des in M8.2 definierten `IdentityRepository`.

## Eigenschaften

- `ConcurrentHashMap` als Speicher
- thread-safe Repository-Zugriff
- `save()` gibt die gespeicherte Identity zurück
- erneutes Speichern derselben `IdentityId` ersetzt den bestehenden Eintrag
- `findById()` liefert `null`, wenn kein Eintrag vorhanden ist
- `findAll()`, `findByType()` und `findByStatus()` liefern deterministisch nach `IdentityId` sortierte Ergebnisse
- Null-Eingaben werden explizit abgelehnt

## Architektur

Die Implementierung liegt unter:

`de.dn.enterprise.platform.identity.inmemory`

Sie hängt ausschließlich vom Identity-Domainmodell und dem Identity-SPI ab. Es wird keine externe Persistence-Technologie eingeführt.

## Nächster Schritt

M8.4 – Identity Persistence Contract.
