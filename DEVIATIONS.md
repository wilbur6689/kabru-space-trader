# Implementation Deviations & Notes

This document tracks divergences from the deployment plan and design docs as the
project is built. Each entry explains what was changed, why, and what (if
anything) needs revisiting later.

---

## Stage 01 — Foundation

### Project root flattened
The deployment doc shows the folder structure under a `SpaceGame/` root. Since
the actual project is `ace-trader/`, all subfolders (`scripts/`, `scenes/`,
`assets/`, `data/`) live directly under the project root with no `SpaceGame/`
nesting layer.

### EventBus signal warnings suppressed
Godot warns about declared-but-not-emitted signals on every signal in
`event_bus.gd` because the bus declares signals that are emitted from other
scripts. Added `@warning_ignore_start("unused_signal")` at the top of the file
to silence the false positives.

### Theme is minimal
`assets/theme.tres` only sets `default_font` (Inconsolata) and
`default_font_size = 16`. The Orbitron header font is applied via per-Label
`theme_override_fonts/font` in scenes that need it (e.g. `main.tscn` title).
Will be expanded into proper Theme types in Stage 10.

### Fonts require headless re-import after first add
TTF files dropped into `assets/fonts/` while Godot is closed don't get
`.import` files until the editor scans them. Workaround was running
`godot --headless --import` once. Worth knowing if more assets are added
between sessions.

---

## Stage 02 — Galaxy & Navigation

### Galaxy is autoloaded as `Galaxy`
The deployment doc says to put the generator in `scripts/systems/`. It's stored
there, but it's also registered as an autoload (`Galaxy`) because every other
system needs to call into it constantly. Treat it as a singleton.

### Faction blending interpretation
The design doc `02a_galaxy_structure.md` gives self-contradicting examples for
faction border blending: it claims `Q01-R0501` and `Q02-R0501` blend 50/50
between Silk Hand and Phantom Circuit, but those regions border `Q03/Q04`
across the horizontal axis, not each other. Implementation uses a reasonable
interpretation: blend with horizontal neighbor when `region_x ≤ 2` and with
vertical neighbor when `region_y ≤ 2` (50% on border, 25% one cell in). Worth
a design review with the doc author.

### Manhattan distance is uniform across quadrants
Within every quadrant, `R0101` is the corner closest to origin `(0,0)`, so
distance is simply `region_x + region_y` regardless of quadrant. This makes the
safe zone `R0101` of all four quadrants automatically distance 2 → "safe", as
the doc requires.

### Jump tier ranges follow design doc, not deployment doc
Deployment doc 02 says jump tiers are 1→1, 2→adjacent, 3→3, 4→6, 5→unlimited.
Design doc `02a_galaxy_structure.md` says 1→2, 2→5, 3→9, 4→14, 5→unlimited.
**The design doc is treated as source of truth** — implementation uses 2/5/9/14
for tiers 1-4. Deployment doc should be reconciled.

### Jump path routing is axis-aligned
`Navigation.calculate_jump_path()` walks x first then y. This produces correct
hop counts but the path shape is naive. Pass-thru events in Stage 07 may want
smarter routing (e.g., avoiding hostile regions, preferring known systems).

### Trade good base prices are placeholders
The design doc `07_economy_and_trading.md` lists 22 common + 22 rare goods with
weight ranges but no base prices. Stage 02 deployment doc says common 5-140 and
rare 100-4000 shards. `trade_goods_db.gd` assigns prices within those bands but
**values are unbalanced placeholders** and need a tuning pass in Stage 04 or 10.

### Hub assignment is naive
Galaxy generator picks the first friendly+major system as the region's hub. The
doc mentions 1 hub per region with 3-5 sub-systems linked to it, but the
sub-system relationship (which systems "belong to" a hub) is not currently
tracked. `SystemData` would need an `is_sub_system_of: String` field. Affects
future sub-system wholesale pricing (Stage 04).

### Dead space border is sparse
Regions with `x > 10` or `y > 10` get only 5 dead systems instead of 200, all
marked as "dead" alignment. This matches the doc's "no viable systems" intent.

---

## Stage 03 — Core Game Loop

### UI panels live in the center console, not the left monitors
The deployment doc says marketplace, journal, missions, etc. should display in
the **left monitor zone** (220px wide × 3 panels). Implementation puts them in
the **center console** because:
- The placeholder left monitors are too small for tabular content
- The center console is the most prominent UI region
- Easier to debug a single panel slot than three

The 3 left monitor placeholders remain wired up for future use. Easy to migrate
when art and final layouts arrive in Stage 10.

