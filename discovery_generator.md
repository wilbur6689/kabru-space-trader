# Discovery Generator — Reference Document

> This document is the complete specification for an external content generator program that produces discovery content variants for the **Ace Trader** game. It is intended to be self-contained: a developer (or AI assistant) reading this file should be able to build the generator program without needing access to the game's source code.

---

## 1. Project Context

### What is Ace Trader?

Ace Trader is a turn-based space trading / exploration / adventure game built in **Godot 4.6** with **GDScript**. The player pilots a ship through a massive procedurally-generated galaxy organized as **4 quadrants × 12×12 regions × 1000 addresses per region**, addressed via a Quadrant-Region (Q-R) system (e.g. `Q01-R0101-1000`).

### Core gameplay loop

1. **Arrive** at a star system → see its tier (Major→Measly), alignment (Friendly/Neutral/Hostile/Pirate/Dead), and discovery progress (`X / Y`).
2. **Search** the system (1 turn per search) → reveal a Discovery: a service, loot stash, encounter, hazard, or passive landmark.
3. **Visit** discovered locations to trade, repair, take missions, fight, scavenge, or experience flavor.
4. **Travel** to a new Q-R address; build wealth → upgrade ship → unlock harder, more lucrative regions.
5. **Clear pirate-controlled systems** to liberate the galaxy and progress the story.

### Where discoveries fit

Discoveries are the **content of every system**. Each system has between 0 and ~12 discoveries hidden (or partially pre-revealed) on arrival. The number, category mix, and visibility are driven by the system's alignment and tier. **Right now, every discovery of a given type is identical** — every "Cache" in the galaxy is just the string `"cache"` with the auto-label `"Cache"` and an empty data dict. This is what the generator program will fix.

### Goal of the generator program

Produce a large library of **per-type content variants** that the game can ingest. Each variant gives a single discovery type (e.g. `merchant`, `cache`, `pirate_hideout`) a unique:

- **Name** (e.g. `"Vex's Trading Post"` instead of `"Merchant"`)
- **Flavor text / lore / backstory**
- **Type-specific gameplay parameters** (faction, prices, loot table, skill check difficulty, NPC names, etc.)

The generator works **one discovery at a time**: the operator picks a type, optionally inputs aspects/themes, the program builds a Claude AI prompt and asks Claude to fill in the specified fields, then stores the result as a JSON variant in a content pack. The game loads these packs at startup and randomly assigns variants to discovery instances at generation time.

---

## 2. Canonical Data Model

### `Discovery` (the model the game uses)

Every discovery — service, loot, encounter, passive, hazard — uses **the same Resource class**. There are no per-type subclasses. Variation lives entirely in `data: Dictionary`.

```gdscript
class_name Discovery
extends Resource

@export var discovery_id: String = ""    # unique within a system, e.g. "Q01-R0101-0500_3"
@export var category: String = ""        # service | loot | encounter | passive | hazard
@export var type: String = ""            # merchant, cache, pirate_hideout, beacon, ...
@export var name: String = ""            # display name (variant fills this)
@export var is_discovered: bool = false  # has the player found it via search?
@export var is_resolved: bool = false    # has the player completed/visited it?
@export var is_blocking: bool = false    # blocks adjacent loot until resolved
@export var blocked_by: String = ""      # discovery_id of the blocker (if any)
@export var data: Dictionary = {}        # type-specific payload — variant fills this
```

The `data` dict is **the entire surface area for variant content**. It is JSON-serialized via `to_dict()` / `from_dict()` and round-trips intact through the save system. The generator writes into this dict.

### How the game will consume a variant

At system generation time, the game picks a category for each discovery slot (weighted by alignment), picks a unique type within that category, then — under the new system — looks up that type in the variant database and **rolls a random variant**, copying its `name` and `data` onto the new Discovery. The variant id is stored inside `data["variant_id"]` so saves are reproducible.

---

## 3. Reference Data (use these exact strings)

### Skills (6 total)

Used for skill checks in hazards, loot retrieval, negotiation, etc. Use these exact lowercase strings:

- `trading`
- `piloting`
- `engineering`
- `negotiation`
- `combat`
- `exploration`

The game checks `pilot.get_skill_level(skill_name)` (returns 0–10) and rolls `randi() % 100 + skill_lvl * 6 >= difficulty`.

### Factions

**Legitimate factions** (tracked via reputation tier 0–4):

| Faction id | Description |
|---|---|
| `space_force` | Military / patrol authority |
| `merchant_guild` | Traders and commerce |
| `explorers_union` | Scientists and researchers |
| `shipwrights_consortium` | Shipyard operators |

**Pirate factions** (tracked via fame level 0–10+, one per quadrant, increasing difficulty):

| Faction id | Quadrant | Personality |
|---|---|---|
| `silk_hand` | Q01 | Negotiable, lower stats. Sophisticated, cunning. |
| `phantom_circuit` | Q02 | Evasive, high accuracy/flee. Tech-focused, ghostlike. |
| `rust_collective` | Q03 | Tanky, high hull/shields, no flee. Industrial, brutal. |
| `void_reavers` | Q04 | Aggressive, high damage, cannot negotiate. Nihilistic, ruthless. |

### Population tiers

- `major` — biggest hubs, 1.10× economy, 4–5 base missions
- `moderate` — solid towns, 1.00× economy, 3–4 base missions
- `minor` — small outposts, 0.92× economy, 2–3 base missions
- `measly` — tiny stops, 0.85× economy, 1–2 base missions

### Alignments

- `friendly` — Space Force patrols, reveals 3 services on arrival, mostly services & passive
- `neutral` — balanced mix, reveals 3 services on arrival
- `hostile` — nothing pre-revealed, mostly encounters & hazards
- `pirate_controlled` — nothing pre-revealed, encounter-heavy + outpost injections + Warlord
- `dead` — no services, loot/passive/hazard only

### Danger zones (Manhattan distance from origin)

- `safe` — distance 1–3, petty pirates
- `low_medium` — distance 4–6, gangs and captains
- `medium_high` — distance 7–9, captains and hunters
- `extreme` — distance 10+, fleets, aliens, warlords

### System hub types (when set)

`trade`, `military`, `tech`, `shipyard`, `outlaw`, `science`, or `""` (no special hub).

### Currency

- **Shards** — single currency for all transactions. Variant fields with costs/rewards should be integers in shards.

### Trade goods (use exact display names if a variant references one)

Defined in `scripts/data/trade_goods_db.gd`. **22 common goods** (base price 15–130 shards) and **22 rare goods** (base price 450–3900 shards).

**Common goods:** Food & Rations, Water & Ice, Medical Supplies, Raw Ores & Minerals, Refined Metals, Industrial Materials, Manufactured Components, Consumer Goods, Basic Electronics, Fuel, Energy Cells & Batteries, Construction Materials, Agricultural Supplies, Textiles & Fibers, Mechanical Parts, Ship Maintenance Supplies, Recycled Scrap, Chemical Compounds, Atmospheric Gases, Terraforming Supplies, Data Storage Devices, Civilian Tools & Equipment.

