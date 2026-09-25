# Retrieval Map

Single resolver for active operating docs under `docs/`. Cross-doc pointers use
`doc:<NAME>`; resolve the NAME here, then open only that file. Register each new
active Markdown doc under `docs/` with one row.

The always-loaded entry point is the router — see `MEMORY_ROUTER.md`. This file
is opened on demand, not preloaded.

## Resolver Table

CONNECTIONS|docs/CONNECTIONS.md|connection/server/plugin details (state, not rules)
WATCHES|docs/WATCHES.md|watch job specs: scope, criteria, failure handling
STATE|docs/STATE.md|current-work snapshot; resume context
LEDGER|docs/MEMORY_LEDGER.md|reusable lessons only
LOG|docs/LOG.md|append-only work record
RETRIEVAL_MAP|docs/RETRIEVAL_MAP.md|this resolver

## Rules For This File

- Pointer rows only. Never paste content, history, or summaries here.
- One NAME → one file. No duplicate names, no duplicate paths.
- When a doc moves, change only its row; other docs keep the same NAME token.

## Maintenance Rules

- **Diet (fixed cadence, e.g. weekly):**
  1. Move daily logs older than N days to trash with a recoverable window (e.g. 30 days).
  2. Clean completed/stale items out of STATE. Once the user delegates this, no
     per-item approval is needed — log what was removed.
  3. If core active docs exceed a size budget (e.g. 30KB total), propose
     deletion/archive candidates (max ~10) — never auto-delete rules.
  4. Report storage regrowth in system-owned dirs and propose cleanup.
  5. Never auto-edit the router, this map, the ledger, or connection/watch specs.
     Rule changes always need approval.
- **Pointer hygiene:** every pointer in the router must resolve here; every path
  registered here must exist. Check on each diet run.
- **Canonical thread:** the main chat is canonical. Side/parallel chats do not
  edit core docs directly; they propose changes to the main thread.
- **Active vs. archive:** keep the active set small. Completed history and raw
  logs live outside it — searchable, not preloaded.
