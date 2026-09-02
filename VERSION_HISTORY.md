# Version history

Every design task done on these files: what changed, when, and by whom.
Newest first. Times are IST (UTC+5:30).

**How to add an entry:** add a row at the top of the current table when you
commit. One row per task, not per file. Say what changed for a *person using
the product*, not which lines moved.

| When | Who | File | What changed |
|---|---|---|---|
| 2026-09-02 14:10 IST | Maharshi Gautam, with Claude | `Global_Nomad_UX.html` | **Scoped to the US.** UAE is hidden — reversibly, the same register-and-restore mechanism already used for personas and pages. Nothing about it is deleted: its warehouses, shelters, drivers and disasters stay exactly as built, only reachable again with `GN.restore('AE')` or "Bring back" on the Archived page. |
| 2026-09-02 14:10 IST | Maharshi Gautam, with Claude | `Global_Nomad_UX.html` | **English only.** Arabic and Spanish are hidden the same reversible way, as `lang-ar` and `lang-es`. The two hides are independent — restoring UAE does not bring Arabic back, and the reverse — so US-in-Spanish or UAE-without-Arabic both stay possible later without new code. |
| 2026-09-02 14:10 IST | Maharshi Gautam, with Claude | `Global_Nomad_UX.html` | **A control with one option is no longer shown.** Once only one place (or one language) is active, the Place and Words groups return nothing rather than a picker with a single always-pressed button — the same call already made for the frame caption. Both reappear the instant a second option is restored, live, no reload. |
| 2026-09-02 14:10 IST | Maharshi Gautam, with Claude | `Global_Nomad_UX.html` | **Region wording resolved.** One vocabulary everywhere, no per-place swaps. "Delivery," not "run." Every persona, driver included, sees the same nouns as the office. |
| 2026-09-02 14:10 IST | Maharshi Gautam, with Claude | `Global_Nomad_UX.html` | **Archived gained a third list: places and languages out of sight.** Sits next to the people list rather than folded with page housekeeping, since a place being hidden is a decision worth seeing, not clutter. |
| 2026-09-02 14:10 IST | Maharshi Gautam, with Claude | `Global_Nomad_Design_System.html` | New pattern, **A control with one option is not a control** — why the Place and Words groups disappear rather than showing a single disabled button, and why the two kinds of hide are kept independent. |
| 2026-09-02 12:20 IST | Maharshi Gautam, with Claude | `Global_Nomad_UX.html` | **"Disasters", not "emergencies", everywhere.** Every label, every record id (`EM-01` became `DIS-01`) and the word list itself. Nothing in either file says "emergency" any more, so nobody picking this up has to guess which word is the real one. |
| 2026-09-02 12:20 IST | Maharshi Gautam, with Claude | `Global_Nomad_UX.html` | **The desktop app fills the window.** No frame, no card, no border, no shadow, and no caption naming the person, their scope and their place — all three sit in the control bar above and are changed from there. Every screen now reflows to the window it is given, from a small laptop to a wall display, and reacts to the app's own width rather than the browser's. |
| 2026-09-02 12:20 IST | Maharshi Gautam, with Claude | `Global_Nomad_UX.html` | **One scroller per screen.** The page itself never scrolls; the screen body is the only thing that does. This is what was making the Archived page stretch the whole app instead of scrolling inside it. A new check fails on any other scrollbar appearing. |
| 2026-09-02 12:20 IST | Maharshi Gautam, with Claude | `Global_Nomad_UX.html` | **Unread alerts are a dot, not a number.** An 8px dot at the bell's top right. At the size a bell is drawn, a two-digit count sits under the text floor and turns the icon into a smudge. The count still reaches a screen reader in full — "Alerts, 2 unread" — and the Alerts page is where the number belongs. The dot is a dark neutral, not a warning colour: something new is not the same as something wrong. |
| 2026-09-02 12:20 IST | Maharshi Gautam, with Claude | `Global_Nomad_UX.html` | **A person is a circle, a thing is a rectangle.** The person mark is round everywhere it appears, so you can tell at a glance whether a row is about a person or a record. |
| 2026-09-02 12:20 IST | Maharshi Gautam, with Claude | `Global_Nomad_UX.html` | **Planning works from a phone.** Relief Org Admin and Warehouse Manager now have a bottom bar on the phone reaching their four main pages, Send out included. Planning is no longer desktop-only. |
| 2026-09-02 12:20 IST | Maharshi Gautam, with Claude | `Global_Nomad_UX.html` | **Archived reads as three lists** — records, people taken out of sight, and pages no longer on any side list — the last one folded away, since it is housekeeping. A foldable section is now a component with its own states. |
| 2026-09-02 12:20 IST | Maharshi Gautam, with Claude | `Global_Nomad_Design_System.html` | **The kit caught up with the product, and stopped lying about it.** Two new entries: Person mark and Foldable section. The bell dot written up with the reasoning. New patterns for how the desktop app fills the window and how the phone keeps its frame. The stray-scroll check recorded. Its CSS for shared components is now copied from the product rather than written fresh, with a note saying so. |
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
| 2026-09-02 13:05 IST | `Global_Nomad_UX.html` | **The person mark showed grey initials, off centre, on a dark circle — 2.41:1.** One loose selector, `.side__who span`, was written for the role line beside the mark but matched the mark too. It outranks `.tag2`, so it turned the mark's grid back into a block, which killed the centring and let the ellipsis rule clip the letters, and it repainted white initials grey. Reported by the user, not by any check. |
| 2026-09-02 13:05 IST | `Global_Nomad_UX.html` | **The contrast check was reading a list, not the screen.** It compared the colour pairs we *meant* to use, so it reported clean while grey-on-dark was on display: nobody ever intended `--text-3` on `--solid`, the cascade invented it. There is now a second pass that walks the text actually drawn, reads the colour actually painted, works up the tree for the background actually behind it, and applies the real WCAG threshold for the size and weight. Proven by re-introducing the bug: the new pass reports it, the old one still does not. Zero failures across 65 screens once fixed. |
| 2026-09-02 12:20 IST | `Global_Nomad_Design_System.html` | **The kit was describing a product that no longer existed.** A line-by-line comparison of the two files found thirteen disagreements, six of them real: status chips, table headings, step numbers and tick labels were all still at the sizes they had before the text floor was raised, so the kit was telling a developer to build something that fails the product's own check. All six now match. |
| 2026-09-02 12:20 IST | `Global_Nomad_Design_System.html` | **The person mark was documented wrong** — light grey fill, 11px, a border. The product draws it dark with white letters at 12px. Anyone building from the kit would have shipped the wrong component. The kit's CSS for shared components is now copied from the product, with a note in the file saying to change both together. |
| 2026-09-02 12:20 IST | `Global_Nomad_Design_System.html` | **The touchable-edge colour was the old, lighter one** — darkened in the product weeks ago and never carried across, so the kit published a hex that nothing uses. |
| 2026-09-02 12:20 IST | `Global_Nomad_Design_System.html` | **The greys table printed `NaN`.** The demo-bar colour was listed in the table but never defined, so its contrast could not be worked out. It was also being judged against a bar it never had to meet — it is a surface, sat on rather than read. The judgment is now one list of surfaces instead of a chain of ifs, which is what let a new token slip through mis-classified. |
| 2026-09-02 12:20 IST | `Global_Nomad_UX.html` | **The person mark could have become an oval.** Its size came from a flex basis with no width, so anywhere outside a flex row it would have sized to its own letters. Nothing overflowed, so no check would have reported it. |
| 2026-09-02 12:20 IST | `Global_Nomad_UX.html` | **Every undesigned screen repeated the person, their scope and their place** — the same three facts the control bar already carries and the only place they can be changed. Removed with the frame caption it duplicated. |
| 2026-09-02 12:20 IST | `Global_Nomad_UX.html` | A fold on the Archived page carried the note "closed by default", which describes the control rather than the contents. The caret already says it. |
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
| 2026-09-01 | **A real map.** The location picker is search-and-list with a schematic outline, no tiles, no key, works offline. Swap point is one component. | Needed once any screen shows live geography — driver navigation, tracking, coverage. Confirmed 2026-09-02: still coming later. |
| 2026-09-01 | **Who builds the relief kits.** Three placeholder kit types per warehouse for now, so stock readouts compute. | Needed before the warehouse stock screens or the request flow. |
| 2026-09-01 | Which of the seven failure modes each flow must demonstrate. | Decides how much error handling each screen carries. |