**Rare goods:** Luxury Items, Weapons & Ammunition, Ancient Artifacts, Alien Relics, Experimental Technology, Black Market Cybernetics, Prototype Ship Components, High-End AI Cores, Quantum Processors, Stellar Cartography Data, Genetic Enhancements, Designer Drugs & Stimulants, Cultural Treasures, Rare Crystals & Exotic Matter, Antimatter Containers, Pre-Collapse Technology, Forbidden Research Data, Smuggled Goods, Sentient AI Fragments, Psionic Amplifiers, Alien Flora & Fauna Specimens, Timeworn Relics.

### Ships (for shipyard / outfitter variants)

**Standard ships** (`ships_db.gd`): `shuttle`, `wayfarer_s`, `drifter_s`, `lance_s`, `trader_m`, `scout_m`, `frigate_m`, `freighter_l`, `destroyer_l`, `battleship_l`.

**Black market ships:** `phantom_s`, `viper_m`, `ghost_m`, `raider_l`, `leviathan_l`, `warbringer_l`.

### Mission types (for mission_board variants)

`delivery`, `smuggling`, `bounty`, `rescue`, `exploration`, `clearance`, `investigation`.

### Difficulty class scale

Used by `difficulty_class` / `*_difficulty` fields. Game compares `pilot_skill * 6 + d100 >= difficulty`.

- `10–20` — easy (no real threat)
- `30–50` — moderate (risky)
- `60–80` — hard (dangerous)
- `90–100+` — extreme (deadly)

---

## 4. Discovery Type Catalog

There are **31 base types** plus **3 special injected types**, organized into 5 categories. Each entry below specifies the schema the generator must produce in `Discovery.data`. Fields marked **(req)** are required for the variant to be usable; everything else is optional flavor.

### 4.1 Service discoveries (9 types)

Services let the player spend turns/shards for benefits. They are typically pre-revealed in friendly/neutral systems. All services share these recommended optional fields: `description` (atmosphere/flavor text), `npc_name` (proprietor), `lore` (backstory paragraph), `reputation_requirement: int` (0–4 faction-rep gate).

#### `merchant` (service)

A trading post where the player buys/sells trade goods. Currently active in the game with a working marketplace UI.

```jsonc
{
  "merchant_name": "Vex's Trading Post",         // (req) display name
  "faction": "merchant_guild",                   // optional faction tag
  "specialty_goods": ["Weapons & Ammunition",    // (req) goods this trader prefers
                      "Luxury Items"],
  "inventory_size": 35,                          // (req) 20–60: count of stocked goods
  "haggle_resistance": 1.2,                      // (req) 0.6–2.0; higher = harder haggle
  "price_multiplier": 1.0,                       // (req) 0.7–1.5 baseline price modifier
  "description": "A grimy hangar lined with crates...",
  "npc_name": "Vex Starling",
  "known_for": "ancient artifacts",
  "lore": "Vex deserted from the Merchant Guild after..."
}
```

#### `repair` (service)

Hull and shield repair. Currently active. Cost is computed from ship size and `hub_type`.

```jsonc
{
  "repair_service_name": "Dockyard Seven",      // (req)
  "faction": "shipwrights_consortium",          // optional
  "tech_level": 3,                              // (req) 1–5; affects cost & quality
  "price_multiplier": 1.0,                      // (req) 0.6–1.5
  "specialty": "shield_systems",                // optional: shield_systems | hull_plates | engines | weapons
  "fast_repair_available": true,                // optional; 2× cost for instant repair
  "can_upgrade_components": false,              // optional
  "description": "Three drydock arms hang over a half-finished frigate...",
  "npc_name": "Chief Engineer Solana",
  "lore": "Built into a hollowed-out asteroid by..."
}
```

#### `shipyard` (service)

Sells new ships. Currently active. Reads from `ShipsDB`.

```jsonc
{
  "shipyard_name": "Corsair's Bay Drydocks",    // (req)
  "faction": "shipwrights_consortium",          // optional
  "specialization": "haulers",                  // (req) haulers | fighters | explorers | mixed
  "inventory_size": 6,                          // (req) 3–10 ships stocked
  "price_multiplier": 1.0,                      // (req) 0.8–1.3
  "tech_level": 3,                              // (req) 1–5; max ship tier available
  "ship_pool_filter": ["wayfarer_s","trader_m"],// optional: explicit ship ids
  "offers_upgrades": true,
  "description": "...",
  "npc_name": "Master Shipwright Hadal",
  "lore": "..."
}
```

#### `outfitter` (service / black market shipyard)

Pirate-aligned shipyard variant. Currently active, routed through the same UI as shipyard with an `is_black_market` flag.

```jsonc
{
  "outfitter_name": "The Phantom Yard",         // (req)
  "faction": "phantom_circuit",                 // (req) must be a pirate faction
  "pirate_fame_required": 3,                    // (req) 0–10
  "specialization": "smugglers",                // (req) smugglers | raiders | privateers | underground
  "inventory_tier": 4,                          // (req) 1–5
  "ship_pool_filter": ["ghost_m","viper_m"],    // optional
  "heat_level": 3,                              // (req) 1–5; hotter = riskier but exclusive
  "price_multiplier": 1.0,                      // (req) 0.8–1.4
  "description": "...",
  "npc_name": "The Architect",
  "lore": "..."
}
```

#### `mission_board` (service)

Posts missions for the player to accept. Currently active. Mission list is generated from the system's `hub_type` and population tier — variants override the mix.

```jsonc
{
  "board_name": "Traders' Guild Hall",          // (req)
  "faction": "merchant_guild",                  // optional
  "mission_pool_bias": {                        // (req) override mission type weights
    "delivery": 60,
    "bounty": 10,
    "smuggling": 0,
    "rescue": 15,
    "exploration": 10,
    "clearance": 5,
    "investigation": 0
  },
  "base_mission_count": 5,                      // (req) 2–8
  "mission_tier": 3,                            // (req) 1–5; reward & difficulty scaler
  "reputation_requirement": 1,                  // optional; min faction rep to view
  "specialties": ["delivery","bounty"],         // optional; whitelist mission types
  "description": "...",
  "npc_name": "Quartermaster Reyna",
  "lore": "..."
}
```

#### `cantina` (service / info broker)

Pay shards for drinks, learn rumors that reveal nearby interesting systems. Currently active via `Search.cantina_rumor()`.

