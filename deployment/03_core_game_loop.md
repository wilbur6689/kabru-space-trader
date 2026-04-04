# Stage 03: Core Game Loop

## Goal
Implement the fundamental gameplay loop: arrive at a system, search to discover locations, visit discovered locations, and travel to another system. This is the skeleton that every other system hangs on. By the end of this stage you can play a basic version of the game — moving through the galaxy, discovering things, and spending turns.

**Design Docs:** [01_core_game_loop.md](../docs/01_core_game_loop.md), [02c_discovery_system.md](../docs/02c_discovery_system.md), [10_ui_and_presentation.md](../docs/10_ui_and_presentation.md)
**Prerequisites:** Stage 02 complete (galaxy generation, navigation, DiscoveryDB)
**Outputs:** Turn system, SearchManager, discovery generation, cockpit UI shell, system overview screen, travel flow

---

## Phase 3.1 — Turn System

**Goal:** Implement the turn-based timing system where only searching and traveling consume turns, and the world reacts after each turn.

### Tasks
- [ ] Add turn management to GameState:
  - `advance_turn()` — increment `turn_count`, update `star_date`, emit `EventBus.turn_started`
  - After all turn processing, emit `EventBus.turn_ended`
- [ ] Define which actions cost turns:
  - **Search**: 1 turn
  - **Travel (intra-region)**: 1 turn per hop
  - **Travel (inter-region)**: 1 turn (single jump, no pass-thru)
  - **Visit, trade, repair, save**: 0 turns (free actions)
- [ ] Implement world reaction hook on `turn_ended`:
  - Placeholder for: economy tick, mission deadline decrement, event checks
  - For now, just log "Turn X completed" to console
- [ ] Track turns-at-current-system (for future event timing)

### Verification
- Searching increments turn count by 1
- Traveling increments by correct hop count
- Visiting a location does not increment turns
- `turn_started` and `turn_ended` signals fire in correct order

---

## Phase 3.2 — Discovery Generation

**Goal:** When a player arrives at or searches a system, generate its discoverable locations based on tier and alignment.

### Tasks
- [ ] Create `scripts/systems/search_manager.gd`
- [ ] Implement `generate_discoveries(system_data: SystemData, seed: int) -> Array[Discovery]`:
  - Count based on population tier (Major 12-18, Moderate 7-11, Minor 3-6, Measly 3)
  - Use seeded RNG so same system always gets same discoveries
  - Generate a mix of discovery types based on alignment:
    - **Friendly**: Heavy services (merchant, repair, shipyard, mission board), light encounters
    - **Neutral**: Balanced services and loot, some encounters
    - **Hostile**: Heavy encounters and environmental hazards, some loot, few services
    - **Pirate-Controlled**: Enemy outposts, pirate merchants, black market contacts, combat encounters
    - **Dead**: Loot caches, salvage, ruins, artifact finds, environmental hazards
  - Assign blocking relationships (hostile discoveries can block related finds)
- [ ] Implement alignment-based initial visibility:
  - **Friendly/Neutral**: 3 service discoveries are known (is_discovered = true) on arrival
  - **Hostile/Pirate-Controlled/Dead**: nothing known on arrival (all is_discovered = false)
- [ ] Implement `perform_search(system_address: String) -> Discovery`:
  - Costs 1 turn (calls `GameState.advance_turn()`)
  - Picks the next undiscovered location
  - Scanner tier influences quality weighting (better scanners find better locations first)
  - Checks for blocking — if a threat blocks a discovery, the threat is found first
  - Marks the discovery as found in DiscoveryDB
  - Emits `EventBus.discovery_found`
  - Returns the discovered location
- [ ] Implement scanner pre-search preview:
  - `get_search_preview(system_address) -> Dictionary` — returns category and risk level of next potential discovery
  - Higher scanner tier reveals more detail

### Verification
- Friendly system starts with 3 known discoveries
- Dead system starts with 0 known discoveries
- Each search reveals one new discovery and costs 1 turn
- Same seed + same system = same discoveries every time
- Blocking threats are discovered before their blocked locations
- Scanner preview shows category without revealing the exact discovery

---

## Phase 3.3 — Cockpit UI Shell

**Goal:** Build the primary game screen — the cockpit view with interactive zones. This is placeholder art with functional UI panels. The cockpit is always visible; all gameplay happens through its monitors and windows.

### Tasks
- [ ] Create `scenes/game/cockpit.tscn` as the main gameplay scene
- [ ] Define clickable cockpit zones as Control nodes:
  - **Left Monitors** (3 panels): Main info displays — will host marketplace, journal, missions, bulletin board, inventory
  - **Console Screens** (center): Ship status, navigation input, scanner, system overview
  - **Cockpit Window** (upper area): Current system visual, travel animations, combat view
  - **Cargo Area** (visual indicator): Shows cargo fill level
  - **Dashboard** (bottom strip): Quick stats — shards, hull/shields, discovery progress, turn counter
- [ ] Use placeholder rectangles with labels for each zone
- [ ] Implement zone click handlers that switch the active panel content
- [ ] Build the **Dashboard** with live data:
  - Shards display (green text)
  - Hull bar (red when low)
  - Shield bar (blue)
  - Discovery progress: "Discovered: X / Y"
  - Turn counter: "Turn: XXX"
  - Current address display
- [ ] Implement screen transition system:
  - Each monitor/zone can display different content panels
  - Panels swap with a simple fade or instant switch
  - `EventBus.screen_changed` fires on panel switch
- [ ] Add notification popup system:
  - Queue messages that appear briefly in the cockpit
  - Types: info (cyan), success (green), warning (yellow), danger (red)

