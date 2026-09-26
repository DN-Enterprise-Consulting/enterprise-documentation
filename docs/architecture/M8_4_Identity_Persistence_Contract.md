# M8.4 – Identity Persistence Contract

## Ziel

Definition eines technologieunabhängigen Persistence-SPI für die Identity-Domain.

## Vertrag

`IdentityPersistence` definiert:

- `void save(Identity identity)`
- `Identity load(IdentityId id)`
- `boolean exists(IdentityId id)`
- `void delete(IdentityId id)`

## Architektur

Der Vertrag liegt unter:

`de.dn.enterprise.platform.identity.spi`

Er enthält keine konkrete Persistence-Technologie. Insbesondere werden in M8.4 keine Datei-, Datenbank-, ORM- oder Framework-Abhängigkeiten eingeführt.

## Semantik

Die konkrete Implementierung entscheidet über den physischen Speicher.

Der Persistence-Vertrag ist bewusst vom Repository-Vertrag getrennt:

- `IdentityRepository` definiert fachlich orientierten Repository-Zugriff inklusive Abfragen.
- `IdentityPersistence` definiert den minimalen Persistenz-Lebenszyklus eines einzelnen Identity-Objekts.

`load()` darf bei nicht vorhandenem Datensatz `null` liefern. Die konkrete Fehlerbehandlung bei technischen Storage-Fehlern wird durch die jeweilige Implementierung festgelegt.

## Test

Der Contract-Test prüft ausschließlich die öffentliche SPI-Signatur und führt keine konkrete Persistence-Technologie ein.

## Nächster Schritt

M8.5 – File-Based Identity Persistence.