```jsonc
{
  "cantina_name": "The Crimson Star Tavern",    // (req)
  "drink_cost": 25,                             // (req) 15–60 shards
  "quality_tier": 3,                            // (req) 1–5; affects rumor quality
  "rumor_region_range": 1,                      // (req) 1–3 regions to sample
  "min_negotiation_level": 0,                   // optional; gate by skill
  "specialties": ["spacer_tales","smuggler_routes"],  // optional flavor tags
  "atmosphere": "rowdy",                        // optional: rowdy | upscale | shady | quiet
  "description": "Smoke-stained booths line the back wall...",
  "bartender_name": "Old Mira",
  "lore": "..."
}
```

#### `refinery` (service — STUB, not yet implemented in game)

Converts raw goods into refined goods at a cost. Generator should produce variants now so the game can implement the handler later.

```jsonc
{
  "refinery_name": "Stellar Metals Refinery",   // (req)
  "faction": "merchant_guild",                  // optional
  "raw_goods_accepted": ["Raw Ores & Minerals", // (req) input good names
                          "Recycled Scrap"],
  "refined_goods_output": ["Refined Metals"],   // (req) output good names
  "conversion_ratio": 2.0,                      // (req) input units → 1 output unit
  "processing_cost_per_unit": 5,                // (req) 2–15 shards/unit
  "processing_time_turns": 0,                   // 0 = instant
  "quality_multiplier": 1.0,                    // (req) 0.8–1.3 affects sell value
  "tech_level": 3,                              // (req) 1–5
  "description": "...",
  "operator_name": "Foreman Drust",
  "lore": "..."
}
```

#### `bank` (service — STUB, not yet implemented)

Deposits, loans, fees. Reputation-gated.

```jsonc
{
  "bank_name": "Neutral Trade Bank",            // (req)
  "faction": "merchant_guild",                  // optional
  "interest_rate_deposit": 0.02,                // (req) % per N turns on deposits
  "interest_rate_loan": 0.08,                   // (req) % per N turns on loans
  "min_deposit": 1000,                          // (req)
  "lending_limit_multiplier": 2.0,              // (req) max loan vs. assets
  "fee_per_transaction": 10,                    // (req)
  "services": ["deposit","loan","letter_of_credit"], // (req)
  "reputation_requirement": 1,                  // optional
  "description": "...",
  "manager_name": "Auditor Klein",
  "lore": "..."
}
```

#### `station_builder` (service)

Sells player-owned station construction at a target Q-R address. Currently active via `station_panel.gd`.

```jsonc
{
  "station_builder_name": "Orbital Construction Authority",  // (req)
  "faction": "shipwrights_consortium",          // optional
  "build_cost_multiplier": 1.0,                 // (req) 0.7–1.5
  "construction_speed_modifier": 1.0,           // (req) 0.5–2.0 (lower = faster)
  "quality_multiplier": 1.0,                    // (req) 0.8–1.3
  "station_type_preference": "trade",           // optional: trade | industrial | military
  "description": "...",
  "engineer_name": "Lead Engineer Halsey",
  "lore": "..."
}
```

---

### 4.2 Loot discoveries (7 types)

Loot stashes give the player goods, shards, or unique items. Most are currently STUBs — the game shows a placeholder notification on visit. Variants drive both flavor and gameplay parameters once handlers are implemented.

#### Common loot fields

All loot variants should produce these shared fields:

- **`loot_table`**: `Array[Dictionary]` — the actual rewards. Each entry: `{"type": "cargo"|"shards"|"item", "good_name": String, "min_qty": int, "max_qty": int}` (or `{"type":"shards","min":int,"max":int}`).
- **`narrative_context`**: `String` — short backstory of why this loot is here.
- **`flavor_text`**: `String` — what the player sees on inspection.

#### `cache` (loot)

A hidden stash. Sealed/guarded variants gate access.

```jsonc
{
  "cache_name": "Smuggler's Stash",             // (req)
  "container_type": "encrypted_safe",           // (req) derelict_pod | hidden_stash | encrypted_safe | drop_pod
  "loot_table": [
    {"type": "cargo", "good_name": "Smuggled Goods", "min_qty": 4, "max_qty": 8},
    {"type": "shards", "min": 200, "max": 500}
  ],
  "sealed": true,                               // (req)
  "unlock_skill": "engineering",                // required if sealed
  "difficulty_class": 45,                       // required if sealed
  "guarded": false,                             // (req)
  "guardian_enemy_type": null,                  // required if guarded — see encounter section
  "condition": "pristine",                      // pristine | weathered | corroded
  "narrative_context": "Left by a Phantom Circuit courier killed in a botched run.",
  "flavor_text": "..."
}
```

#### `salvage` (loot)

Larger debris field with hazards and time pressure.

```jsonc
{
  "salvage_name": "Drifting Hauler Wreck",      // (req)
  "salvage_type": "wreckage",                   // (req) wreckage | derelict_station | asteroid_ruins
  "loot_table": [...],                          // (req)
  "total_mass": 800,
  "hazard": "structural_failure",               // optional: radiation | volatility | structural_failure
  "hazard_skill": "engineering",                // required if hazard
  "hazard_difficulty": 50,                      // required if hazard
  "time_to_salvage_turns": 2,                   // (req)
  "decay_rate": 10,                             // (req) % loss per turn of delay
  "rival_scavengers": false,                    // optional
  "narrative_context": "...",
  "flavor_text": "..."
}
```

#### `wreckage` (loot)

Specifically a wrecked ship, possibly tied to a player death (auto-generated by `DeathManager`) or NPC battle.

```jsonc
{
  "wreckage_name": "Charred Frigate Hulk",      // (req)
  "wreckage_source": "npc_battle",              // (req) player_death | npc_battle | industrial_accident
  "ship_class": "Frigate",                      // (req)
  "loot_table": [...],                          // (req) cargo recovered
  "components_salvageable": ["shield_emitter","cargo_extender"], // optional
  "hull_integrity": 0.4,                        // (req) 0.0–1.0
  "radiation_hazard": false,                    // optional
  "structural_collapse_timer": 5,               // (req) turns until wreck collapses (0 = none)
  "narrative_context": "Iron Hammer-class destroyer, scorched by plasma fire.",
  "flavor_text": "..."
}
```

#### `abandoned_outpost` (loot)

A larger explorable site with multiple rooms/areas.

```jsonc
{
  "outpost_name": "Outpost Helix-7",            // (req)
  "outpost_faction": "explorers_union",         // optional
  "abandonment_reason": "attack",               // (req) resource_depletion | attack | recall | unknown
  "time_abandoned_turns": 200,                  // optional flavor
  "loot_quality": "intact",                     // (req) picked_over | intact | untouched
  "searchable_areas": ["quarters","storage","lab","armory"], // (req)
  "area_loot": {                                // (req) loot per area
    "quarters": [...],
    "storage": [...]
  },
  "area_hazards": {                             // optional
    "lab": "contamination_zone"
  },
  "unique_discovery_item": "research_data_helix7",  // optional
  "exploration_skill_required": 3,              // optional gate
  "narrative_context": "...",
  "flavor_text": "..."
}
```