### Discovery types are flat lists
`SearchManager` defines fixed type lists per category (`SERVICE_TYPES`,
`LOOT_TYPES`, etc.) — no rich data attached to each type yet. Stage 03 only
needs the type name + display label. Stages 04-08 will attach interaction data
via `Discovery.data`.

### Blocking discoveries are simplified
Encounters get `is_blocking = true` 40% of the time, and the **next loot
discovery in the same generation pass** becomes blocked by them. The doc
suggests richer blocking relationships ("threats can block related finds") but
"related" isn't defined. Current heuristic is good enough for testing the
mechanic.

### Visiting a discovery costs 0 turns and marks it resolved
Every non-merchant discovery currently shows a placeholder "interaction coming
in a later stage" notification and marks itself resolved. Real interactions
arrive when their owning systems land (combat for encounters in Stage 06, loot
caches for loot in Stage 07/08, etc.).

### `StartingRegion` is an autoload
The deployment doc treats it as a one-shot setup, but I made it an autoload
(`StartingRegion`) so the cockpit can call `StartingRegion.start_new_game()`
from `_ready()` without import-order issues.

### Earth address moved to `Q01-R0101-1010` (was `1000` in the design doc)
The design doc `02a_galaxy_structure.md` places **Earth at `Q01-R0101-1000`**,
but the same doc's system-number rules require the 3rd digit to be **odd**.
`1000` has `0` as the 3rd digit, so **Earth's canonical address is invalid by
its own rules** and the address validator (correctly) rejects it.

Implementation moved Earth to **`Q01-R0101-1010`** — the lowest valid address
in `Q01-R0101` (1 / 0-even / 1-odd / 0-any). The starting system "Wayfarer's
Rest" remains at `Q01-R0101-1230` (1-2-3-0, valid). Both are registered via
`Galaxy.fixed_overrides`. **Design doc should be reconciled** — either move
Earth's canonical address or relax the digit rules for hand-crafted hubs.

### Travel always costs 1 turn in current implementation
Stage 03 doc says intra-region travel is "1 turn per hop". The current
`Navigation.travel_to()` treats every visit as a single hop (1 turn), since
intra-region routing through intermediate systems isn't modeled yet. Will need
revisiting when pass-thru events land in Stage 07.

---

## Stage 04 — Economy & Trading

### Marketplace and Journal panels are in the center console
Same divergence as Stage 03 — see above. Both `marketplace.tscn` and
`journal_trading.tscn` swap into the center console rather than the left
monitor zone.

### Sub-system wholesale discounts not implemented
Phase 4.3 mentions "sub-systems of a hub may offer discounted prices". Skipped
because the hub/sub-system relationship isn't tracked yet (see Stage 02 hub
assignment note). Add a `SystemData.is_sub_system_of` field and a wholesale
modifier in `_system_modifier_for()` when the data structure exists.

### Profit route suggestions not implemented
Phase 4.5 asks for the journal to suggest "buy at A, sell at B within jump
range" routes. Only the price observation table is implemented. The route
suggestion logic is straightforward (cross-product of known prices, filter by
`Navigation.can_jump_to`) — added when the player has enough observed systems
for it to be useful.

