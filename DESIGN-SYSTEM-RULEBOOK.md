# Design System Rulebook

*Extracted from a real, working enterprise console (Tabsons QC Ops — Gyana.ai), cross-checked directly against its live code rather than its own design-system doc, which had drifted out of date in places. That's itself the first lesson: a design doc is a claim, not a guarantee — verify against the code before trusting it.*

## How to use this document

This is not a component library. None of the exact class names, hex values, or markup here need to survive a port to a different app. What needs to survive is the **discipline** — the structural and behavioral laws a mature, well-audited design system enforces, each stated as a rule plus the reasoning that makes it non-negotiable. Where a concrete number or snippet is included, it's there to make the rule *legible*, not to be copied verbatim.

Each entry below is written as:
- **Law** — the rule itself, stated generally.
- **Why** — the reasoning, usually a specific failure this rule was written to prevent.
- **Reference** — how this system embodies it (grounding, not gospel).

---

## 1. Layout & shell

**Law: The page shell scrolls in exactly one place. Every panel inside it manages its own overflow, and layout containment stops a panel's internal scroll from leaking into its ancestors.**
Why: if two nested elements both think they own scrolling, the outer one silently inherits the inner one's overflow height, and the pointer starts scrolling "the whole page" while the panel's own scrollbar sits there unused — confusing and hard to diagnose after the fact.
Reference: `html,body{overflow:hidden}`; the shell's main content area is the only page-level scroller (`overflow-y:auto`). A table's own scroll container additionally uses `contain:layout` specifically because an expandable/grouped table can insert far more rows than fit on screen the moment a group opens — without containment, that unclipped height counted toward every ancestor up to the page.

**Law: A view is a vertical stack in a fixed, predictable order — header region, then the main working surface, then any floating/secondary controls — never reordered per-screen.**
Why: consistent order means a user's spatial memory of "where things are" transfers across every screen in the app; a screen that reorders this breaks that transfer for no benefit.
Reference: every view = `view-head` (title + description + primary action) → `panel` (toolbar → content → pager, as its own flex column) → optional secondary bar (legend, bulk-action bar). Verified identical across five unrelated screens.

**Law: A "maximize to fill the screen" mechanism should be one generic, positional override — never bespoke per-component logic.**
Why: if every panel needed its own maximize implementation, the behavior would drift screen to screen, and each one would need its own bug-fixing. A single mechanism, applied uniformly, means a fix in one place fixes it everywhere.
Reference: any panel is already toolbar → flexible content → pager; "maximize" only overrides `position`, changing nothing about internal behavior. Only one thing is ever maximized at once (see §11, the recurring "one active thing" law).

**Law: A collapsible chrome element's state (e.g., a collapsed sidebar) should persist across everything that doesn't explicitly touch it — and nothing should touch it implicitly.**
Why: a user who collapses the sidebar for more room did that on purpose; having it silently reset on an unrelated action (switching accounts, signing back in) is a small but real trust violation — it makes the UI feel like it isn't listening.
Reference: the sidebar-collapse flag lives on a single class that only its own toggle button ever touches — switching accounts or signing out and back in never clears it. Contrast deliberately with the *maximized-panel* state, which **is** explicitly reset on every navigation, because that state belongs to the view you were on, not to the session.

---

## 2. Top bar

**Law: Persistent global chrome (brand, account switcher, sign-out) lives in one fixed-height bar, right-aligned against the brand mark, and it wraps to a second line under width pressure — it never truncates or shrinks the things inside it.**
Why: truncating a person's name or role to save space produces ambiguous, sometimes embarrassing labels ("Sarah W..."); wrapping costs vertical space but never loses information.
Reference: the top bar's account cluster is `margin-left:auto` (pins right against the brand) and both the bar and the cluster set `flex-wrap:wrap` — under a narrow viewport the account controls drop to a second row rather than clip.

**Law: When two competing elements in the same fixed-position bar tie on CSS specificity, resolve the tie by qualifying the selector on an ancestor class — never by relying on source order, and never by reaching for `!important`.**
Why: a tie broken by source order is a tie waiting to be flipped by the next unrelated edit that reorders the file. Ancestor-qualifying wins unconditionally, so it can't regress by accident.
Reference: a real bug from this exact trap — a profile-avatar button's own class was the same specificity as a more generic `button` selector inside its container, and lost by sitting earlier in the file; the fix was `.container button.profile-avatar`, not reordering the CSS.