#### `asteroid_deposit` (loot)

A mineable asteroid. Future mining feature.

```jsonc
{
  "deposit_name": "Veined Iron Belt",           // (req)
  "ore_type": "rare_earth",                     // (req)
  "ore_goods": [                                // (req) what trade goods are produced
    {"good_name": "Raw Ores & Minerals", "qty": 600},
    {"good_name": "Rare Crystals & Exotic Matter", "qty": 4}
  ],
  "ore_quality": 1.2,                           // (req) 0.5–1.5
  "extraction_equipment_required": "mining_lasers",  // optional
  "hazard_type": "solar_flare_risk",            // optional
  "hazard_difficulty": 40,                      // required if hazard
  "extraction_time_turns": 3,                   // (req)
  "rival_miners": false,
  "depletion_chance_per_turn": 0.0,
  "narrative_context": "...",
  "flavor_text": "..."
}
```

#### `ancient_ruin` (loot)

Knowledge / artifact site. Tied to `explorers_union` rep.

```jsonc
{
  "ruin_name": "The Glass Spires",              // (req)
  "ruin_age": "pre_collapse",                   // (req) pre_collapse | ancient | mysterious
  "archaeological_value": 8,                    // (req) 1–10
  "loot_table": [                               // (req) physical loot
    {"type":"cargo","good_name":"Ancient Artifacts","min_qty":1,"max_qty":3}
  ],
  "knowledge_reward": "alien_language_fragment_3", // optional unique
  "excavation_difficulty": 60,                  // (req) skill check
  "excavation_skill": "exploration",            // (req)
  "structural_integrity": 60,                   // (req) 1–100
  "collapse_hazard": false,
  "activation_risk": false,
  "faction_interest": "explorers_union",        // optional — bring findings here for rep
  "narrative_context": "Hexagonal pillars of obsidian-glass jut from the regolith...",
  "flavor_text": "..."
}
```

#### `black_market_contact` (loot — hybrid service)

Unlocks contraband delivery missions and pirate inventory. Currently active via `black_market.gd`.

```jsonc
{
  "contact_name": "Nexus",                      // (req)
  "contact_alias": "The Cipher",                // optional
  "faction_affiliation": "phantom_circuit",     // (req) pirate faction
  "pirate_fame_required": 2,                    // (req) 0–10
  "specialization": "tech",                     // (req) weapons | tech | contraband | information
  "offer_quality": "premium",                   // (req) standard | premium
  "offer_count_base": 4,                        // (req) 3–8 contracts on offer
  "margin_percent": 30,                         // (req) 10–60
  "danger_rating": 3,                           // (req) 1–5
  "information_available": ["faction_movements","outpost_locations"], // optional rumors
  "description": "...",
  "lore": "..."
}
```

---

### 4.3 Encounter discoveries (5 types + 3 special)

Encounters trigger combat (or negotiation). All route to the combat screen via `encounter_manager.gd` and use `enemy_generator.gd` to spawn enemies. Variants drive enemy parameters and reward tables.

#### Common encounter fields

All encounter variants should produce:

- **`enemy_template`**: `Dictionary` describing the enemy(ies). Fields: `entity_name`, `faction`, `fame_level: int`, `ship_class`, `base_stats: {hull, shields, damage, accuracy, defense, flee_chance, can_negotiate, can_intimidate, negotiation_resist}`. The game's `MasterEntity` model uses this exact structure.
- **`loot_table`**: `Array[Dictionary]` — same format as loot discoveries — awarded on victory.
- **`reputation_change`**: `Dictionary` `{faction_id: int_delta}` applied on resolution.
- **`narrative`**: `String` — combat intro text.

#### `pirate_hideout` (encounter)

Static pirate base. Combat on visit. Currently active.

```jsonc
{
  "hideout_name": "Stiletto's Den",             // (req)
  "faction": "silk_hand",                       // (req) pirate faction
  "leader_name": "Captain Stiletto",            // optional
  "enemy_template": {                           // (req)
    "entity_name": "Stiletto's Crew",
    "faction": "silk_hand",
    "fame_level": 3,
    "ship_class": "Corvette",
    "base_stats": {"hull":160,"shields":80,"damage":18,"accuracy":0.7,"defense":0.6,
                   "flee_chance":0.1,"can_negotiate":true,"can_intimidate":true,
                   "negotiation_resist":0.7}
  },
  "fortifications": 2,                          // 0–3 difficulty mod
  "loot_table": [                               // (req)
    {"type":"shards","min":400,"max":900},
    {"type":"cargo","good_name":"Smuggled Goods","min_qty":2,"max_qty":5}
  ],
  "reputation_change": {"silk_hand":-2,"space_force":1},
  "narrative": "..."
}
```

#### `smuggler_dock` (encounter — currently misrouted in game)

⚠️ Note: the game currently routes `smuggler_dock` to the shipyard UI (as a black market variant). The intent is for it to be an encounter OR a black market service depending on `combat_encounter` flag. Generator should produce both kinds.

```jsonc
{
  "dock_name": "Phantom Harbor",                // (req)
  "faction": "phantom_circuit",                 // (req) pirate faction
  "dock_master": "Quartermaster Vyle",          // optional
  "combat_encounter": false,                    // (req) true=fight, false=service
  "security_level": 3,                          // (req) 1–5
  "smuggling_route": "Q02-R0303 corridor",      // optional
  "contraband_pool": ["Black Market Cybernetics","Designer Drugs & Stimulants"], // for service mode
  "enemy_template": {...},                      // required if combat_encounter=true
  "loot_table": [...],                          // required if combat_encounter=true
  "narrative": "..."
}
```

#### `patrol_outpost` (encounter)

Legitimate authority encounter. Currently active.

```jsonc
{
  "outpost_name": "Space Force Station Gamma-7", // (req)
  "authority_faction": "space_force",            // (req)
  "commander_name": "Commander Vance",
  "enforcement_level": 3,                        // (req) 1–5
  "jurisdiction": "lawful",                      // optional
  "cargo_scan_risk": true,                       // (req) triggers fight if smuggling
  "bribery_allowed": true,                       // (req) can negotiate exit
  "bribery_cost_base": 500,                      // required if bribery_allowed
  "enemy_template": {...},                       // (req)
  "loot_table": [...],                           // (req) only if defeated (illegal)
  "reputation_change": {"space_force":-3},       // applied if you fight
  "narrative": "..."
}
```

#### `mercenary_camp` (encounter)

Professional combatants. Can sometimes be hired. Currently active.

```jsonc
{
  "camp_name": "Striker's Company HQ",          // (req)
  "company_name": "Storm Legion",               // (req)
  "commander": "Strike-Captain Halloran",       // (req)
  "reputation": 4,                              // (req) 1–5
  "specialization": "heavy_assault",            // (req) heavy_assault | espionage | logistics | scout
  "hireable": true,                             // (req)
  "hire_cost": 2500,                            // required if hireable
  "current_contract": "defending Sector 5",     // optional flavor
  "combat_difficulty": "hard",                  // (req) easy | moderate | hard | extreme
  "enemy_template": {...},                      // (req)
  "loot_table": [...],                          // (req)
  "narrative": "..."
}
```

