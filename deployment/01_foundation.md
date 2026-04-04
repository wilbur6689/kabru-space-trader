# Stage 01: Foundation

## Goal
Set up the Godot 4.3+ project structure, create all core autoload singletons, define the data model resources, and establish the event bus. By the end of this stage the project runs cleanly with all singletons loaded and data models instantiable.

**Design Docs:** [06_technical.md](../docs/06_technical.md), [09_save_system.md](../docs/09_save_system.md)
**Prerequisites:** None — this is the starting point
**Outputs:** Project skeleton, autoloads, data models, EventBus, SaveManager shell

---

## Phase 1.1 — Project Setup

**Goal:** Create the Godot project with the correct folder structure and settings.

### Tasks
- [ ] Create new Godot 4.3+ project
- [ ] Set up folder structure:
  ```
  SpaceGame/
  ├── assets/
  │   ├── sprites/
  │   ├── audio/
  │   ├── fonts/
  │   └── shaders/
  ├── scenes/
  │   ├── main/
  │   ├── game/
  │   ├── ui/
  │   └── combat/
  ├── scripts/
  │   ├── autoload/
  │   ├── models/
  │   ├── systems/
  │   └── ui/
  ├── data/
  └── addons/
  ```
- [ ] Configure project settings (window size, stretch mode, default theme)
- [ ] Import fonts (Orbitron for headers, Inconsolata for body)
- [ ] Set up base color palette as a Theme resource:
  - Background: `#0a0e1a`
  - Panels: `#1a1e2e`
  - Text: `#d0d0d0`
  - Cyan headers: `#00ccff`
  - Green currency: `#00ff88`
  - Red damage: `#ff3344`
- [ ] Create a placeholder main scene that loads on startup

### Verification
- Project opens in Godot editor without errors
- Main scene runs and displays a styled placeholder screen with correct fonts and colors

---

## Phase 1.2 — EventBus Singleton

**Goal:** Create the global signal bus that all systems will use to communicate.

### Tasks
- [ ] Create `scripts/autoload/event_bus.gd`
- [ ] Define core signals (these will expand as systems are built):
  ```gdscript
  # Turn signals
  signal turn_started(turn_number: int)
  signal turn_ended(turn_number: int)
  
  # Navigation signals
  signal system_entered(system_data: SystemData)
  signal travel_started(from_address: String, to_address: String)
  signal travel_completed(system_data: SystemData)
  
  # Discovery signals
  signal search_started()
  signal discovery_found(discovery: Discovery)
  signal discovery_visited(discovery: Discovery)
  
  # Economy signals
  signal trade_completed(good_name: String, quantity: int, total_price: int, is_buy: bool)
  
  # Combat signals
  signal combat_started(enemy: MasterEntity)
  signal combat_ended(result: String)
  
  # UI signals
  signal screen_changed(screen_name: String)
  signal notification_queued(message: String, type: String)
  ```
- [ ] Register as Autoload in Project Settings

### Verification
- Autoload appears in the running scene tree
- Test signals can be emitted and received from a debug script

---

## Phase 1.3 — Data Model Resources

**Goal:** Define all core data structures as Godot Resource classes. These are the data containers that every system reads and writes.

### Tasks
- [ ] Create `scripts/models/pilot.gd` (Resource):
  - `name: String`
  - `background: String`
  - `skills: Dictionary` — {trading, piloting, engineering, negotiation, combat, exploration} each with level (0-10) and xp (int)
  - `fame_level: int` (0-12)
  - `fame_points: Dictionary` — allocated points per skill
  - `faction_reputation: Dictionary` — per faction name → tier (0-4)
  - `pirate_fame: Dictionary` — per pirate faction → level
  - `shards: int`
  - `current_address: String`
  - `respawn_address: String`
  - `known_addresses: Array[Dictionary]` — {address, source, label}
  - `journal: Dictionary` — visited systems, price history, statistics

