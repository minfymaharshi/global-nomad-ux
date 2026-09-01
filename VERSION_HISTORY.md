# Version history

Every design task done on these files: what changed, when, and by whom.
Newest first. Times are IST (UTC+5:30).

**How to add an entry:** add a row at the top of the current table when you
commit. One row per task, not per file. Say what changed for a *person using
the product*, not which lines moved.

| When | Who | File | What changed |
|---|---|---|---|
| 2026-09-01 22:19 IST | Maharshi Gautam, with Claude | `Global_Nomad_UX.html` | **The side list from the sketches.** Grouped under headings, foldable to a 60px icon rail, current page boxed, and the bell plus the person's name pinned in a block at the bottom rather than sitting in the list. Applies to every persona. |
| 2026-09-01 22:19 IST | Maharshi Gautam, with Claude | `Global_Nomad_UX.html` | **Four people now, four out of sight.** Super Admin, Relief Org Admin, Warehouse Manager, Driver. Site lead, Stock keeper, Send-out planner and Buyer are in the register with a reason and a date — `GN.restore('site-lead')` brings any of them straight back. |
| 2026-09-01 22:19 IST | Maharshi Gautam, with Claude | `Global_Nomad_UX.html` | **Nothing is ever deleted.** Every record carries archived, when, by whom and why. The Archived page lists records and hidden people-views side by side, each with a Bring back. Eleven pages no longer on any side list are registered rather than orphaned. |
| 2026-09-01 22:19 IST | Maharshi Gautam, with Claude | `Global_Nomad_UX.html` | **Who added what.** Every record now says who put it in the system and whether it came from the platform catalogue or an organisation's own. This is what Super Admin's "added by" reads. Warehouses, fleets, vehicles and drivers inherit it from whoever runs them. |
| 2026-09-01 22:19 IST | Maharshi Gautam, with Claude | `Global_Nomad_UX.html` | **Fleets exist.** A fleet groups drivers and may belong to the platform, to a relief organisation, or to a haulage company contracted in. A driver with no fleet is a volunteer — one is seeded that way. |
| 2026-09-01 22:19 IST | Maharshi Gautam, with Claude | `Global_Nomad_UX.html` | **Impact is worked out, not remembered.** `GN.IMPACT.of('stores')` names the people, pages and surfaces a change touches, including the people currently out of sight who would be affected when restored. |
| 2026-09-01 22:19 IST | Maharshi Gautam, with Claude | `Global_Nomad_UX.html` | **The demo bar stepped down** to its own value, L\* 89.8 against the page's 92.7. At the old value the two were 1.3 apart, which is invisible, so the boundary between the tool and the product rested entirely on a 1px line. The touchable edge darkened slightly to hold 3:1 on the new bar. |
| 2026-09-01 22:19 IST | Maharshi Gautam, with Claude | `Global_Nomad_Design_System.html` | Side list entry with every state. The archive rule written up as a pattern. A section on working out who a change affects. Two more checks recorded. |
| 2026-09-01 11:45 IST | Maharshi Gautam, with Claude | `Global_Nomad_UX.html`, `Global_Nomad_Design_System.html` | **Text sizes brought up to the floor.** Fourteen places sat under the 12px desktop / 13px phone minimum — every status chip, every table heading, the step numbers, sidebar detail lines and badges. Three more were inline sizes that looked right on a desktop and silently stayed too small on a phone; those are now classes. The small-caps exception at 11px is gone. The size check now measures the demo bar as well as the product, and skips anything marked decoration. |
| 2026-08-31 13:24 IST | Maharshi Gautam, with Claude | `Global_Nomad_UX.html` | **New file, built clean.** Plain-language pass across every label. Two places (UAE, US) driven by data so more can be added. Real arithmetic layer — no figure typed into a screen. Simulated waiting and failing with seven named failure modes. Real motion, five movements, all switchable off. Control bar: place, person, view, phone model, words, movement, and a collapse. Seven Android sizes plus rotate. WCAG 2.2 AA with four automated checks. |
| 2026-08-31 13:24 IST | Maharshi Gautam, with Claude | `Global_Nomad_Design_System.html` | **New file.** Desktop/Phone entry switch. Words, type, spacing, greys with live-measured contrast, 24 drawn icons, movement set, 8 component entries with states and handover notes, patterns, the seven failure modes with recovery paths, access record, developer handover. |
| 2026-08-31 13:24 IST | Maharshi Gautam, with Claude | `VERSION_HISTORY.md`, `README.md` | Added, so history and setup are recorded in the repo rather than in chat. |

## Fixed while building

| When | File | What was wrong |
|---|---|---|
| 2026-09-01 22:19 IST | `Global_Nomad_UX.html` | The side list animated its own width, which makes the browser redo layout every frame. Caught by the movement check within minutes of writing it. It snaps now. |
| 2026-09-01 22:19 IST | `Global_Nomad_UX.html` | The bell inherited a full-width rule from the nav item and squashed the name beside it to 8px, with its badge hanging outside the sidebar. No colour or size check would ever have seen this, so an overflow check was added. |
| 2026-09-01 22:19 IST | `Global_Nomad_UX.html` | Three side-list items pointed at pages that did not exist, and four pages were left with no way in. Both silent. A link check now fails on either. |
| 2026-09-01 22:19 IST | `Global_Nomad_UX.html` | Simulated delays were raw millisecond values written into a screen, which is a figure written into a screen. They are named speeds now. |
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
| 2026-08-27 | Region wording beyond the four settled words — "delivery" versus "run", "job" versus "trip", and whether a driver sees the same nouns as an office. | Touches every screen's wording, so the cost grows with each screen built. |
| 2026-09-01 | **A real map.** The location picker is search-and-list with a schematic outline, no tiles, no key, works offline. Swap point is one component. | Needed once any screen shows live geography — driver navigation, tracking, coverage. |
| 2026-09-01 | **Who builds the relief kits.** Three placeholder kit types per warehouse for now, so stock readouts compute. | Needed before the warehouse stock screens or the request flow. |
| 2026-09-01 | Which of the seven failure modes each flow must demonstrate. | Decides how much error handling each screen carries. |
| 2026-09-01 | Whether Warehouse Manager's phone view carries only floor tasks — receiving, picking, confirming a load leaves — with planning left to the desk. | Proposed and not yet confirmed. Changes what gets built for that persona. |