#### `ambush_zone` (encounter)

Hidden combat trap. High piloting reveals it before triggering.

```jsonc
{
  "ambush_name": "The Shrike's Net",            // (req)
  "ambush_type": "pirate_trap",                 // (req) pirate_trap | faction_patrol_trap | automated_defense
  "trigger_condition": "always",                // (req) always | if_wealthy | if_carrying_contraband
  "ambush_strength": "moderate",                // (req) weak | moderate | strong
  "warning_signs": 2,                           // (req) 0–3, piloting perception DC
  "escape_possible": true,                      // (req)
  "escape_skill": "piloting",
  "escape_difficulty": 50,
  "enemy_template": {...},                      // (req)
  "loot_table": [...],                          // (req)
  "reward_on_defeat": 300,                      // bonus for clearing the trap
  "narrative": "..."
}
```

#### Special: `bounty_target` (encounter, mission-linked)

Created dynamically when the player accepts a bounty mission. Generator produces variants that the mission system can pick from to flavor the target.

```jsonc
{
  "target_name": "Razorhand Kel",               // (req)
  "target_alias": "The Surgeon",
  "target_faction": "void_reavers",             // (req)
  "crime": "piracy",                            // (req)
  "threat_level": 4,                            // (req) 1–5
  "enemy_template": {...},                      // (req) typically buffed: +2 fame, 1.5× hull
  "personal_loot_table": [                      // (req) unique drops
    {"type":"cargo","good_name":"Black Market Cybernetics","min_qty":1,"max_qty":2}
  ],
  "wanted_poster_text": "Wanted dead. 2,400 shards.",
  "narrative": "..."
}
```

#### Special: `pirate_outpost` (encounter, clearance-injected)

Auto-injected by `clearance_manager.gd` into pirate-controlled systems. Generator produces variants the clearance system can pick from.

```jsonc
{
  "outpost_codename": "Forge of Rust",          // (req)
  "faction": "rust_collective",                 // (req)
  "commander_name": "Iron-Marshal Drog",
  "outpost_strength": 3,                        // (req) 1–5
  "fortifications": 2,                          // (req) 0–3
  "garrison_count": 1,                          // for future multi-enemy
  "critical_equipment": ["radar_array","fuel_depot"], // optional flavor
  "enemy_template": {...},                      // (req)
  "loot_table": [...],                          // (req)
  "clear_reward_fame": 1,                       // bonus pirate-faction fame
  "narrative": "..."
}
```

#### Special: `warlord` (encounter, clearance-injected boss)

Spawned after all outposts in a pirate-controlled system are cleared. End-of-clearance boss.

```jsonc
{
  "warlord_name": "Captain Vex the Merciless",  // (req)
  "warlord_title": "Bloodfist of the Void",
  "faction": "void_reavers",                    // (req)
  "threat_level": 8,                            // (req) 5–10
  "fleet_strength": 3,                          // (req) implied escort count
  "command_style": "brutal",                    // brutal | cunning | honorable | nihilistic
  "personal_ship_class": "Battleship",          // (req)
  "weakness": "loses focus when his second-in-command falls", // optional lore hint
  "enemy_template": {...},                      // (req) high-end stats
  "loot_table": [...],                          // (req) unique high-value drops
  "system_liberation_reward": {                 // (req)
    "shards": 5000,
    "reputation": {"space_force": 3, "merchant_guild": 2}
  },
  "narrative": "..."
}
```

---

### 4.4 Passive discoveries (5 types)

Passive discoveries are flavor / lore / one-shot bonus sites. Currently all STUBs — placeholder notification on visit.

#### Common passive fields

- **`flavor_text`**: shown on visit
- **`lore`**: longer backstory
- **`one_time_reward`**: optional `{shards: int, xp: {skill: int}, reputation: {faction: int}}`

#### `observatory` (passive)

Knowledge site, often `explorers_union` affiliated.

```jsonc
{
  "observatory_name": "Kaleidoscope Observatory",  // (req)
  "faction_affiliated": "explorers_union",         // optional
  "primary_research": "stellar_formation",         // (req) stellar_formation | alien_signals | cosmic_radiation | xenobiology
  "knowledge_grants": ["new_jump_route","faction_info"],  // optional
  "exploration_xp": 25,                            // (req)
  "discovery_chance": 0.15,                        // optional: % to reveal hidden nearby system
  "one_time_reward": {"reputation":{"explorers_union":1}},
  "scientist_name": "Doctor Iyandra",
  "flavor_text": "...",
  "lore": "..."
}
```

#### `beacon` (passive)

Landmark with utility — navigation, distress, or alien.

```jsonc
{
  "beacon_name": "Listening Post Theta",        // (req)
  "beacon_type": "navigation",                  // (req) navigation | distress | homing | alien
  "beacon_status": "active",                    // (req) active | dormant | malfunctioning
  "beacon_origin": "pre_collapse",              // (req)
  "navigation_boost": true,                     // optional
  "signal_strength": 4,                         // 1–5
  "hidden_message": "Decoded text revealed at exploration ≥5...", // optional
  "hazard_if_active": false,
  "one_time_reward": {"xp":{"piloting":15}},
  "flavor_text": "...",
  "lore": "..."
}
```

#### `monument` (passive)

Lore / history landmark.

```jsonc
{
  "monument_name": "The Last Watcher",          // (req)
  "monument_type": "megastructure",             // (req) space_station | megastructure | artificial | natural
  "builder": "pre_collapse_civilization",       // (req)
  "monument_age": "ancient",
  "inscription": "We were here. Remember.",     // optional
  "historical_significance": 9,                 // (req) 1–10
  "one_time_reward": {"shards":300,"reputation":{"explorers_union":2}},
  "unlocks_story_flag": "ancient_builders_confirmed", // optional
  "faction_interest": {"explorers_union": 2},
  "flavor_text": "...",
  "lore": "..."
}
```

#### `ruined_station` (passive)

Lore site with optional salvage potential.

```jsonc
{
  "station_name": "Derelict Station Epsilon",   // (req)
  "original_purpose": "research",               // (req) research | mining | military | habitation
  "destruction_cause": "attack",                // (req) attack | accident | abandonment | unknown
  "structural_stability": 2,                    // (req) 1–5
  "salvage_potential": true,                    // optional → if true, includes loot_table
  "loot_table": [...],                          // required if salvage_potential
  "hazards_inside": ["radiation","vacuum_breach"], // optional
  "faction_clue": "Void Reavers patrol logs",   // optional story hook
  "flavor_text": "...",
  "lore": "..."
}
```