---

## 3. Bottom / floating action bars

**Law: A floating action bar tied to a multi-select (bulk edit/delete/reassign) appears the instant the selection count is nonzero and disappears at zero — it is never a persistent, disabled-by-default control.**
Why: a persistent bulk-action bar sitting empty/disabled clutters every view all the time for a capability most visits never use; showing it only when it's actionable keeps the UI quiet until it has something to say.
Reference: a boolean-driven `.on` class toggled purely off `selection.size > 0`, checked on every selection change.

**Law: If any full-screen/maximize overlay exists in the app, every floating control that must survive it (a bulk-action bar, the maximize toggle itself) needs deliberate, individually-justified elevated stacking — but *only* small, self-contained controls should be elevated this way. Elevating a large block of chrome (a whole header, a whole toolbar) above a full-screen surface risks painting illegible text directly on top of the content the maximize was meant to showcase.**
Why: this is a real, hard-won lesson from this exact codebase. A first fix elevated *every* piece of chrome near a maximizable panel — including full-width title/subtitle/dropdown blocks — reasoning "so it stays reachable." It technically worked, but it also meant a page title and a table's own column headers now occupied the same visual space, overlapping illegibly. The fix was reverted for anything wider than a small icon-sized control: chrome now gets *covered* by the maximized surface (which is the entire point of maximizing — focus on one thing), and only truly small, isolated controls (the maximize/collapse toggle itself, a floating action pill) keep elevated stacking.
Reference: the bulk-action bar and the maximize/collapse button each independently carry an elevated z-index with a code comment explaining exactly why; a full header block that was given the same treatment was reverted after producing overlapping text, with the reasoning kept in the comment as a warning against repeating it.

---

## 4. Overlay & z-index layering

**Law: Define an explicit, small, ordered stacking scale up front, and never invent an ad-hoc z-index mid-feature without placing it deliberately in that scale.**
Why: z-index wars (an element needing `9999` to "just work") are a symptom of no shared scale existing. A small number of well-known tiers, each used for exactly one purpose, means every new overlay has an obvious slot rather than a guess.
Reference: this system's actual tiers, low to high — sticky table headers/columns (single digits) → full-screen overlays like a persona picker or maximized panel (~50–65, with small floating controls a few points above their own maximized surface) → standard modal (80) → a confirm dialog stacked on top of another modal (90) → toast (100) → a hover-preview layer (120) → a context/tag menu (130) → the single shared tooltip layer, deliberately the highest of all (900), because a tooltip must never be hidden by anything it's explaining.

**Law: Overlays can nest (a confirm dialog opening on top of a wizard that triggered it). Track the *open order* explicitly, and make a global "dismiss" key (Escape) close only the top of that stack — never all open overlays, and never an arbitrary one.**
Why: dismissing everything on one Escape press would silently discard whatever the user was doing in the overlay underneath (an in-progress multi-step form, for instance) — a destructive surprise from what should be a safe, reversible key.
Reference: an explicit stack array pushed to on open and spliced from on close; Escape looks up the top of that stack and closes only that one; with nothing open, the same key falls through to a secondary "back out of full-screen mode" action instead of doing nothing.

---

## 5. Modals

**Law: A modal is a fixed-size frame with three regions — a pinned header, a scrollable body, and a pinned action bar — never a single scrolling blob.**
Why: if the whole modal scrolls as one unit, the title and the confirm/cancel buttons scroll out of view on a long form, forcing the user to scroll back up just to know what they're confirming, or down just to submit.
Reference: `display:flex;flex-direction:column` on the modal itself; header and footer are `flex:0 0 auto` (fixed), the body is `flex:1 1 auto;overflow-y:auto` (the only part that scrolls).

