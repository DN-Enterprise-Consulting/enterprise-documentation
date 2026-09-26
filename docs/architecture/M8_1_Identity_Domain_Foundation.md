# M8.1 – Identity Domain Foundation

## Ziel

Aufbau der frameworkfreien Domain-Grundlage für Identity & Access.

## Bestandteile

- `IdentityId` – stabiler UUID-basierter Identity-Identifier
- `IdentityType` – `USER` / `SERVICE`
- `IdentityStatus` – `ACTIVE` / `LOCKED` / `DISABLED`
- `IdentityMetadata` – unveränderliche beschreibende Metadaten
- `Identity` – immutable Identity-Aggregat

## Architekturgrenzen

M8.1 enthält bewusst noch keine:

- Passwörter oder Credentials
- Token-/Session-Logik
- Authentifizierungsprotokolle
- Rollen-/Permission-Policies
- externen Identity Provider
- Persistenz

Diese Themen werden in nachfolgenden M8-Schritten über explizite Domain-, SPI- und Application-Grenzen ergänzt.

## Invarianten

- IDs sind niemals `null`.
- Type, Status und Metadata sind niemals `null`.
- Identity-Namen dürfen nicht leer sein.
- Domain-Objekte sind immutable.
- Status- und Metadatenänderungen erzeugen neue Identity-Werte.

## Definition of Done

- Java 21 kompatibel
- frameworkfreie Domain
- Unit-Tests vorhanden
- keine Abhängigkeit auf andere Fachdomänen
- vollständiger Maven-Reactor-Build muss grün sein
