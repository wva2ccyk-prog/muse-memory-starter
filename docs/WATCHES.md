# Watches — Template

Specs for recurring watch / scan jobs. The router points here via `doc:WATCHES`.
Open only when querying, modifying, or checking the status of a watch.

One section per watch. The scheduler owns the cadence; this file owns the
**selection criteria** and the reporting rules. Strict criteria are the whole
point: silence when there is nothing worth reporting.

## <Watch name>

- Cadence: <e.g. "every 30 min" — the schedule lives in the scheduler, this is documentation>
- Scope: <where it scans>
- Report only: <exact criteria — be strict; "actually worth acting on" is the bar>
- Exclude: <what never gets reported, even if it matches loosely>
- On scan failure: <e.g. "report that the attempt happened; never silently skip">
- On CAPTCHA / block: <e.g. "tell the user immediately so they can intervene">

## <Another watch>

- ...

---
Sanitized starter template. A watch with loose criteria is just noise with a schedule.
