# Version history

Every design task done on these files: what changed, when, and by whom.
Newest first. Times are IST (UTC+5:30).

**How to add an entry:** add a row at the top of the current table when you
commit. One row per task, not per file. Say what changed for a *person using
the product*, not which lines moved.

| When | Who | File | What changed |
|---|---|---|---|
| 2026-09-01 11:45 IST | Maharshi Gautam, with Claude | `Global_Nomad_UX.html`, `Global_Nomad_Design_System.html` | **Text sizes brought up to the floor.** Fourteen places sat under the 12px desktop / 13px phone minimum — every status chip, every table heading, the step numbers, sidebar detail lines and badges. Three more were inline sizes that looked right on a desktop and silently stayed too small on a phone; those are now classes. The small-caps exception at 11px is gone. The size check now measures the demo bar as well as the product, and skips anything marked decoration. |
| 2026-08-31 13:24 IST | Maharshi Gautam, with Claude | `Global_Nomad_UX.html` | **New file, built clean.** Plain-language pass across every label. Two places (UAE, US) driven by data so more can be added. Real arithmetic layer — no figure typed into a screen. Simulated waiting and failing with seven named failure modes. Real motion, five movements, all switchable off. Control bar: place, person, view, phone model, words, movement, and a collapse. Seven Android sizes plus rotate. WCAG 2.2 AA with four automated checks. |
| 2026-08-31 13:24 IST | Maharshi Gautam, with Claude | `Global_Nomad_Design_System.html` | **New file.** Desktop/Phone entry switch. Words, type, spacing, greys with live-measured contrast, 24 drawn icons, movement set, 8 component entries with states and handover notes, patterns, the seven failure modes with recovery paths, access record, developer handover. |
| 2026-08-31 13:24 IST | Maharshi Gautam, with Claude | `VERSION_HISTORY.md`, `README.md` | Added, so history and setup are recorded in the repo rather than in chat. |

## Fixed while building

| When | File | What was wrong |
|---|---|---|
| 2026-09-01 11:45 IST | `Global_Nomad_UX.html` | Inline font sizes bypassed the phone floor entirely — a size written onto an element cannot be raised by a later rule. Caught only by measuring the drawn page, not by reading the code. |
| 2026-08-31 13:24 IST | `Global_Nomad_UX.html` | The word-check function shadowed the token helper of the same name and would have recursed until the stack overflowed. |
| 2026-08-31 13:24 IST | `Global_Nomad_UX.html` | Two menu labels were fixed English instead of place words, so they would not have switched between "Relief camps" and "Shelters". |
| 2026-08-31 13:24 IST | `Global_Nomad_UX.html` | The progress bar animated its width, which makes a cheap phone redo layout every frame. Now scales instead. |
| 2026-08-31 13:24 IST | `Global_Nomad_UX.html` | The number-check counted `font-weight:650` as invented data. Now ignores text and style strings. |

---

## Earlier work

Reconstructed from file timestamps — these predate this file, so treat the
dates as approximate.

| When | Who | File | What changed |
|---|---|---|---|
| 2026-08-27 | Maharshi Gautam, with Claude | `app/global-nomad.html` | Neutral-grey shell with persona picker and web/mobile switch. Superseded by `Global_Nomad_UX.html`. |
| 2026-08-27 | Maharshi Gautam, with Claude | `prototype/` | Earlier build with branded demo chrome, a scenario scrubber and an event inspector. Rejected: the brief was the product, not a harness around it. Kept for reference. |
| 2026-08-27 | Maharshi Gautam, with Claude | `docs/systems-design.html` | Systems design pack: personas, permissions, data model, state machines, event map, screen inventory. Approved. Still the reference for how the parts fit together. |

---

## Decisions still open

| Raised | What needs deciding | Why it matters |
|---|---|---|
| 2026-08-27 | The full list of words per place. Four are settled (site, warehouse, area, request); more to go — "delivery" versus "run", "job" versus "trip", and whether a driver sees the same nouns as an office. | It touches every screen's wording, so the cost of deciding late grows with each screen built. |
| 2026-08-31 13:24 IST | Which of the seven failure modes each flow must demonstrate. | Decides how much error handling each screen needs. |
| 2026-08-31 13:24 IST | Whether the shareable repository is public or private. | A public repository is readable by anyone. See README. |
