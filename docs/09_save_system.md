# Save and Load System

## Overview
This document defines how game state is persisted, loaded, and managed. The save system must capture the **persistent discovery database**, dynamic economy state, entity states, and all player progress.

---

## Save Data

### What Gets Saved
- **Pilot state** — Name, portrait, background, skills (0-10 per skill with XP progress), fame level, fame points allocated, shards balance
- **Reputation** — Per legitimate faction (5 tiers: Hated/Unfriendly/Neutral/Friendly/Allied)
- **Pirate fame** — Per pirate faction (Void Reavers, Silk Hand, Rust Collective, Phantom Circuit)
- **Current ship** — Class, size, name, hull integrity, shield state, components with upgrade tiers, cargo contents (regular + special slots)
- **Owned ships** — Up to 4 stored ships with class, components, location (hub address), and any in-transit status with delivery turn
- **Loaner state** — Whether player is in a loaner shuttle (death recovery)
- **Abandoned ship** — Address where ship was lost, remaining cargo, loot percentage taken
- **Current location** — XX-XXXX address of current system
- **Last hub location** — XX-XXXX address of last visited hub (respawn point)
- **Star date** — Current game star date (used for economy calculations)
- **Turn count** — Background game clock
- **Galaxy seed** — Master seed for procedural generation of unvisited systems
- **Discovery database** — All discovered locations per visited system, persistent across visits
- **System states** — Per visited system: type, sub-type, hostility status, controlling faction, cleared status, boss warning state, outposts cleared count, departure star date, generated economy data
- **Economy state** — Per visited system: current prices, stock levels, supply/demand values, last visit star date
- **Known addresses** — All system addresses the player has learned (source tagged: exploration, NPC, star chart, mission, bulletin board)
- **Active missions** — Current quests with objectives, destination addresses, deadline turns (if applicable), issuing faction
- **Completed missions** — Quest history
- **Story progress** — Main story flags, tutorial completion, act progression
- **Player stations** — Up to 3 stations: address, region, tier level, stored cargo, upgrade state
- **Ship transfers in transit** — Ships being moved between hubs: origin, destination, delivery turn
- **Derelicts being towed** — Derelict ships en route to hubs: origin, destination, delivery turn
- **Black market contacts** — Known dealers, locations, how they were found
- **Boss states** — Per hostile system: outposts cleared, boss warned, boss rebuild timer
- **Journal data** — Visited systems list, last known prices per system, regional goods overview, statistics
- **NPC relationship flags** — Side mission completion states that unlock black market referrals and other contacts
- **World event state** — Active events, system hostility changes, faction war status, pirate turf war zones

### What Does NOT Get Saved
- Static game data (loaded from Godot Resource files at startup)
- Generated content for **unvisited systems** (regenerated from galaxy seed)
- UI state (menu positions, scroll positions)
- Temporary calculations
- Audio playback state
- Loaner ship data (temporary, not tracked)

---

## Save Format

### File Format
- **Development**: JSON via `user://saves/` directory — human-readable, easy to debug
- **Release**: Consider compressed JSON or binary format for smaller files
- All saves include a **version field** for migration support