### Verification
- Cockpit scene loads with all zones visible and labeled
- Dashboard shows live GameState data (shards, hull, turn count)
- Clicking zones switches their content panel
- Notifications appear and auto-dismiss
- Scene is responsive to different window sizes

---

## Phase 3.4 — System Overview Screen

**Goal:** When the player is at a system, show the system overview with available actions: Search, Visit discovered locations, Travel, and Save.

### Tasks
- [ ] Create `scenes/ui/system_overview.tscn` (displays in console screen zone)
- [ ] Show system information:
  - System name, address, population tier, alignment
  - Danger zone indicator
  - Discovery progress bar: "Discovered: X / Y"
  - Hub type (if applicable)
- [ ] Show action buttons:
  - **Search** — enabled if undiscovered locations remain; shows scanner preview tooltip
  - **Visit** — opens discovery list (only discovered locations)
  - **Travel** — opens navigation input
  - **Save** — quick save to current slot
- [ ] Build discovery list panel:
  - List all discovered locations with type icons and names
  - Undiscovered slots shown as "??? — Unknown" with count
  - Resolved discoveries marked differently (dimmed or checkmark)
  - Blocked discoveries show lock icon
- [ ] Implement visit interaction:
  - Clicking a discovered location opens its interaction panel
  - For now, show placeholder text: "You visit [location name]. [Type] interaction coming in later stages."
  - Mark as resolved in DiscoveryDB

### Verification
- System overview shows correct data for the current system
- Search button discovers a new location each press, turn increments
- Discovery list updates in real-time as locations are found
- Search button disables when all locations discovered
- Visit shows placeholder interaction for each discovery type

---

## Phase 3.5 — Travel Flow

**Goal:** Implement the complete travel sequence: input destination, validate jump range, execute travel, handle intra-region pass-thru events (placeholder), arrive at new system.

### Tasks
- [ ] Create `scenes/ui/navigation_panel.tscn` (displays in console screen zone)
- [ ] Implement address input:
  - Text field for Q-R address entry
  - Validate format on input
  - Show system name if address is known
  - Show "Unknown System" if address is valid but unvisited
  - Show error if address is invalid or unpopulated
- [ ] Show jump info before confirming:
  - Distance (region hops)
  - Jump drive tier required
  - Estimated turns (1 per hop for intra-region, 1 for inter-region)
  - Warning if destination is in a higher danger zone
- [ ] Implement known address list:
  - Scrollable list of all known addresses with labels and source tags
  - Click to auto-fill the address input
  - Sort by: recent, distance, region, alphabetical
- [ ] Implement travel execution:
  - Validate jump range against ship's jump drive tier
  - For intra-region travel:
    - Each hop costs 1 turn
    - Each hop is a pass-thru event check point (placeholder — just advance turn for now)
    - Show brief "Jumping to hop X of Y..." transition
  - For inter-region travel:
    - Single jump, costs 1 turn
    - No pass-thru events
  - On arrival:
    - Generate system if not yet generated (from seed)
    - Store in DiscoveryDB if first visit
    - Apply alignment-based initial visibility
    - Update GameState.current_system and current_address
    - Add address to known addresses
    - Emit `EventBus.system_entered`
    - Update system overview display
    - Trigger auto-save
- [ ] Implement jump drive range feedback:
  - If destination is out of range, show "Jump drive Tier X required" message
  - Suggest which tier is needed

### Verification
- Can enter a valid address and travel to it
- Turn count increments correctly (1 per intra-region hop, 1 for inter-region)
- Jump drive tier correctly gates travel range
- Arriving at a new system generates it and shows system overview
- Known address list populates as systems are visited
- Invalid/out-of-range destinations show clear error messages
- Auto-save triggers on arrival

---

## Phase 3.6 — Starting Region Setup

**Goal:** Define the starting region (Q01-R0202 or equivalent safe zone region) with hand-crafted content for early game testing and eventual tutorial use.

### Tasks
- [ ] Define the starting region with a known hub system:
  - Hub at a fixed address (e.g., Earth at Q01-R0101-1000)
  - Hub type: Trade Hub (basic services available)
  - 3-5 sub-systems around the hub with varied tiers and alignments
- [ ] Pre-define key starting discoveries for the hub:
  - Merchant (known on arrival)
  - Repair service (known on arrival)
  - Mission board (known on arrival)
  - Additional discoveries to find via searching
- [ ] Define 1-2 neighboring regions with different specialties:
  - Ensures the player has trade route options in early game
- [ ] Set starting system for new game:
  - Player starts at a Minor system near the hub (not at the hub itself — tutorial has them travel there)
  - Starting system has a repair service and a delivery mission discovery
- [ ] Add these fixed systems to the galaxy generator (override seed for starting region)

### Verification
- New game starts at the designated Minor system
- Hub system has expected discoveries pre-generated
- Traveling from start to hub works within Tier 1 jump drive range
- Neighboring regions have different trade specialties

---

## Stage 03 Completion Checklist

- [ ] Turn system works — search and travel cost turns, visiting is free
- [ ] Discovery generation produces correct counts by tier with alignment-based visibility
- [ ] Scanner preview shows useful pre-search info
- [ ] Cockpit UI shell displays with functional zones and live dashboard
- [ ] System overview shows system data, discovery progress, and action buttons
- [ ] Discovery list updates as locations are found and resolved
- [ ] Navigation panel accepts addresses, validates range, and executes travel
- [ ] Known address list tracks visited systems
- [ ] Intra-region travel has correct hop count and turn cost
- [ ] Starting region is playable with hand-crafted content
- [ ] Auto-save triggers on system arrival

**You can now play the core loop:** arrive → search → visit → travel → repeat

**Next Stage:** [04 — Economy & Trading](04_economy_and_trading.md)
