# M8.2 – Identity Repository Contract

## Ziel

Definition eines technologieunabhängigen Repository-SPI für die Identity-Domain.

## Vertrag

`IdentityRepository` definiert:

- `save(Identity)`
- `findById(IdentityId)`
- `findAll()`
- `findByType(IdentityType)`
- `findByStatus(IdentityStatus)`
- `existsById(IdentityId)`

## Architektur

Der Vertrag liegt im Identity-Modul unter `identity.spi`.

Es gibt in M8.2 bewusst keine konkrete Persistence-Technologie und keine Implementierung. Die Domain bleibt von Datenbank-, Datei- oder Framework-Technologien entkoppelt.

## Verhalten

Die konkreten Semantiken der Repository-Implementierung werden in den folgenden M8-Schritten festgelegt. M8.2 definiert ausschließlich die technische Schnittstelle.

## Nächster Schritt

M8.3 – In-Memory Identity Repository.