**Law (STANDING RULE, quoted verbatim from this system's own CSS comment): "A modal must not change height while the reader is inside it." A single-purpose dialog sits at its natural height and never moves. A multi-step dialog is pinned to the height of its *tallest* step from the first render, not re-measured per step.**
Why, with the receipts: this system's own author measured a real three-step wizard's natural height per step at 712px, 792px, and 566px. Without pinning, moving from step 2 to step 3 would shrink the dialog by 226px *while the user's cursor is still over it* — a jarring resize under the pointer. Pinning to the tallest observed step and toggling step content with simple show/hide inside that fixed frame eliminates the resize entirely.
Reference: a dedicated modifier class applied only to multi-step dialogs, fixing height to a generous viewport-relative ceiling; steps are shown/hidden via display toggling inside that unchanging frame, never by letting the frame's own height respond to content.

---

## 6. Wizards / multi-step flows

**Law: Step-forward validation blocks on that step's own required fields only — never on fields belonging to a later step the user hasn't reached yet.**
Why: validating ahead of where the user currently is produces incomprehensible errors ("Step 1 says something on Step 3 is wrong") and blocks forward progress the user hasn't earned the right to be blocked on yet.
Reference: the step-advance function checks `if(step > 1) validate step-1 fields`, `if(step > 2) validate step-2 fields` — each gate only fires for fields on steps already passed through, and passing a step explicitly clears any stale error flag on it (a step you already fixed should never still show as broken when you return to it).

**Law: A step indicator (Step N / Total) is a live, computed label, not a hardcoded string — because the total can itself be conditional (e.g., a simpler flow skips a step entirely under certain choices).**
Why: a hardcoded "Step 2 of 3" is wrong the instant the flow's step count becomes conditional on an earlier answer; computing it keeps the label honest automatically.
Reference: total-step count is a function of the in-progress draft's own state, not a constant; the label re-renders from that function every step.

---

## 7. Scrolling

**Law: A sticky header's CSS specificity must be defended explicitly wherever an attribute selector on the same element (e.g., one that adds a tooltip) could accidentally outrank it and silently cancel its `position: sticky`.**
Why: this is a genuinely subtle, easy-to-reintroduce bug. `[data-tip]{position:relative}` (an attribute selector, specificity 0-1-0) can outrank a plain `thead th{position:sticky}` (element+element, specificity 0-0-2) purely on specificity math, with no visual warning — the header just quietly stops sticking the moment someone adds a tooltip to it, and scrolls away, leaving a gap.
Reference, quoted verbatim as a reusable warning: *"A header that carries a tooltip must STILL be sticky. `[data-tip]{position:relative}` scores (0,1,0) and beats plain `thead th` (0,0,2), which silently un-sticks any header with a tooltip — it scrolls away and rows show through the gap. Re-assert sticky at (0,1,2) so the attribute selector can't win."*

**Law: When a scroll container's content can legitimately overflow its own box (an expandable table, for instance), apply layout containment so that overflow is absorbed locally — not counted toward every ancestor's scrollable area.**
Why: without it, a component correctly sized to shrink and scroll internally can still silently expand its *ancestors'* scrollable area to match its full unclipped content height, producing a second, competing scrollbar at the page level that fights the component's own.
Reference: `contain:layout` on the table's own scroll wrapper, added after exactly this symptom was observed and diagnosed.

---

## 8. Buttons

**Law: A button has a small, fixed set of ranks — one filled "primary" call-to-action, an outlined secondary rank for everything else that still needs to read as clickable, and a tinted (not solid) danger rank that only goes solid on hover/focus. A bare underlined link-style button and a small icon-only button round out the set. Resist inventing more ranks.**
Why: more ranks dilute the meaning of each one. If "primary" existed twice, the eye no longer knows which action is *the* one to take.

**Law: Exactly one primary-rank button per view (or per open modal). No exceptions bought for convenience.**
Why: a screen with two filled, high-contrast buttons forces the user to decide which one is "more" primary — a decision the design should have already made for them.
Reference: verified across five unrelated screens in this system — every one has zero or exactly one primary button in its header; a screen briefly showing two only does so because they're mutually exclusive (one is always hidden while the other shows), never two visible at once.

**Law: A disabled control is dimmed, never hidden or removed from layout.**
Why: hiding a disabled control teaches the user nothing about why an action isn't available yet; leaving it visible-but-dim signals "this exists, but not right now" and preserves the layout's spatial stability.
Reference: `opacity` drop plus a non-interactive cursor — nothing else about the element changes.

**Law: An icon inside a button gets spacing that adapts to whether it's alone or followed by a label — a fixed margin when a label follows, and zero margin when the icon is the button's only child.**
Why: a hardcoded icon margin looks correct next to a label and off-center-with-dead-space when the icon is alone; conditioning the margin on whether a label sibling exists fixes both cases with one rule instead of two near-duplicate button variants.

---

## 9. Input fields

**Law: A field is always the same three-part shape — an uppercase, small, letter-spaced label; the control itself, full width of its container; and an optional muted hint line beneath. This shape never varies by field type.**
Why: uniform shape means a user's eye learns the pattern once ("label, then control, then maybe a hint") and never has to re-parse an unfamiliar field layout.

**Law: A field's width is always relative to its container — 100% normally, or an equal flex-share when several fields sit in one row — never a hardcoded pixel width chosen per-field.**
Why: a hardcoded width breaks the moment the field's container is reused somewhere narrower or wider than the one it was designed against; a relative width self-adjusts.

**Law: A reusable multi-select "chip toggle" mechanism, once built, should be a single generic function taking a container id, an item list, a selection set, and a re-render callback — not re-implemented per feature that happens to need multi-select chips.**
Why: this system's version of this exact pattern is reused across at least four unrelated features (queue selection in three different contexts, language selection). A generic function means a UX fix to the mechanism (say, adding a "select all" affordance) lands everywhere at once instead of needing four separate patches.

---

## 10. Error-mode rules (the validation contract)

**Law: A validation failure marks the specific control it belongs to — border color, a small icon-prefixed message directly beneath it — and only the *first* failing field per attempt gets marked. Fixing it and resubmitting reveals the next problem in turn, rather than dumping every error on screen at once.**
Why: dumping every error at once is overwhelming and doesn't tell the user what to fix *first*; one-at-a-time keeps each correction feel like real progress.

**Law: The instant the user touches the specific field that was flagged, its error clears itself — automatically, without waiting for a re-submit.**
Why: an error message that outlives the correction that fixed it reads as the app not noticing you already fixed the problem — a small, avoidable trust cost.
Reference: a single global "on input" listener, scoped by checking whether the event's target is inside a currently-errored field wrapper — so it only clears the field actually being edited, not every error on the page.

**Law: Reserve a separate, form-level (not field-level) error banner for refusals that don't belong to any single control — e.g., "pick at least one item from this list" where there's no one field to blame. Keep this mechanism visually and structurally distinct from a field-level error, and don't conflate the two.**
Why: forcing a formless error onto an arbitrary field (just to have somewhere to put it) misleads the user about what's actually wrong; a dedicated form-level banner tells the truth about the error's real scope.

**Law: A validation failure always fires through two channels at once — the persistent, visible marker (so it can be found by looking), and a transient announcement (so it's caught by someone not looking at that exact spot, and by assistive tech).**
Why: a visible-only error can be missed if the user's eye is elsewhere when it appears; an announcement-only error disappears and leaves no trace to refer back to. Together, neither failure mode applies.

---

## 11. State rules

**Law: Every empty/error/no-results state shares one generic builder — icon, title, optional body text, optional call-to-action — never a bespoke one-off per screen.**
Why: an audit of this exact system found over a dozen distinct empty-state messages, several of which told the user to "clear your filters" without actually providing a control to do it. A single shared builder makes that omission structurally impossible to repeat, because the action slot is a real parameter, not an afterthought.

**Law: Simulate latency only where a real backend would genuinely wait (a save, a report generating, a data pull) — never on a purely client-side action (switching tabs, expanding an accordion, toggling a display preference). Keep the simulated delay just past the point where a user perceives that "something happened," and no longer.**
Why: simulating delay everywhere trains the user that the whole app is slow; simulating it nowhere makes a genuinely backend-shaped action (like a save) feel suspiciously, unrealistically instant. Match the simulation to where latency would really exist.
Reference: a single shared delay constant, chosen just past the ~400ms threshold at which people start to consciously notice a wait; the underlying action's real result is always computed synchronously and immediately — the delay only gates *when the already-final result is revealed*, so nothing about correctness or timing-sensitive logic depends on the simulated wait.

**Law: A control mid-simulated-action disables itself and shows a spinner *in place of* (not in addition to a separate loading label) its own icon slot, keeping its original text — and guards against being triggered twice by a fast double-click.**
Why: swapping the whole button for a generic "Loading…" state loses context (what was I doing?); keeping the label and only swapping the icon for a spinner keeps the user oriented, and the re-entrancy guard stops a double-click from firing the real action twice.

**Law: Respect a user's reduced-motion preference at both layers where motion can hide — CSS transitions/animations (governed by the stylesheet's own duration token) *and* any JS-driven delay/timer that simulates loading (which no CSS media query can reach, since it's not a CSS transition at all).**
Why: a CSS-only reduced-motion fix silently misses anything implemented as a `setTimeout`-driven UI delay — that still "moves," just without CSS animating it, and still needs to be skipped for someone who's opted out of motion.

---

## 12. The recurring meta-law: exactly one active thing at a time

This shows up independently, in at least four unrelated subsystems of this codebase, implemented three different ways — which is exactly what makes it a *law* rather than a coincidence:

- **A single tracked id, explicitly unset before a new one is set** (an active full-screen panel; an active working "period"/scope) — maximizing a second thing automatically un-maximizes the first, with no possibility of two being active simultaneously because there is only one variable to hold the answer.
- **A selection set gated by an explicit rejection when its scope would change** (a multi-select that only spans one logical group at a time) — trying to select an item from a second group while the first group's items are still selected is refused outright, with direct feedback ("one at a time"), rather than silently mixing scopes.
- **An ordered stack where only the top is ever addressable** (nested overlays) — closing "the current one" always means the most recently opened one, never an arbitrary member of the stack.

**Law: When a feature needs "only one X active," don't leave it to convention or hope — pick one of these three mechanisms deliberately (single tracked id, rejecting-selection-set, or ordered stack) and make the enforcement structural, not a habit someone has to remember to maintain by hand.**
Why: an unenforced "only one at a time" convention degrades the first time someone adds a new code path that sets the state without checking/clearing the old one. A structural mechanism (there's only one variable; the wrong action is actively refused; only the stack's top is reachable) can't degrade that way — it's not possible to violate by omission.

---

## 13. Padding, spacing, and tokens — an honesty note

**Law: If you declare a design token (a spacing/radius/scale variable), *use* it everywhere the value recurs — don't let components hardcode literal values that happen to match it. A declared-but-unused token is worse than no token: it looks enforced and isn't, which misleads anyone reading the stylesheet later into thinking a value is centrally controlled when it's actually copy-pasted N times.**
Why, with the receipt: this exact system declares `--radius`, `--space`, and `--pad` custom properties at the top of its stylesheet — and then never references any of the three anywhere else in the file. Every component hardcodes its own literal pixel radius instead (7px on buttons and inputs, 6px on small icon controls, 8–10px on containers, 999px on pills) — values that are *informally* consistent by discipline, but not *mechanically* enforced by the token. Two tokens in the same system (`--dur`, `--ease`) *are* genuinely referenced everywhere they apply, proving the team knows how to do this correctly when they choose to — which makes the unused three even more clearly an oversight worth not repeating, not a stylistic choice.

**Practical takeaway for a new system:** pick your scale early (a controls-radius, a container-radius, a pill-radius; a base spacing unit and its multiples), declare it as tokens, and audit — don't assume — that every component actually consumes the token rather than a literal number that happens to currently match it.

---

## 14. Interaction & focus conventions

**Law: A visible focus ring is a global default (every interactive element gets one automatically), with per-component overrides existing only where the *default* ring would be visually wrong for that component's shape (e.g., an oversized hit-target around a small visual marker needs its own, differently-offset ring, not the global default clipped awkwardly against it).**
Why: a global baseline means keyboard accessibility is the default state of the app, not something bolted on per-component after the fact; overrides stay rare and each one earns its existence by solving a real shape mismatch, not by habit.

**Law: A tooltip layer is a single, shared, viewport-anchored element — repositioned live against whichever control triggered it — never a per-component, relatively-positioned pseudo-element.**
Why: a tooltip implemented as a CSS `::after` inherits its ancestor's overflow/clipping and stacking context, which breaks the instant that ancestor needs `overflow:hidden` or text truncation for an unrelated reason. A single shared layer, positioned via live viewport-coordinate math and clamped to the visible viewport, sidesteps that entirely and also fixes any host element that needs to *both* truncate its own text *and* carry a tooltip — a combination that's otherwise a real conflict.
Reference: the shared layer measures the trigger's real screen position on each show, prefers a default side, flips to the opposite side if it would run off-screen, and clamps its final position to stay fully inside the viewport regardless of where the trigger sits.

**Law: A theme/brand's animation duration is a single token multiplied through every transition rule — never a per-component hardcoded duration — so a system-wide "no motion" mode (a deliberate wireframe/reduced-motion state, or honoring the user's OS-level reduced-motion preference) is a single token flip, not a search-and-replace across the stylesheet.**
Why: hardcoded per-component durations mean "turn off all motion" requires finding and zeroing every one individually, with no guarantee the search was exhaustive; a single token multiplied everywhere makes it correct by construction.