### Random market event generation not implemented
Phase 4.6 says "stub random event generation (1-3% chance per turn per
system)". The infrastructure exists (`apply_event`, `apply_price_spike/crash/
shortage`, automatic expiration on tick), but no per-turn RNG triggers them.
The doc itself notes "full event system with narrative comes in Stage 07," so
this is intentional — Stage 07 will wire random triggers and narrative content.

### Negotiation skill discount cap
Haggle success modifier is `clampf(0.05 + level * 0.01, 0.05, 0.20)` —
i.e., 5% discount at level 0, capped at 20% by skill level 15. Failure is a
flat +10% penalty. Numbers are placeholders for Stage 12 balancing.

### Economy state is per-system, not per-merchant
The doc allows a system to have multiple discovered merchant locations. The
current `EconomyManager` keeps **one shared marketplace state per system
address** — every merchant in that system shares stock and prices. Simpler and
matches typical 4X economies. Revisit if design wants distinct merchant
inventories.

### Cargo capacity is fixed by tier table
`CARGO_CAPACITY_BY_TIER = {1: 20, 2: 50, 3: 100, 4: 200, 5: 400}` and
`SPECIAL_SLOTS_BY_TIER = {1..5}`. These are placeholders and may need
rebalancing in Stage 12.

### Buy price observations override sell price observations
`_record_price_observation()` records the buy price even when the player sells
(so the journal always shows the merchant's buy price for comparison). Future
tweaks may want to track buy and sell separately.

---

## Stage 05 — Pilot & Progression

### Background bonus is +100 XP, not +2 XP
The deployment doc says backgrounds give "+2 XP starting bonus" in the chosen
skill. With a 150 XP level-1 threshold, +2 XP is meaningless. Implementation
gives **+100 XP** so the choice has visible impact (puts the pilot 2/3 of the
way to level 1 in the bonus skill). Needs reconciliation in the balancing pass.

### Pilot creation panel, not a separate scene
The doc says "create `scenes/ui/pilot_creation.tscn`" implying it might be a
separate launch scene. Implementation puts it in the **center console** of the
cockpit and shows it on first run when `GameState.pilot == null`. Same UX, no
scene transition required.

### Starting ship is "Wayfarer", not "Shuttle"
Cosmetic — `GameState.new_game()` creates a generic "Wayfarer" with all Tier 1
components, matching the doc's intent of "Shuttle, all Tier 1 components".
Renaming is trivial when you settle on canonical ship names.

### Mission slot unlocks computed but unused
`Progression.get_max_mission_slots()` returns 3-10 based on fame level
correctly, but **no system consumes the limit yet** because the mission system
arrives in Stage 07. Wire it into MissionManager when that lands.

### Combat skill effects stubbed
Phase 5.6 wired Trading → prices, Negotiation → haggle, Exploration → scanner
preview. **Combat skill effects** (damage bonus, accuracy) are not implemented
because combat doesn't exist yet. Stage 06 will add them at the same time as
the combat system.

### Fame allocation is a panel, not a modal
The doc says "popup on fame point earned". Implementation makes it a regular
center-console panel reachable from Pilot Status, with a **notification** when
a point is earned. Doesn't auto-open the allocation screen — players choose
when to spend points. Easy to convert to a real modal `AcceptDialog` if forced
allocation is preferred.

### XP reward values are placeholders
Trading XP = 8 + (total_price / 50), Piloting XP = 12 per jump, Discovery XP = 18,
new system bonus = 24. These are seat-of-the-pants values; Stage 12 balancing
will tune them.

### Statistics live under `pilot.journal.stats`
Stats (systems_visited, discoveries_found, trades_completed, shards_earned/spent,
jumps, combats) are stored as a sub-dict of `pilot.journal` rather than as
top-level Pilot fields. Keeps the Pilot resource lean and lets the journal grow
organically.

### Visited-address tracking duplicates DiscoveryDB
ProgressionManager keeps its own `pilot.journal.visited_addresses` set to detect
"first visit" for the new-system XP bonus, separate from `DiscoveryDB.visited`.
This is because `DiscoveryDB` marks the starting system visited *before* the
pilot exists, so the pilot's first arrival there would otherwise not count.
Could be unified later by ordering `start_new_game` differently.

---

## Stage 06 — Combat

### Combat is auto-loads, not separate scenes
`Enemies`, `Combat`, and `Death` are autoloaded singletons in
`scripts/systems/`. The deployment doc puts them in `scripts/systems/` but
doesn't say to autoload them; doing so simplifies access from the UI panels
and from Navigation/system_overview without import-cycle headaches.

### Combat screen uses center console (consistent with other panels)
Doc Phase 6.3 describes the combat UI with cockpit window for the enemy
sprite, console screen for enemy info, dashboard for player status, and left
monitor for action buttons. Implementation puts **everything in the center
console** (enemy info + bars + actions + log) because the placeholder cockpit
zones aren't sized for a real combat layout. Will be migrated to the proper
zones during the Stage 10 polish pass.

### Enemy stats are placeholder values
Hull/shield/damage/accuracy/defense numbers in `enemy_generator.gd` are
seat-of-the-pants values. Combat will need a serious balancing pass — the
current weapon scaling (`8 + tier*4`) can melt low-fame enemies in 2-3 hits
but stalls against high-fame ones. Stage 12 territory.

### Encounter chance scales by danger zone
On every system arrival, `Navigation._maybe_trigger_encounter()` rolls a
chance based on danger zone (5/15/25/35%) with a ×0.3 multiplier on friendly
systems. The doc says "intra-region travel hop" — since current travel uses
1-hop arrivals (no real intermediate hops modeled), I roll on arrival
instead. This will need rework when Stage 07 wires real pass-thru events.

### Combat damage halving uses integer division
`raw_damage = raw_damage / 2` when defending — uses integer division
intentionally with `@warning_ignore`. Means defending against a 1-damage
attack reduces it to 0 (free defense at very low damage). Acceptable.

### Loaner ship is a fixed Shuttle
On death, the player gets a hardcoded "Loaner Shuttle" with Tier 1
components, hull 60, shield 30. Doc says "loaner Shuttle" — same intent. The
loaner replaces the entire `owned_ships` list (which is currently always
size 1). When ship swapping arrives in a later stage, this needs to preserve
the rest of the fleet.

### Wreckage recovery is a one-shot replacement
When the player travels back to the wreckage and recovers it, the loaner is
discarded and the wreck becomes the active ship (with damaged hull). No
choice between keeping the loaner or recovering — the doc allows that
choice but it's UI-fiddly and the loaner is intentionally disposable.

### Wreckage isn't a real Discovery yet
The doc says wreckage should appear "as a special discovery at that
system". Current implementation tracks it on `GameState.wreckage_address`
and the system overview/repair panel can detect `Death.can_recover_here()`.
**A wreckage row in the discovery list is missing** — needs a small addition
to system_overview to show "Recover Wreck" when at the death address. Easy
follow-up.

### Patrol checkpoint contraband scans not implemented
Doc Phase 6.5 mentions "patrol checkpoint encounters" that scan for
contraband in special slots. Skipped — contraband and special slots aren't
populated by any system yet (Stage 08 territory). Will land alongside the
black market.

### Repair sub-system caps repair, not just price
Doc says "Sub-system repair: max 75% repair, 70% of hub pricing". Implemented
both — `repair_panel._max_repair()` returns 75% of damage at sub-systems,
and `_system_factor()` returns 0.7 for the price. Whether the player can
ever repair to full at a sub-system by visiting twice is currently **yes**
(repairing 75% of damage twice eventually closes the gap). May want to make
the cap absolute later.

### Combat XP and shard rewards are placeholders
`+25 + base_fame*15` shards on victory, `20 + fame*8` combat XP. Tuning
target for Stage 12. The auto-intimidate bonus is `25 + fame*10`.

### `pending_discovery_*` is a transient field on Combat
Used to thread "this combat came from visiting an encounter discovery" so
victory marks the discovery resolved. Cleared on `_end_combat`. Slightly
hacky but cleaner than passing it through every action call.

---

## Stage 07 — Missions & Events

### Missions live in `scripts/systems/`, autoloaded as `Missions`
Same pattern as the other "manager" systems — autoloaded for global access.
The deployment doc only says "create `scripts/systems/mission_manager.gd`",
not whether it should be a singleton. Consistent with `Combat`, `Death`, etc.

### Mission destinations are picked from any valid system in the same/adjacent region
The doc implies destinations should be real generated systems. Implementation
**generates a random valid address** in the origin region (70%) or an
adjacent region (30%), using `_random_valid_system_number()`. The
destination doesn't need to exist yet — `Galaxy.generate_system()` will
produce one on demand when the player travels there. This works because the
generator is deterministic per seed.

### Mission rewards and XP values are placeholders
Base reward varies by type (250 to 800 shards) with a `1 + 0.4 * distance`
distance multiplier. XP is `20 + reward / 25`. All Stage 12 territory.

### Smuggling outcome rolls not implemented
Phase 7.2 describes a 50/25/15/5/5 outcome roll for smuggling deliveries
(exact / shortage / different goods / bust / bonus). Implementation treats
smuggling as a regular delivery for now — visit destination → reward + pirate
fame is awarded via the loot path. The roll system can be added when the
black market arrives in Stage 08.

### Bounty / rescue / clearance missions are stubs
- **Bounty**: Sets `target_entity` to a procedural name but does not actually
  spawn that specific enemy at the destination. Currently any combat at the
  destination doesn't reference the bounty target.
- **Rescue**: Marks `cargo_required.rescued = true` on visit and completes on
  return, but the "blocking threat" requirement isn't checked.
- **Clearance**: Auto-completes on visiting the destination — no actual
  clearance mechanic until Stage 08.

These need fleshing out as the systems they depend on land.

### Investigation completion is purely visit-based
Visiting each clue address marks it visited. No real "clue gathering"
interaction yet — the doc says clues should reveal something, but Stage 09's
narrative system will fill in the actual clue content.

### Travel events are rolled on arrival, not per intermediate hop
Same caveat as the Stage 06 random combat encounter. Real intra-region
hop-by-hop travel isn't modeled yet, so the event rolls once on arrival
instead of every hop.

### Combat encounters and travel events are mutually exclusive
On arrival, `Navigation.travel_to()` first calls `Encounters.roll_event()`.
If it returns an event, the travel event popup fires. **If no event**, it
falls back to the older `_maybe_trigger_encounter()` (Stage 06 raw combat).
This prevents stacking two surprises on a single arrival.

### Combat-triggering events leave the player on the travel-event panel
When a travel event resolves into combat (pirate ambush, bounty hunter,
turf war "Join"), the cockpit's `combat_started` handler swaps to the
combat screen — the travel event panel is destroyed mid-render. Works
fine but worth knowing if behavior looks weird.

### Hazards live in `Discovery.category = "hazard"`
Phase 7.5 says hazards are "discovery types". The Discovery model already
has a `category` field with `service / loot / encounter / passive`. I added
`hazard` as a 5th category and 5 hazard types in `SearchManager.HAZARD_TYPES`
with skill check definitions in `HAZARD_DEFS`. Hazards generate in
neutral/hostile/pirate/dead systems via `ALIGNMENT_WEIGHTS` (0% in friendly).

### Hazard resolution is inline, not a popup
Visiting a hazard discovery runs the skill check and shows the result via
`EventBus.notification_queued`. The doc implies a more elaborate popup with
"reward preview" and "skill check preview" — that's a polish-pass UX
improvement. The mechanic works.

### Radiation Zone hazard is a one-shot, not a per-turn timer
The doc describes radiation zones as "each turn spent in system causes
minor hull damage". Implementation treats them as a regular skill check on
visit. A real per-turn timer requires state on `SystemData` or `DiscoveryDB`
that doesn't exist yet. Easy to add later.

### World events spawn 2% per turn
`NEW_EVENT_CHANCE = 0.02`. Doc says "1-2% chance per turn" — picked the high
end. Each event lasts 10-50 turns. Pre-Stage 12 numbers.

### World events use the player's known regions, not the whole galaxy
A new event picks a random region from `pilot.known_addresses` rather than
truly any region. Means events feel relevant to where the player has been.
Falls back to `Q01-R0101` if no addresses are known.

### Bulletin board is a center console panel
Same UI placement as everything else. Accessed from the System Overview
"Bulletin" button. The doc calls for hub-only access — currently it's
available everywhere. Will gate to hub systems if it feels wrong.

### Star map indicators for affected regions not implemented
Doc Phase 7.6 mentions "star map indicators for affected regions". The galaxy
viewer (debug only) doesn't read world events. Easy to add when there's a
real player-facing star map.

### Patrol checkpoints actually scan now
The deployment doc Phase 6.5 says patrol checkpoints scan for contraband.
Stage 06 left this as a TODO; Stage 07 implemented it as a travel event:
`patrol_checkpoint` event detects items with `is_contraband = true` in
`special_slots` and applies a fine + reputation hit + cargo strip.

### Mission cargo is stored in special slots, not cargo hold
Delivery and smuggling missions push entries into
`Ship.special_slots` with `mission_id` and `is_contraband` fields. They
**don't consume regular cargo capacity** — special slots are unlimited for
now. Stage 06 added a `get_special_slot_capacity()` helper but it's not
enforced. Worth wiring up in the cargo polish pass.

---

## Active Development Shortcuts

These are intentional shortcuts taken during development that **must be reverted before public testing**.

### Pilot creation skipped on new game
Date: 2026-04-07
Location: `scenes/game/cockpit.gd` `_ready()`

When `GameState.pilot == null`, the cockpit currently calls
`StartingRegion.start_new_game("phillip", "merchant")` directly instead of
`show_pilot_creation()`. This puts the developer straight into the game with
a default pilot named **phillip** and the **merchant** background.

**To revert:** restore the `show_pilot_creation()` branch in
`cockpit.gd:_ready()` before any public build. The pilot creation panel
itself is fully functional — it's just bypassed for faster iteration on the
core loop.

---

## Cross-cutting Notes

### Class registry refresh after adding new `class_name` files
Whenever a new file with `class_name X` is added while Godot isn't running, the
project boots with a "Identifier X not declared" error until the editor's
class registry is refreshed. Workaround: run
`godot --headless --import --path <project>` once. This is a Godot quirk, not
a project bug.

### Verification harnesses
Each stage has a `scripts/debug/phaseN_test.gd` that exercises that stage's
core systems via `extends SceneTree`. Run with **Ctrl+Shift+X** in the editor
when the file is selected (or via `File → Run`). They're expected to pass
silently and quit; assertion failures halt execution at the first problem.

### Auto-save timing
`SaveManager` auto-saves on `system_entered` and `combat_ended`. The very first
`system_entered` fires from `StartingRegion.start_new_game()`, so a new game
immediately writes `auto_save.json` before the player has done anything. This
is fine — it just means new games always have a recoverable starting point.
