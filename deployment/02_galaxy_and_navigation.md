# Stage 02: Galaxy & Navigation

## Goal
Build the galaxy generation system, Q-R address scheme, system population, and jump routing. By the end of this stage you can generate a full seeded galaxy, look up any system by address, and calculate jump paths between systems.

**Design Docs:** [02_planets.md](../docs/02_planets.md), [02a_galaxy_structure.md](../docs/02a_galaxy_structure.md), [02b_system_types.md](../docs/02b_system_types.md), [02c_discovery_system.md](../docs/02c_discovery_system.md)
**Prerequisites:** Stage 01 complete (data models, autoloads, EventBus)
**Outputs:** GalaxyGenerator, NavigationManager, DiscoveryDB shell, seeded galaxy with populated systems

---

## Phase 2.1 — Galaxy Grid and Address System

**Goal:** Implement the quadrant-region grid and Q-R address format so any location in the galaxy has a unique, parseable address.

### Tasks
- [ ] Create `scripts/systems/galaxy_generator.gd`
- [ ] Implement address parsing and validation:
  - Format: `Q[01-04]-R[xx][yy]-[system_number]`
  - Quadrants: Q01 (top-left), Q02 (top-right), Q03 (bottom-left), Q04 (bottom-right)
  - Region coordinates: xx (01-12), yy (01-12) per quadrant
  - Usable core: xx/yy 02-11 (10x10), outer ring = dead space border
  - System numbers: 4-digit, valid range defined by address rules (1st digit 1-4, 2nd even, 3rd odd, 4th any)
  - 1,000 valid addresses per region
- [ ] Implement `parse_address(address: String) -> Dictionary`:
  - Returns {quadrant, region_x, region_y, system_number} or null if invalid
- [ ] Implement `format_address(quadrant, region_x, region_y, system_number) -> String`
- [ ] Implement `is_valid_address(address: String) -> bool`
- [ ] Implement `get_region_address(address: String) -> String`:
  - Returns Q-R portion without system number
- [ ] Implement `get_quadrant(address: String) -> int`

### Verification
- `parse_address("Q01-R0305-1234")` returns correct components
- Invalid addresses (bad quadrant, out of range, wrong digit rules) are rejected
- Round-trip: format(parse(address)) == address

---

## Phase 2.2 — Manhattan Distance and Danger Zones

**Goal:** Calculate Manhattan distance from origin for any region and map it to danger zones.

### Tasks
- [ ] Implement `get_manhattan_distance(quadrant: int, region_x: int, region_y: int) -> int`:
  - Origin is at the corner where all 4 quadrants meet (0,0)
  - Convert region coordinates to absolute grid position relative to origin
  - Calculate Manhattan distance: |abs_x| + |abs_y|
- [ ] Implement `get_danger_zone(manhattan_distance: int) -> String`:
  - 1-3: "safe" (Space Force controlled)
  - 4-6: "low_medium" (pirate activity begins)
  - 7-9: "medium_high" (deep pirate territory)
  - 10+: "extreme" (edge of galaxy)
- [ ] Implement `get_quadrant_faction(quadrant: int) -> String`:
  - Q01: "silk_hand"
  - Q02: "phantom_circuit"
  - Q03: "rust_collective"
  - Q04: "void_reavers"
- [ ] Implement faction border blending for regions within 2 cells of quadrant borders:
  - Adjacent to border: 75% home / 25% neighbor
  - 2 cells from border: same quadrant faction only
  - On border: 50/50 split

### Verification
- Origin-adjacent regions (e.g., Q01-R0202) have distance 2 → "safe"
- Far corner regions have distance 18+ → "extreme"
- Border regions return correct faction blending ratios

---

## Phase 2.3 — System Generation

**Goal:** Procedurally generate populated systems for each region using the galaxy seed.

### Tasks
- [ ] Implement `generate_region(quadrant, region_x, region_y, seed) -> Array[SystemData]`:
  - Use seeded RNG (seed + region coordinates as hash)
  - Generate 200 populated systems out of 1,000 valid addresses (20% density)
  - For each populated system, determine:
    - **Population tier** — weighted by distance (closer = more Major/Moderate, farther = more Minor/Measly)
    - **Alignment** — weighted by distance and quadrant (closer = more Friendly/Neutral, farther = more Hostile/Pirate-Controlled/Dead)
    - **System name** — procedurally generated from seed
  - Assign discovery count based on tier:
    - Major: 12-18
    - Moderate: 7-11
    - Minor: 3-6
    - Measly: 3
- [ ] Implement system name generator:
  - Sci-fi style names: "Kepler-7", "Voss Prime", "NX-4821", "Arcadia Station"
  - Seeded so same seed always produces same names
- [ ] Implement `generate_system(address, seed) -> SystemData`:
  - Generate a single system on demand (for unvisited system lookups)
- [ ] Determine hub assignments:
  - 1 hub per region (highest tier Friendly system)
  - Hub type weighted by region characteristics
  - 3-5 sub-systems linked to hub

### Verification
- Same seed produces identical galaxy every time
- Region at distance 2 has mostly Friendly/Neutral systems with Major/Moderate tiers
- Region at distance 8 has mostly Hostile/Pirate-Controlled with Minor/Measly tiers
- 200 systems generated per region
- All generated addresses are valid per the address rules
- System names are readable and don't repeat within a region