### File Structure
```json
{
  "version": "1.0",
  "timestamp": "2026-04-01T12:00:00",
  "play_time_seconds": 14400,
  "galaxy_seed": 8472910,
  "star_date": 482,
  "turn_count": 142,

  "pilot": {
    "name": "Kira Voss",
    "portrait_id": 7,
    "background": "mechanic",
    "shards": 28500,
    "fame_level": 4,
    "fame_points_allocated": { "trading": 1, "combat": 2, "exploration": 1 },
    "skills": {
      "trading": { "level": 3, "xp": 450 },
      "piloting": { "level": 2, "xp": 210 },
      "engineering": { "level": 5, "xp": 980 },
      "negotiation": { "level": 2, "xp": 175 },
      "combat": { "level": 4, "xp": 720 },
      "exploration": { "level": 4, "xp": 690 }
    },
    "reputation": {
      "faction_alpha": "friendly",
      "faction_beta": "neutral",
      "faction_gamma": "unfriendly"
    },
    "pirate_fame": {
      "void_reavers": 3,
      "silk_hand": 6,
      "rust_collective": 1,
      "phantom_circuit": 4
    }
  },

  "current_ship": {
    "class": "corvette",
    "size": "medium",
    "name": "The Drifter",
    "hull_current": 65,
    "hull_max": 100,
    "shield_current": 40,
    "shield_max": 50,
    "components": {
      "jump_drive": 3,
      "hull_plating": 3,
      "shields": 2,
      "cargo_bay": 3,
      "scanner": 3,
      "weapons": 3,
      "engine": 2,
      "nav_computer": 3
    },
    "cargo": [
      { "good": "refined_metals", "quantity": 45, "type": "common" },
      { "good": "rare_crystals", "quantity": 8, "type": "rare" }
    ],
    "special_slots": [
      { "slot": 1, "item": "mission_package_042" },
      { "slot": 2, "item": null }
    ]
  },

  "owned_ships": [
    {
      "class": "hauler",
      "size": "large",
      "name": "Big Bertha",
      "stored_at": "08-0421",
      "hull_current": 120,
      "hull_max": 150,
      "components": { "jump_drive": 2, "hull_plating": 4, "shields": 2, "cargo_bay": 5, "scanner": 2, "weapons": 1, "engine": 1, "nav_computer": 3 },
      "cargo": [],
      "in_transit": false
    },
    {
      "class": "gunship",
      "size": "small",
      "name": "Fang",
      "stored_at": null,
      "hull_current": 80,
      "hull_max": 80,
      "components": { "jump_drive": 2, "hull_plating": 3, "shields": 3, "cargo_bay": 1, "scanner": 2, "weapons": 3, "engine": 3, "nav_computer": 1 },
      "cargo": [],
      "in_transit": true,
      "transit_origin": "08-0421",
      "transit_destination": "14-1102",
      "transit_arrival_turn": 156
    }
  ],

  "recovery_state": {
    "is_in_loaner": false,
    "abandoned_ship_address": null,
    "abandoned_ship_data": null,
    "abandoned_cargo": [],
    "loot_percentage": 0
  },

  "location": {
    "current_address": "08-0421",
    "last_hub_address": "08-0100"
  },

  "discovery_database": {
    "08-0421": {
      "system_name": "Voss Prime",
      "system_type": "friendly",
      "sub_type": "mining_colony",
      "total_locations": 4,
      "discovered_locations": [
        { "id": "merchant_01", "type": "merchant", "name": "Riker's Trading Post" },
        { "id": "resource_01", "type": "resource_deposit", "name": "Deep Ore Vein" },
        { "id": "danger_01", "type": "hostile_wildlife", "name": "Tunnel Beasts" }
      ],
      "hostility": "friendly",
      "cleared": false
    },
    "12-0887": {
      "system_name": "Ashfall",
      "system_type": "hostile",
      "sub_type": "gang_controlled",
      "total_locations": 4,
      "discovered_locations": [
        { "id": "outpost_01", "type": "enemy_outpost", "name": "Reaver Outpost Alpha", "cleared": true },
        { "id": "outpost_02", "type": "enemy_outpost", "name": "Reaver Outpost Beta", "cleared": false }
      ],
      "hostility": "hostile",
      "controlling_faction": "void_reavers",
      "cleared": false,
      "outposts_cleared": 1,
      "outposts_required": 3,
      "boss_warned": false,
      "boss_rebuild_timer": null
    }
  },

  "economy": {
    "08-0421": {
      "last_visit_star_date": 480,
      "departure_star_date": 482,
      "prices": {
        "raw_ores_minerals": 32,
        "refined_metals": 58,
        "food_rations": 85,
        "medical_supplies": 120
      },
      "stock": {
        "raw_ores_minerals": 850,
        "refined_metals": 320,
        "food_rations": 45,
        "medical_supplies": 22
      },
      "goods_available": ["raw_ores_minerals", "refined_metals", "food_rations", "medical_supplies", "mechanical_parts", "recycled_scrap", "fuel"]
    }
  },

  "known_addresses": [
    { "address": "00-0001", "name": "Tutorial Start", "source": "exploration", "system_type": "friendly" },
    { "address": "00-0100", "name": "Haven Station", "source": "exploration", "system_type": "friendly" },
    { "address": "08-0421", "name": "Voss Prime", "source": "npc_tip", "system_type": "friendly" },
    { "address": "08-0100", "name": "Irongate Hub", "source": "bulletin_board", "system_type": "friendly" },
    { "address": "12-0887", "name": "Ashfall", "source": "star_chart", "system_type": "hostile" },
    { "address": "14-1102", "name": null, "source": "star_chart", "system_type": "unknown" }
  ],

  "missions": {
    "active": [
      {
        "id": "mission_042",
        "type": "delivery",
        "title": "Emergency Medical Run",
        "source_faction": "faction_alpha",
        "source_type": "faction",
        "destination": "14-1102",
        "objective": "Deliver 20 medical supplies",
        "reward_shards": 3500,
        "deadline_turn": 160,
        "progress": "in_transit"
      }
    ],
    "completed": [
      {
        "id": "mission_001",
        "type": "delivery",
        "title": "Tutorial Delivery",
        "completed_turn": 8
      }
    ]
  },

  "story_flags": {
    "tutorial_complete": true,
    "tutorial_ship_repaired": true,
    "tutorial_cargo_delivered": true,
    "freighter_awarded": true,
    "act_1_started": true,
    "first_hostile_warning_received": true
  },

  "player_stations": [
    {
      "address": "22-4481",
      "region": 22,
      "name": "Outpost Alpha",
      "tier": 1,
      "stored_cargo": [
        { "good": "rare_crystals", "quantity": 15, "type": "rare" }
      ]
    }
  ],

  "black_market_contacts": [
    {
      "id": "dealer_01",
      "location": "12-0887",
      "found_via": "search",
      "faction": "void_reavers"
    },
    {
      "id": "dealer_02",
      "location": "08-0421",
      "found_via": "npc_referral",
      "referring_npc": "merchant_riker"
    }
  ],

  "journal": {
    "visited_systems": ["00-0001", "00-0100", "08-0421", "08-0100", "12-0887"],
    "last_known_prices": {
      "08-0421": { "raw_ores_minerals": 32, "refined_metals": 58, "food_rations": 85 },
      "08-0100": { "refined_metals": 95, "manufactured_components": 110, "food_rations": 42 }
    },
    "regional_goods_overview": {
      "00": { "common_goods": ["food_rations", "agricultural_supplies", "water_ice"], "hint": "Agricultural region — food is cheap, technology is scarce" },
      "08": { "common_goods": ["raw_ores_minerals", "refined_metals", "mechanical_parts"], "hint": "Mining region — raw materials abundant, food in demand" }
    },
    "statistics": {
      "total_shards_earned": 42000,
      "total_shards_spent": 13500,
      "systems_visited": 5,
      "discoveries_made": 12,
      "enemies_defeated": 8,
      "hostile_systems_cleared": 0,
      "missions_completed": 1,
      "missions_failed": 0,
      "black_market_deals": 3,
      "deaths": 0,
      "haggle_successes": 4,
      "haggle_failures": 2
    }
  },

  "world_events": {
    "active_events": [
      {
        "type": "faction_war",
        "factions": ["faction_alpha", "faction_gamma"],
        "affected_regions": [8, 12, 14],
        "started_turn": 100,
        "effects": "weapons and medicine prices +80% in affected regions"
      }
    ],
    "pirate_turf_wars": [
      {
        "factions": ["void_reavers", "silk_hand"],
        "border_regions": [10, 11],
        "intensity": "high"
      }
    ]
  }
}
```

