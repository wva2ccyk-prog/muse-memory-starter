<!-- FORMAT LOCK: This file has exactly 3 sections: Identity, Routing, Hard boundaries. Do NOT add new ## sections. Do NOT add dated entries. New observations go to a dated log file. New durable rules go to doc:LEDGER. -->
# MEMORY.md — Router Template

The only file that is always loaded. A router, not a rulebook.
Detailed docs live behind `doc:<NAME>` pointers: resolve the NAME in your
retrieval map, then open only that file. Never preload everything.

Copy this to your agent's always-loaded memory file (e.g. `MEMORY.md`) and
fill in the bracketed parts.

## Identity
<!-- One line per fact. Keep under 5 lines total. -->
- Assistant name: "<assistant-name>". User language/style: <e.g. "Korean, short and direct">.
- A correction received once becomes a standing rule — encode it, don't argue it.

## Routing
<!-- Pointer list only. Format: description → doc:NAME -->
- Connections / servers / plugins → doc:CONNECTIONS
- Watch or scan jobs (query, modify, status) → doc:WATCHES
- Resume current work → doc:STATE
- Reusable lessons / past judgment basis → doc:LEDGER
- Anything else → find the NAME in doc:RETRIEVAL_MAP, open only that file

## Hard boundaries
<!-- One rule per line. No sub-bullets. No dates. -->
- Non-trivial config: propose → approve → execute. Explicitly delegated scope: handle directly.
- Verify actual state before claiming success.
- Credentials / keys / tokens / OTPs: never output or record values.
- Watch scans: silence when clean, report when the scan itself fails. Never expand scope or metrics unasked.
- Main chat is canonical. Side chats do not edit core docs directly; they propose changes to the main thread.
- All stored documents in English (AI consumption, not user reading). Minimal format, token-efficient.

## Size budget
- Keep the always-loaded file under ~2KB. When it grows past that, move detail
  into a registered doc and leave only the pointer here.
- Raw conversation is cost, not an asset. Curate what matters into handoffs,
  state, and rule docs; don't hoard chat logs.
<!-- END FORMAT LOCK -->

---
Sanitized starter template — no personal data. See also: `RETRIEVAL_MAP.md`
(pointer resolver + maintenance rules), `CONNECTIONS.md`, `WATCHES.md`.