- [ ] Create `scripts/models/ship.gd` (Resource):
  - `ship_name: String`
  - `ship_class: String`
  - `size: String` (small/medium/large)
  - `is_black_market: bool`
  - `components: Dictionary` — {jump_drive, hull, shields, cargo, scanner, weapons, engine, nav_computer} each with tier (1-5) and current state
  - `hull_current: int`, `hull_max: int`
  - `shield_current: int`, `shield_max: int`
  - `cargo_hold: Array[Dictionary]` — {good_name, quantity, buy_price}
  - `special_slots: Array[Dictionary]` — mission items, station modules, contraband
  - `storage_location: String` — hub address where stored (empty if active)

- [ ] Create `scripts/models/system_data.gd` (Resource):
  - `address: String` (Q-R format)
  - `system_name: String`
  - `population_tier: String` (major/moderate/minor/measly)
  - `alignment: String` (friendly/neutral/hostile/pirate_controlled/dead)
  - `hub_type: String` (trade/military/tech/shipyard/outlaw/science or empty)
  - `manhattan_distance: int`
  - `danger_zone: String`
  - `discovery_count: int`
  - `discoveries: Array[Discovery]`
  - `is_visited: bool`
  - `economy_specialties: Array[String]`

- [ ] Create `scripts/models/discovery.gd` (Resource):
  - `discovery_id: String`
  - `category: String` (service/loot/encounter/passive)
  - `type: String` (merchant, repair, cache, pirate_hideout, etc.)
  - `name: String`
  - `is_discovered: bool`
  - `is_resolved: bool`
  - `is_blocking: bool` — whether this threat blocks other discoveries
  - `blocked_by: String` — discovery_id of blocking threat
  - `data: Dictionary` — type-specific data (prices, loot, enemy stats, etc.)

- [ ] Create `scripts/models/master_entity.gd` (Resource):
  - `entity_name: String`
  - `entity_type: String` (player/npc/enemy/resource/event)
  - `fame_level: int`
  - `location_address: String`
  - `base_stats: Dictionary` — {hull, shields, damage, accuracy, defense, flee_chance}
  - `faction: String`
  - `loot_table: Array[Dictionary]`

- [ ] Create `scripts/models/mission.gd` (Resource):
  - `mission_id: String`
  - `mission_type: String` (delivery/smuggling/bounty/rescue/exploration/clearance/investigation)
  - `title: String`
  - `description: String`
  - `source_faction: String`
  - `origin_address: String`
  - `destination_address: String`
  - `reward_shards: int`
  - `reward_reputation: Dictionary`
  - `cargo_required: Dictionary` — for delivery/smuggling missions
  - `target_entity: String` — for bounty missions
  - `deadline_turns: int` — 0 means no deadline
  - `turns_remaining: int`
  - `status: String` (available/active/completed/failed/abandoned)

- [ ] Create `scripts/models/trade_good.gd` (Resource):
  - `good_name: String`
  - `category: String` (common/rare)
  - `base_price: int`
  - `weight: int` (1-15)
  - `source_regions: Array[String]` — for rare goods
  - `description: String`

- [ ] Create `scripts/models/station.gd` (Resource):
  - `station_id: String`
  - `address: String`
  - `tier: int` (1-3)
  - `storage: Array[Dictionary]` — stored cargo
  - `is_under_raid: bool`
  - `raid_timer: int`

### Verification
- Each Resource can be instantiated in a test script without errors
- Resources can be serialized to Dictionary and back (needed for save system)
- Print a test Pilot, Ship, and SystemData to confirm all fields populate correctly

---

## Phase 1.4 — GameState Singleton

**Goal:** Create the central game state manager that holds the current pilot, active ship, turn counter, and star date.