---

## Phase 2.4 — Regional Economy Specialties

**Goal:** Assign economic specialties to each region so trade routes emerge naturally.

### Tasks
- [ ] Define trade good master list in `data/trade_goods.tres` or a GDScript constant:
  - 22 common goods with base prices (5-140 shards)
  - 22 rare goods with base prices (100-4,000 shards)
  - Each good has weight (1-15), category, description
- [ ] Implement `generate_region_specialties(quadrant, region_x, region_y, seed) -> Dictionary`:
  - Assign weighted specialty list for all trade goods
  - Top 25% become "dominant" (high supply, low local price)
  - Bottom 25% become "scarce" (low supply, high local price)
  - Middle 50% are normal
- [ ] Store specialties in SystemData or in a region-level data structure
- [ ] Ensure adjacent regions have complementary specialties to encourage trade routes

### Verification
- Each region has distinct dominant and scarce goods
- Nearby regions don't share the same dominant goods (or rarely do)
- A player could identify profitable routes by comparing region specialties

---

## Phase 2.5 — NavigationManager Singleton

**Goal:** Implement the navigation system that handles travel between systems, jump range checks, and path routing.

### Tasks
- [ ] Create `scripts/autoload/navigation_manager.gd`
- [ ] Implement `can_jump_to(from_address, to_address, jump_drive_tier) -> bool`:
  - Same region: always possible (intra-region travel)
  - Different region: check Manhattan distance between regions against jump drive range
    - Tier 1: same region only
    - Tier 2: adjacent regions (distance 1)
    - Tier 3: distance up to 3
    - Tier 4: distance up to 6
    - Tier 5: unlimited
- [ ] Implement `calculate_jump_path(from_address, to_address) -> Array[String]`:
  - For intra-region travel: direct path, 1 jump
  - For inter-region travel: calculate intermediate region hops
  - Each hop is a potential pass-thru event trigger point
  - Return array of intermediate addresses
- [ ] Implement `get_jump_count(from_address, to_address) -> int`
- [ ] Implement `get_region_distance(region_a, region_b) -> int`:
  - Manhattan distance between two region grid positions
- [ ] Implement `get_nearby_systems(address, radius) -> Array[String]`:
  - Return known systems within a certain jump range
- [ ] Register as Autoload

### Verification
- Tier 1 jump drive can only travel within same region
- Tier 5 can travel anywhere
- Jump path returns correct intermediate hops
- `get_jump_count()` matches path length

---

## Phase 2.6 — DiscoveryDB Singleton (Shell)

**Goal:** Create the persistent discovery database that tracks what the player has found in each system. Full discovery generation comes in Stage 03, but the storage layer needs to exist now.

### Tasks
- [ ] Create `scripts/autoload/discovery_db.gd`
- [ ] Implement storage structure:
  - Dictionary keyed by system address
  - Each entry holds the system's discovery array and visited state
- [ ] Implement `has_visited(address) -> bool`
- [ ] Implement `mark_visited(address)`
- [ ] Implement `get_discoveries(address) -> Array[Discovery]`
- [ ] Implement `set_discoveries(address, discoveries: Array[Discovery])`
- [ ] Implement `get_discovered_count(address) -> int` — how many the player has found
- [ ] Implement `get_total_count(address) -> int` — total discoverable locations
- [ ] Implement `mark_discovery_found(address, discovery_id)`
- [ ] Implement `mark_discovery_resolved(address, discovery_id)`
- [ ] Implement serialization methods for SaveManager integration
- [ ] Register as Autoload

### Verification
- Store discoveries for a test system, retrieve them correctly
- `mark_visited()` persists across lookups
- Serializes to Dictionary and deserializes without loss
- Discovery counts update when discoveries are marked found

---

## Phase 2.7 — Debug Galaxy Viewer

**Goal:** Create a temporary debug scene that visualizes the generated galaxy so you can verify generation is working correctly. This is a development tool, not a player-facing feature.

### Tasks
- [ ] Create `scenes/debug/galaxy_viewer.tscn`
- [ ] Display a grid of regions colored by danger zone
- [ ] Click a region to see its populated systems listed
- [ ] Click a system to see its SystemData (tier, alignment, name, discovery count)
- [ ] Show faction territory colors (one color per pirate faction)
- [ ] Display Manhattan distance numbers on each region cell
- [ ] Show trade specialty summary per region
- [ ] Test with multiple seeds to verify variety

### Verification
- Galaxy visually makes sense: safe center, danger increasing outward
- Faction territories align with quadrants
- Border blending visible at quadrant edges
- System counts look correct (~200 per region)
- Different seeds produce different galaxies

---

## Stage 02 Completion Checklist

- [ ] Q-R address system parses, validates, and formats correctly
- [ ] Manhattan distance calculates correctly for all regions
- [ ] Galaxy generates 200 systems per region with correct tier/alignment weighting
- [ ] System names generate consistently from seed
- [ ] Regional economy specialties assigned with complementary trade routes
- [ ] NavigationManager handles jump range, path routing, and hop counts
- [ ] DiscoveryDB stores and retrieves discovery state
- [ ] Debug galaxy viewer confirms generation visually
- [ ] Same seed always produces identical galaxy

**Next Stage:** [03 — Core Game Loop](03_core_game_loop.md)
