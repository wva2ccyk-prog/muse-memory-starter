# Connections — Template

Operational details for connected services, servers, and plugins.
The router points here via `doc:CONNECTIONS`. Open only when doing
connection, server, or plugin work.

This file holds **state** (things that change), not rules. Usage instructions
belong in Skills; this file records what is connected and where credentials
live — **never the credential values themselves**.

## <Service name>

- What: <one line — what this connection is for>
- Endpoint: <host or URL — no secrets>
- Auth: <where the credential lives, e.g. "Secure Vault entry '<name>'" — never the value>
- Scope / limits: <e.g. rate limits, cost per call, allowed operations>
- Notes: <quirks — e.g. "use only when the user explicitly asks">

## <Another service>

- ...

---
Sanitized starter template. Fill in your own services; never commit secrets,
keys, tokens, or personal identifiers.