#### `stellar_anomaly` (passive)

Mystery site with science value.

```jsonc
{
  "anomaly_name": "The Whisper Rift",           // (req)
  "anomaly_type": "spatial_distortion",         // (req) black_hole | wormhole | spatial_distortion | energy_storm
  "danger_level": 3,                            // (req) 1–5
  "navigation_hazard": true,
  "approach_difficulty": 55,                    // required if navigation_hazard
  "scientific_value": 400,                      // (req) shards reward for studying
  "unique_resource": "exotic_matter_sample",    // optional
  "one_time_reward": {"shards":400,"xp":{"engineering":20}},
  "story_significance": "wormhole_destination_unknown", // optional
  "flavor_text": "...",
  "lore": "..."
}
```

---

### 4.5 Hazard discoveries (5 types)

Hazards are skill-check encounters. **Currently fully implemented** via `Search.resolve_hazard()`. The game already has working defaults — variants enrich them with flavor and overrides.

Default behavior (from `HAZARD_DEFS` in `search_manager.gd`):

| Type | Skill | Default Difficulty |
|---|---|---|
| `unstable_ruins` | exploration | 50 |
| `contamination_zone` | engineering | 55 |
| `hostile_wildlife` | combat | 60 |
| `trap_mechanism` | piloting | 50 |
| `radiation_zone` | engineering | 65 |

Distance modifier: `final_difficulty = base + manhattan_distance`. Success: `+60 + dist*12` shards, `+12 + dist*2` skill XP. Failure: `-(8 + dist)` hull damage.

#### Common hazard fields

- **`hazard_label`**: display name for the variant
- **`skill_override`**: if set, replaces the default skill
- **`difficulty_override`**: if set, replaces the default base difficulty
- **`success_bonus_shards`**: optional flat bonus added to default reward
- **`failure_extra_damage`**: optional flat bonus added to default damage
- **`narrative`**, **`flavor_text`**, **`success_text`**, **`failure_text`**

#### `unstable_ruins` (hazard)

```jsonc
{
  "hazard_label": "The Fractured Tomb",         // (req)
  "ruin_type": "alien_construct",               // (req)
  "structural_damage": 3,                       // (req) 1–5
  "hazard_pattern": "cascading_failure",        // (req)
  "advance_warning": 2,                         // 0–3 piloting bonus
  "secondary_hazard": "toxic_dust",             // optional
  "loot_probability": 0.4,                      // optional: chance of bonus loot on success
  "loot_table": [...],                          // required if loot_probability > 0
  "skill_override": null,
  "difficulty_override": null,
  "narrative": "...",
  "success_text": "...",
  "failure_text": "..."
}
```

#### `contamination_zone` (hazard)

```jsonc
{
  "hazard_label": "Plague-Touched Vault",       // (req)
  "contaminant_type": "biological",             // (req) biological | chemical | radioactive | nanite
  "contamination_spread_rate": 3,               // 1–5
  "suit_protection_available": true,            // optional alt path
  "suit_cost": 200,
  "ship_hull_corrosion": false,
  "skill_override": null,
  "difficulty_override": 60,
  "narrative": "...",
  "success_text": "...",
  "failure_text": "..."
}
```

#### `hostile_wildlife` (hazard)

```jsonc
{
  "hazard_label": "Voidmaw Swarm",              // (req)
  "creature_type": "xenomorphic",               // (req)
  "creature_count": 3,                          // (req) 1–5
  "creature_aggression": 4,                     // (req) 1–5
  "avoidance_possible": true,
  "avoidance_skill": "piloting",
  "avoidance_difficulty": 45,
  "hive_intelligence": false,
  "loot_from_creatures": true,
  "loot_table": [...],
  "ecosystem_preservation": false,              // if true: killing them costs faction rep
  "narrative": "...",
  "success_text": "...",
  "failure_text": "..."
}
```

#### `trap_mechanism` (hazard)

```jsonc
{
  "hazard_label": "Pre-Collapse Sentinel Grid", // (req)
  "trap_type": "automated_weapons",             // (req)
  "trap_origin": "ancient",                     // (req)
  "trigger_condition": "proximity",             // (req)
  "deactivation_method": "hacking",             // optional alt path
  "deactivation_skill": "engineering",
  "deactivation_difficulty": 55,
  "damage_type": "energy",                      // (req)
  "warning_signs": 2,                           // (req) 0–3
  "disarmed_reward": 200,                       // optional bonus
  "narrative": "...",
  "success_text": "...",
  "failure_text": "..."
}
```

#### `radiation_zone` (hazard)

```jsonc
{
  "hazard_label": "Cinder Belt",                // (req)
  "radiation_type": "stellar",                  // (req)
  "radiation_intensity": 4,                     // (req) 1–5
  "shielding_available": true,
  "shielding_cost": 350,
  "equipment_damage": true,
  "time_limit": 0,                              // 0 = no limit
  "safe_zone_exists": false,
  "skill_override": null,
  "difficulty_override": null,
  "narrative": "...",
  "success_text": "...",
  "failure_text": "..."
}
```

---

### 4.6 Shared: `loot_table` format

`loot_table` is referenced by loot discoveries, encounters (combat rewards), and any hazard variant that grants bonus loot on success. It is always an **array of reward entries**, where every entry resolves independently — they are *not* mutually exclusive picks.

#### Entry types

| `type` | Required fields | Notes |
|---|---|---|
| `"shards"` | `min`, `max` | Currency reward. Game rolls a random int in `[min, max]`. |
| `"cargo"` | `good_name`, `min_qty`, `max_qty` | `good_name` **must** match an exact display string from the trade goods list in §3. |
| `"item"` | `item_id`, `min_qty`, `max_qty` | Unique narrative items (research data, alien artifacts, story flag tokens). `item_id` is a snake_case string the game looks up later. |

#### Optional fields on any entry

These are forward-compatible — the game ignores unknown keys, but the generator should support them so handlers can use them once implemented:

| Field | Type | Meaning |
|---|---|---|
| `drop_chance` | float `0.0–1.0` | Probability the entry drops at all. Omit for "always drops". |
| `condition` | string | Narrative gate handled by the discovery's specific handler (e.g. `"if_skill_check_passed"`, `"if_guarded_defeated"`). Handler-specific. |

#### Example — basic three-entry table

```json
"loot_table": [
  { "type": "shards", "min": 200, "max": 500 },
  { "type": "cargo", "good_name": "Smuggled Goods", "min_qty": 4, "max_qty": 8 },
  { "type": "cargo", "good_name": "Black Market Cybernetics", "min_qty": 1, "max_qty": 2 }
]
```

This always grants 200–500 shards **and** 4–8 Smuggled Goods **and** 1–2 Black Market Cybernetics.

#### Example — encounter reward with a rare bonus drop