### Tasks
- [ ] Create `scripts/autoload/game_state.gd`
- [ ] Properties:
  - `pilot: Pilot` — the current player pilot
  - `active_ship: Ship` — the ship the player is currently flying
  - `owned_ships: Array[Ship]` — up to 5 ships
  - `turn_count: int`
  - `star_date: Dictionary` — {year, month, day} (cosmetic)
  - `current_system: SystemData`
  - `galaxy_seed: int`
  - `is_tutorial_complete: bool`
  - `story_flags: Dictionary`
  - `active_missions: Array[Mission]`
  - `completed_missions: Array[String]` — mission IDs
  - `player_stations: Array[Station]`
  - `world_events: Array[Dictionary]`
- [ ] Methods:
  - `new_game(pilot_name, background)` — initialize fresh game state
  - `advance_turn()` — increment turn, emit signal, trigger world reactions
  - `get_current_address() -> String`
  - `add_known_address(address, source, label)`
  - `is_address_known(address) -> bool`
- [ ] Register as Autoload

### Verification
- Call `new_game()` from a debug script — confirm pilot, ship, and turn state initialize
- Call `advance_turn()` — confirm turn increments and signal fires
- Access `GameState.pilot` and `GameState.active_ship` from another script

---

## Phase 1.5 — SaveManager Singleton

**Goal:** Create the save/load system shell that can serialize and deserialize the full game state to JSON files.

### Tasks
- [ ] Create `scripts/autoload/save_manager.gd`
- [ ] Define save file path: `user://saves/`
- [ ] Define save structure:
  ```
  save_slot_1.json
  save_slot_2.json
  save_slot_3.json
  auto_save.json
  ```
- [ ] Implement `save_game(slot: int)`:
  - Serialize GameState to Dictionary
  - Include version field for future migration
  - Write JSON to file
- [ ] Implement `load_game(slot: int)`:
  - Read JSON from file
  - Deserialize into GameState
  - Validate version compatibility
- [ ] Implement `auto_save()`:
  - Save to auto_save slot
  - Connect to EventBus signals for auto-save triggers (system_entered, combat_ended, etc.)
- [ ] Implement `get_save_info(slot: int) -> Dictionary`:
  - Return metadata (pilot name, star date, turn count, address) without loading full save
- [ ] Implement `delete_save(slot: int)`
- [ ] Register as Autoload

### Verification
- Create a new game, save to slot 1, modify state, load from slot 1 — state restores correctly
- Auto-save triggers on system entry
- Save files appear in `user://saves/` as valid JSON
- `get_save_info()` returns correct metadata

---

## Phase 1.6 — AudioManager Singleton (Shell)

**Goal:** Create the audio manager shell that will handle music and SFX playback. Actual audio content comes in Stage 10, but the system needs to exist early so other systems can call it.

### Tasks
- [ ] Create `scripts/autoload/audio_manager.gd`
- [ ] Create AudioStreamPlayer nodes for:
  - `music_player` — background music
  - `sfx_player` — sound effects (or use a pool of players for overlapping SFX)
- [ ] Implement stub methods:
  - `play_music(track_name: String, crossfade: bool = true)`
  - `stop_music(fade_out: float = 1.0)`
  - `play_sfx(sfx_name: String)`
  - `set_music_volume(volume: float)`
  - `set_sfx_volume(volume: float)`
- [ ] For now, methods log to console instead of playing audio
- [ ] Register as Autoload

### Verification
- Calling `AudioManager.play_music("test")` logs to console without errors
- Volume methods work without crashing

---

## Stage 01 Completion Checklist

- [ ] Project runs with all 4 autoloads (EventBus, GameState, SaveManager, AudioManager)
- [ ] All 8 Resource models can be instantiated and serialized
- [ ] `new_game()` creates valid initial state
- [ ] Save/load round-trips without data loss
- [ ] EventBus signals fire and are receivable
- [ ] Folder structure is clean and organized
- [ ] No editor warnings or errors

**Next Stage:** [02 — Galaxy & Navigation](02_galaxy_and_navigation.md)
