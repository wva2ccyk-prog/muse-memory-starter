# Muse Memory Starter

Memory management starter kit for Muse. Read why and how to use it first.

**TL;DR**: Muse's default memory rots within weeks — logs pile up (~1MB/month), MEMORY.md bloats, lossy compaction deletes important rules. This is the minimal user-side defense until Meta fixes it system-side: a 1KB router, pointer-based docs, and a weekly diet.

## Why

One month on defaults:

- **Daily logs**: 30-40KB/day → ~1MB/month. Nothing is deleted by default.
- **MEMORY.md**: background memory jobs keep writing to it. Without curation it bloats to tens of KB (measured 85KB on OpenClaw).
- **System areas** (`memory/bank/`, etc.): empty them and background refills them. Users can't touch them.
- **Compaction is lossy**: triggers at 150K tokens; important rules get dropped during summarization. Hit twice in practice (lost scan-failure reporting rule, false completion claim).
- **Vicious cycle**: always-loaded baseline grows → per-turn token cost rises → compaction fires more often → summaries pile up longer.

What users can touch is ~MEMORY.md editing. System crons, feed pulses, bank/ need Meta to add memory budgets, TTLs, and visible controls system-side. Until then, this structure is the minimum defense.

Principles:

- **One router** always loaded. Keep under ~2KB.
- Detail docs behind `doc:<NAME>` pointers; read only when needed.
- Raw conversation logs are cost, not asset. Curate what matters into handoffs, state, and rule docs.
- Separate completed work from current state. Discard old stuff periodically.

## How

1. Copy this repo's `docs/` into your Muse workspace.
2. Fill `MEMORY.md` (the always-loaded file) from the `docs/MEMORY_ROUTER.md` template. Change only `<assistant-name>` and language/style to yours.
3. Record connected services/servers in `docs/CONNECTIONS.md` (locations, not values — never commit keys/tokens).
4. Record recurring watch selection criteria in `docs/WATCHES.md`.
5. Record current-work snapshot in `docs/STATE.md`, reusable lessons in `docs/MEMORY_LEDGER.md`.
6. Schedule a **weekly diet**:
   - Move daily logs older than N days to recoverable trash
   - Clean completed/stale items from `STATE.md` (no per-item approval once delegated; log what was removed)
   - Verify all router pointers resolve and registered paths exist
   - If core docs exceed budget (e.g. 30KB total), propose deletion/archive candidates (never auto-delete rules)
   - Never auto-edit router, map, ledger, or connection/watch specs. Rule changes need approval.

## File layout

| File | Role |
|---|---|
| `docs/MEMORY_ROUTER.md` | Always-loaded router template (~1KB) |
| `docs/RETRIEVAL_MAP.md` | `doc:` pointer resolver + maintenance rules |
| `docs/CONNECTIONS.md` | Connections/servers/plugins state template |
| `docs/WATCHES.md` | Watch job spec template |
| `docs/STATE.md` | Current-work snapshot template |
| `docs/MEMORY_LEDGER.md` | Reusable lessons template |
| `docs/LOG.md` | Work record template (append-only) |

## Notes

- Never commit personal data, keys, or tokens to this repo. Structure only.
- On a new Muse app, start by filling the placeholders in step 2.