```json
"loot_table": [
  { "type": "shards", "min": 400, "max": 900 },
  { "type": "cargo", "good_name": "Weapons & Ammunition", "min_qty": 2, "max_qty": 5 },
  { "type": "cargo", "good_name": "Smuggled Goods", "min_qty": 1, "max_qty": 3 },
  { "type": "cargo", "good_name": "Stellar Cartography Data", "min_qty": 1, "max_qty": 1, "drop_chance": 0.25 }
]
```

The cartography data has a 25% chance to drop; everything else drops every time.

#### Example — unique narrative item

```json
"loot_table": [
  { "type": "shards", "min": 100, "max": 200 },
  { "type": "item", "item_id": "alien_language_fragment_3", "min_qty": 1, "max_qty": 1 }
]
```

#### Generator validation rules for `loot_table`

1. Must be a JSON array (may be empty).
2. Every entry must have a `type` field equal to one of: `"shards"`, `"cargo"`, `"item"`.
3. `"shards"` entries must have integer `min` and `max`, with `min <= max`, both `>= 0`.
4. `"cargo"` entries must have a `good_name` matching an exact trade good display string from §3, plus integer `min_qty` and `max_qty` with `min_qty <= max_qty`, both `>= 1`.
5. `"item"` entries must have a snake_case `item_id`, plus integer `min_qty` and `max_qty` with `min_qty <= max_qty`, both `>= 1`.
6. If `drop_chance` is present it must be a float in `[0.0, 1.0]`.
7. If `condition` is present it must be a non-empty string.

---

## 5. Output Format

### Variant pack file structure

The generator should output **one JSON file per discovery type** into a folder the game loads at startup (suggested: `res://data/discoveries/<type>.json`). Each file holds an array of variants for that type. Example file `merchant.json`:

```json
{
  "schema_version": 1,
  "type": "merchant",
  "category": "service",
  "variants": [
    {
      "variant_id": "merchant_vex_trading_post_001",
      "name": "Vex's Trading Post",
      "data": {
        "merchant_name": "Vex's Trading Post",
        "faction": "merchant_guild",
        "specialty_goods": ["Weapons & Ammunition", "Luxury Items"],
        "inventory_size": 35,
        "haggle_resistance": 1.2,
        "price_multiplier": 1.05,
        "description": "A grimy hangar lined with crates...",
        "npc_name": "Vex Starling",
        "known_for": "ancient artifacts",
        "lore": "Vex deserted from the Merchant Guild after..."
      }
    },
    { ... next variant ... }
  ]
}
```

### Required fields on every variant

Every variant, regardless of type, must have:

- `variant_id`: globally unique string, snake_case (e.g. `cache_smuggler_stash_007`)
- `name`: display name copied to `Discovery.name`
- `data`: the full type-specific dict; the game copies this verbatim to `Discovery.data`

### Validation rules the generator should enforce

1. **Faction strings** must match exactly one of the 8 known faction ids.
2. **Skill strings** must match exactly one of the 6 known skills.
3. **Trade good names** must match exactly one entry in `TradeGoodsDB` (use the display strings listed in §3).
4. **Ship ids** must match exactly one entry in `ShipsDB` (also §3).
5. **Mission types** must match the 7 known types.
6. **Difficulty** integers should fall in 10–120.
7. **All required fields** for the type (marked `(req)` in §4) must be present and non-null.
8. **`variant_id`** must be unique within the type's variant pack.

### Discovery serialization (game side, for reference)

When the game saves a discovery, it writes:

```json
{
  "discovery_id": "Q01-R0101-0500_3",
  "category": "service",
  "type": "merchant",
  "name": "Vex's Trading Post",
  "is_discovered": true,
  "is_resolved": false,
  "is_blocking": false,
  "blocked_by": "",
  "data": { /* the variant's data dict, possibly with a "variant_id" key */ }
}
```

The generator does **not** produce these — it only produces the contents of `name` + `data` packaged inside variant entries. The game wires `discovery_id`, the booleans, etc. at runtime.

---

## 6. Generator Program — Recommended Workflow

This section is a suggestion for how the operator-facing generator should work, based on the user's described intent.

1. **Master list view**: pull the full type list from §4 — show category, type id, current variant count, status (active/stub).
2. **Pick a type**: operator selects one type to generate a variant for.
3. **Aspect input**: show the type's schema as an editable form. Pre-fill optional fields with reasonable defaults; leave creative/lore fields empty for the operator to seed (e.g. "theme: derelict mining colony", "tone: melancholic", "faction lean: rust_collective").
4. **Build prompt**: assemble a Claude AI prompt that:
   - Embeds the relevant type schema (copy from §4)
   - Embeds the operator's aspect inputs
   - Embeds the global reference data (factions, skills, trade goods) so Claude doesn't invent invalid strings
   - Instructs Claude to **respond with strict JSON** matching the schema
5. **Call Claude** via the Anthropic API (hidden terminal / subprocess).
6. **Validate** the response against the rules in §5. Reject and retry with feedback if invalid.
7. **Append to the pack file** for that type. Re-write the JSON file atomically.
8. **Diff / preview**: show the operator the new variant before committing, and let them edit fields by hand.

---

## 7. Game-Side Integration (what the game needs to add)

This section is informational, describing what the game must do to ingest the variant packs. The generator program does not need to implement these — but the schemas above are designed to make this trivial.

### A. New autoload `DiscoveryContentDB`

```gdscript
extends Node
# Loads variant packs from res://data/discoveries/*.json on _ready()
# Internal: _by_type: Dictionary[String, Array[Dictionary]]
# API:
#   has_type(type_id) -> bool
#   pick_variant(type_id, rng) -> Dictionary   # random pick
#   get_variant(type_id, variant_id) -> Dictionary  # exact lookup for save reproducibility
#   count_for(type_id) -> int
```

### B. Hook into `search_manager.gd::generate_discoveries()`

After picking a `type_name`, before assigning the auto-label name:

```gdscript
if DiscoveryContentDB.has_type(type_name):
    var variant := DiscoveryContentDB.pick_variant(type_name, rng)
    d.name = variant["name"]
    d.data = variant["data"].duplicate(true)
    d.data["variant_id"] = variant["variant_id"]   # for save reproducibility
else:
    d.name = _label_for_type(type_name)            # current behavior — fallback
```

This makes variant ingestion incremental: any type without variants falls back to today's auto-label behavior, so the game keeps working while content is filled in.

### C. Handlers must read from `data`

Existing handlers (`marketplace.gd`, `repair_panel.gd`, `shipyard.gd`, `mission_board.gd`, `combat_manager.gd`, `Search.resolve_hazard()`, etc.) need to be updated to read variant fields from `discovery.data` and apply them — pricing multipliers, skill overrides, loot tables, enemy templates, etc. STUB handlers (loot, passive) need to be implemented.

---

## 8. Key Insights for the Generator

