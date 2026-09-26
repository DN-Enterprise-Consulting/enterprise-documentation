# M8.7 – Identity Service & Lifecycle

## Ziel

Einführung eines persistenten Identity-Service als fachliche
Anwendungsschicht oberhalb des `IdentityRepository`.

## Architektur

`PersistentIdentityService`
↓
`IdentityRepository`
↓
`FileIdentityRepository`
↓
`FileIdentityPersistence`

Der Service enthält keine direkte Filesystem-Abhängigkeit.

## Use Cases

- `create(type, metadata)`
- `get(id)`
- `updateMetadata(id, metadata)`
- `activate(id)`
- `lock(id)`
- `disable(id)`

## Lifecycle

Der aktuelle Identity-Lifecycle verwendet die bestehenden Statuswerte:

`ACTIVE → LOCKED → ACTIVE`

`ACTIVE → DISABLED`

`LOCKED → DISABLED`

`DISABLED` ist terminal.

Ein Übergang auf denselben Status ist idempotent.

## Änderungsregeln

Metadaten können bei `ACTIVE` und `LOCKED` geändert werden.
Deaktivierte Identities sind unveränderlich.

## Fehler

- `IdentityNotFoundException` bei unbekannter ID.
- `IdentityLifecycleException` bei ungültigen Lifecycle-Übergängen
  oder Änderungen an deaktivierten Identities.

## Abgrenzung

M8.7 implementiert noch keine Authentifizierung, Autorisierung,
Credentials, Tokens, Rollen oder Security Provider.