---

## Save Slots

### Configuration
- **3 manual save slots** + **1 dedicated auto-save slot**
- Auto-save never overwrites manual saves
- Manual saves never overwrite auto-save

### Save Slot Preview
Each slot displays:
- Pilot name
- Play time (real-world hours:minutes)
- Current system address and name

### Slot Naming
- Manual slots labeled: Save 1, Save 2, Save 3
- Auto-save labeled: Auto-Save
- Preview info updates each time the slot is written

---

## Save/Load Triggers

### Manual Save
- Available **only when docked** at a system
- Appears as an option in the system action menu alongside Search, Visit, Travel
- Player selects which of the 3 manual slots to save to
- Overwrites the existing save in that slot (with backup)

### Auto-Save
- Triggers on the following events:
  - **Arriving at a new system**
  - **Completing a mission**
  - **Winning combat**
  - **Clearing a hostile system**
  - **Story progression moments**
  - **Player death** (saves recovery state)
- Always writes to the dedicated auto-save slot
- Serves as crash recovery fallback

### Loading
- Load available from the **main menu** and the **pause menu**
- Player can load any of the 3 manual slots or the auto-save
- Loading replaces current game state entirely

---

## Save-Scumming Policy

### Death and Saves
- On player death, the auto-save records the **recovery state** (loaner ship, abandoned ship location, etc.)
- Manual saves are **preserved** — the player can reload a pre-death manual save to avoid death consequences
- This is intentionally allowed for a more **casual-friendly** experience
- The game does **not** overwrite manual saves on death