1. **One model, many shapes.** The game has exactly one `Discovery` class. All variation lives in `data: Dictionary`. The generator is free to add any keys it wants to `data` — the game serializes the whole dict, so future fields don't need code changes.

2. **Variant pools by type, not by category.** Variants are grouped by `type` (e.g. `merchant`), not category (e.g. `service`). The game picks the type first, then rolls a variant within that type's pool.

3. **Fallback is safe.** Until a type has ≥1 variant, the game uses the auto-label behavior. So the generator can ship variant packs incrementally — start with one type, expand from there.

4. **Stubs vs. active types.** Types marked STUB in §4 have no handler yet. Generator should still produce variants for them — the schemas in §4 anticipate the eventual handler design, and writing variants now means content is ready when the handlers land.

5. **Hard string validation matters.** Faction ids, skill names, trade good display strings, and ship ids must be exact. Bake them into the prompt so Claude doesn't hallucinate `space-force` instead of `space_force` or `Iron Ore` instead of `Raw Ores & Minerals`.

6. **Reproducibility via `variant_id`.** Once a system is generated and saved, the game stores the variant_id inside `data`. On load, it can look up the exact variant again to refresh any computed fields. So variant_ids must be stable — never rename or remove a published variant id; deprecate instead.

7. **Difficulty curve.** The galaxy's difficulty scales linearly with Manhattan distance from origin (1 to ~22). Generator should produce a *spread* of variants per type — easy variants for low-distance regions, hard variants for extreme zones. Add an optional `recommended_distance_range: [min,max]` field to variants if you want the game to filter by it later.

8. **Faction flavor.** Each pirate faction has a distinct personality (§3). Variants for `pirate_hideout`, `outfitter`, `black_market_contact`, `warlord`, `pirate_outpost` should lean into these — Silk Hand cunning, Phantom Circuit ghostlike, Rust Collective brutal-industrial, Void Reavers nihilistic-aggressive.

9. **Schema versioning.** Include `schema_version: 1` at the top of every pack file. If the schema changes later, the game can migrate or reject mismatched packs gracefully.

10. **Localization-ready.** All player-visible strings (names, descriptions, lore, narratives) should be plain text now, but consider that later they may be wrapped in i18n calls. Avoid embedding gameplay numbers inside flavor strings — keep numbers in their own fields.

---

## 9. Quick Reference — All 31+ Discovery Types

| # | Type | Category | Status | Required minimum fields |
|---|---|---|---|---|
| 1 | `merchant` | service | active | merchant_name, specialty_goods, inventory_size, haggle_resistance, price_multiplier |
| 2 | `repair` | service | active | repair_service_name, tech_level, price_multiplier |
| 3 | `shipyard` | service | active | shipyard_name, specialization, inventory_size, price_multiplier, tech_level |
| 4 | `outfitter` | service | active | outfitter_name, faction, pirate_fame_required, specialization, inventory_tier, heat_level, price_multiplier |
| 5 | `mission_board` | service | active | board_name, mission_pool_bias, base_mission_count, mission_tier |
| 6 | `cantina` | service | active | cantina_name, drink_cost, quality_tier, rumor_region_range |
| 7 | `refinery` | service | stub | refinery_name, raw_goods_accepted, refined_goods_output, conversion_ratio, processing_cost_per_unit, quality_multiplier, tech_level |
| 8 | `bank` | service | stub | bank_name, interest_rate_deposit, interest_rate_loan, min_deposit, lending_limit_multiplier, fee_per_transaction, services |
| 9 | `station_builder` | service | active | station_builder_name, build_cost_multiplier, construction_speed_modifier, quality_multiplier |
| 10 | `cache` | loot | stub | cache_name, container_type, loot_table, sealed, guarded |
| 11 | `salvage` | loot | stub | salvage_name, salvage_type, loot_table, time_to_salvage_turns, decay_rate |
| 12 | `wreckage` | loot | stub | wreckage_name, wreckage_source, ship_class, loot_table, hull_integrity, structural_collapse_timer |
| 13 | `abandoned_outpost` | loot | stub | outpost_name, abandonment_reason, loot_quality, searchable_areas, area_loot |
| 14 | `asteroid_deposit` | loot | stub | deposit_name, ore_type, ore_goods, ore_quality, extraction_time_turns |
| 15 | `ancient_ruin` | loot | stub | ruin_name, ruin_age, archaeological_value, loot_table, excavation_difficulty, excavation_skill, structural_integrity |
| 16 | `black_market_contact` | loot | active | contact_name, faction_affiliation, pirate_fame_required, specialization, offer_quality, offer_count_base, margin_percent, danger_rating |
| 17 | `pirate_hideout` | encounter | active | hideout_name, faction, enemy_template, loot_table |
| 18 | `smuggler_dock` | encounter | misrouted | dock_name, faction, combat_encounter, security_level |
| 19 | `patrol_outpost` | encounter | active | outpost_name, authority_faction, enforcement_level, cargo_scan_risk, bribery_allowed, enemy_template, loot_table |
| 20 | `mercenary_camp` | encounter | active | camp_name, company_name, commander, reputation, specialization, hireable, combat_difficulty, enemy_template, loot_table |
| 21 | `ambush_zone` | encounter | active | ambush_name, ambush_type, trigger_condition, ambush_strength, warning_signs, escape_possible, enemy_template, loot_table |
| 22 | `observatory` | passive | stub | observatory_name, primary_research, exploration_xp |
| 23 | `beacon` | passive | stub | beacon_name, beacon_type, beacon_status, beacon_origin |
| 24 | `monument` | passive | stub | monument_name, monument_type, builder, historical_significance |
| 25 | `ruined_station` | passive | stub | station_name, original_purpose, destruction_cause, structural_stability |
| 26 | `stellar_anomaly` | passive | stub | anomaly_name, anomaly_type, danger_level, scientific_value |
| 27 | `unstable_ruins` | hazard | active | hazard_label, ruin_type, structural_damage, hazard_pattern |
| 28 | `contamination_zone` | hazard | active | hazard_label, contaminant_type |
| 29 | `hostile_wildlife` | hazard | active | hazard_label, creature_type, creature_count, creature_aggression |
| 30 | `trap_mechanism` | hazard | active | hazard_label, trap_type, trap_origin, trigger_condition, damage_type, warning_signs |
| 31 | `radiation_zone` | hazard | active | hazard_label, radiation_type, radiation_intensity |
| 32 | `bounty_target` | encounter (special) | mission-linked | target_name, target_faction, crime, threat_level, enemy_template, personal_loot_table |
| 33 | `pirate_outpost` | encounter (special) | clearance-injected | outpost_codename, faction, outpost_strength, fortifications, enemy_template, loot_table |
| 34 | `warlord` | encounter (special) | clearance-injected | warlord_name, faction, threat_level, fleet_strength, personal_ship_class, enemy_template, loot_table, system_liberation_reward |

---

*End of document.*