---

## Procedural Generation and Saves

### Hybrid Approach
- **Galaxy seed** is saved — used to regenerate unvisited systems on demand
- **Visited systems** save all generated data (economy, discoveries, stock, events)
- Unvisited systems are **not stored** — regenerated from the seed when needed
- This keeps save file size manageable while preserving exact state for systems the player has interacted with
- Requires **deterministic generation** — same seed + same address must always produce the same base system

### What the Seed Generates
- Whether an address is populated or empty space
- System type (friendly, dead, hostile)
- Sub-type
- System name
- Number of discoverable locations
- Base economy (goods selection, initial stock, initial prices)
- Hub specialization (for hub systems)
- Cluster membership

### What Gets Saved Over the Seed
- Any player-modified state (discovered locations, economy changes, cleared status)
- Once a system is visited, its generated data is saved and the seed is no longer used for that system

---

## Save File Location
- Godot's `user://saves/` directory
- Cross-platform: Godot handles OS-appropriate paths automatically
  - Windows: `%APPDATA%/Godot/app_userdata/SpaceGame/saves/`
  - macOS: `~/Library/Application Support/Godot/app_userdata/SpaceGame/saves/`
  - Linux: `~/.local/share/godot/app_userdata/SpaceGame/saves/`

---

## Version Compatibility
- Save files include a **version field**
- Migration functions handle saves from older game versions
- Strategy: **add new fields with sensible defaults** rather than breaking the format
- Major version changes may require save migration scripts
- Version field checked on load — incompatible saves display a warning

---

## Error Handling
- Validate save file integrity on load (structural validation of required fields)
- Corrupted save detection with user-friendly error message
- Keep **backup of previous save** when overwriting (`.bak` file per slot)
- Recovery from failed saves — don't delete old save until new save is confirmed written to disk
- Auto-save provides a fallback if manual save is corrupted
- If all saves are corrupted, player can start a new game (no silent data loss)

---

## Notes
_Add design notes, open questions, and decisions here._

### Open Design Questions
- Save file size optimization — at what point do visited systems need compression?
- Maximum number of visited systems before performance concerns
- Cloud save support (future consideration)
- Save file encryption for anti-cheat (future consideration)
- Exact deterministic generation requirements for seed-based regeneration
