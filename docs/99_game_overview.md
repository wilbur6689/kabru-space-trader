# Game Overview

## Overview
This is the master design document for the space trading game. It ties together all other design documents into a complete picture of the game. **This document should be completed last**, after all other design documents have been fleshed out.

---

## Game Summary
- **Title:** [Game Title]
- **Genre:** Space trading / exploration / adventure
- **Engine:** Godot 4.3+
- **Scripting:** GDScript (primary), C# (optional)
- **Platform:** Windows, macOS, Linux (potential Web/HTML5 export)
- **Target Audience:** [Define]
- **Estimated Play Time:** [Define]

### Elevator Pitch
_One paragraph describing the game in an exciting, compelling way._

---

## Core Pillars
The fundamental principles that guide every design decision:

1. **[Pillar 1]** - e.g., "Trading should always feel rewarding and strategic"
2. **[Pillar 2]** - e.g., "Every journey should tell a story"
3. **[Pillar 3]** - e.g., "Player choices should have meaningful consequences"

---

## Document Index

| Document | Description | Status |
|----------|-------------|--------|
| [Core Game Loop](01_core_game_loop.md) | Turn-based loop, searching, discovery, navigation | Draft |
| [Planets & Systems](02_planets.md) | System types, quadrant-region grid, Q-R addresses, discovery | Draft |
| [Galaxy Structure](02a_galaxy_structure.md) | Quadrant grid layout, regions, Q-R address system, starting region | Draft |
| [System Types](02b_system_types.md) | Population tiers, alignment types, discovery counts | Draft |
| [Discovery System](02c_discovery_system.md) | Search mechanic, discovery types, persistent database | Draft |
| [Player Stations](02d_player_stations.md) | Station construction, tiers, storage, raids, respawn | Draft |
| [Ship](03_ship.md) | Ship classes, jump drive, upgrades, cargo, death/recovery | Draft |
| [Pilot](04_pilot.md) | Stats, skills, backgrounds, reputation, journal | Draft |
| [Challenges](05_challenges.md) | Discovery dangers, travel encounters, combat, missions | Draft |
| [Combat System](05a_combat_system.md) | Turn-based combat, fame checks, MasterEntity system | Draft |
| [Enemies & Factions](05b_enemies_and_factions.md) | Pirate factions, enemy types, fame scaling, loot | Draft |
| [Pirate System Clearing](05c_hostile_systems.md) | Outpost clearing, Warlord bosses, system liberation | Draft |
| [Missions](05d_missions.md) | Mission types, rewards, reputation, fame-gated slots | Draft |
| [Events & Hazards](05e_events_and_hazards.md) | Travel encounters, environmental hazards, system events | Draft |
| [Technical Design](06_technical.md) | Godot architecture, autoloads, scenes, procedural gen | Draft |
| [Economy & Trading](07_economy_and_trading.md) | Stock market pricing, trade goods, routes, black market | Draft |
| [Story & Narrative](08_story_and_narrative.md) | Tutorial, story arcs, factions, NPCs, lore | Draft |
| [Save System](09_save_system.md) | Persistent discovery DB, economy state, recovery saves | Draft |
| [UI & Presentation](10_ui_and_presentation.md) | System overview, star map, address input, HUD, journal | Draft |
| [Audio & Atmosphere](11_audio_and_atmosphere.md) | Music per system type, SFX, visual atmosphere | Draft |
| [Game Balancing](12_balancing.md) | Jump drive tiers, economy tuning, difficulty scaling | Draft |
| [Deferred Design](13_deferred_design.md) | Deferred decisions, algorithms, and mechanics for later | Draft |
| [Galaxy ASCII Map](galaxy_ascii_map.md) | Visual reference for quadrant grid layout | Reference |

---

## Game Flow Summary

### New Game (Tutorial)
1. Player creates pilot (name, background)
2. Player begins **stranded** with a broken ship in a Minor system within Q01-R0101
3. Repair the ship (learn searching, discovery, and repair)
4. Fly to a second system to deliver goods (learn main story hook)
5. Navigate to Earth at Q01-R0101-1000 (learn quadrant-region address system and jump travel)
6. Main story begins at Earth
7. Tutorial ends — open galaxy available

### Core Loop
1. Arrive at a system — view overview (type, danger, discovery progress "X / Y")
2. **Search** to discover locations (costs 1 turn, may find merchants, resources, or dangers)
3. **Visit** discovered locations freely (trade, repair, accept missions, gather intel)
4. **Travel** — input a Q-R address, ship plots jump path, intra-region travel has pass-thru events
5. Repeat — build wealth, upgrade ship (especially jump drive), explore new regions
6. **Clear pirate-controlled systems** to change the galaxy and progress

### Progression Gates
- **Jump drive tier** gates access to new regional clusters
- **Ship upgrades** enable survival in harder regions
- **Reputation** unlocks missions, better prices, and faction content
- **Knowledge** (known addresses, price trends) enables smarter trading

### Death and Recovery
- Player respawns at last visited Friendly Major/Moderate system with a loaner ship
- Original ship stays at death location with 0-50% cargo looted
- Player recovers ship or buys a new one — always able to continue

---

## Game Loops

### Primary Loop — Trade and Explore
The core loop that drives most gameplay sessions:
1. Check scanner/journal for known systems with profitable trade opportunities
2. Travel to a system — buy goods where supply is high (cheap)
3. Travel to another system — sell goods where supply is low (expensive)
4. Use profits to upgrade ship components, buy better ships, or invest in stations
5. Upgraded jump drive unlocks new regions with better opportunities and harder challenges
6. Repeat — each trade run builds wealth, XP, and galaxy knowledge

### Exploration Loop — Search and Discover
Driven by curiosity and the discovery mechanic:
1. Arrive at an unvisited system — check alignment and tier for discovery count and visibility
2. Friendly/Neutral systems reveal 3 known services on arrival; Hostile/Pirate/Dead reveal nothing
3. Scanner previews what the next search might reveal (category + risk level, influenced by scanner tier)
4. Search (costs 1 turn) — discover a service, loot, or encounter; hostile discoveries may block related finds
5. Resolve the discovery — trade at a merchant, loot a cache, fight a threat, rescue an NPC
6. Decide: search again (risk more turns for more discoveries) or move on
7. Discoveries are permanent — building a personal map of valuable locations across the galaxy

### Combat Loop — Fight or Flee
Triggered by dangerous discoveries, travel pass-thru events, or pirate territory:
1. Encounter triggered — fame comparison check (player fame 3+ above? Enemy flees on sight)
2. Enemy info displayed — type, faction, fame level, ship class
3. Choose action: Attack, Defend, Flee, Negotiate, or Intimidate (if fame allows)
4. Resolve turn — damage applied to shields then hull, enemy responds based on faction behavior
5. Check outcome: victory (loot), defeat (death/respawn), escape, negotiated peace, or intimidation
6. Combat XP earned, ship damage persists until repaired

### Pirate Clearing Loop — Liberate Systems
Mid-to-late game progression through pirate-controlled territory:
1. Enter a Pirate-Controlled system — nothing known, everything hidden
2. Search to discover locations — some are enemy outposts (combat required)
3. Clear required number of outposts (2-5 based on Manhattan distance)
4. Boss warning received — 2-3 turns before the faction Warlord arrives
5. Leave to prepare (repair, upgrade, restock) or stay and fight
6. Defeat the Warlord — system converts to Friendly, trade routes open, region stabilizes

### Mission Loop — Structured Objectives
Goal-oriented gameplay with faction reputation effects:
1. Visit a system with a mission board discovery — browse available missions
2. Accept a mission (delivery, bounty, smuggling, rescue, exploration, clearance, investigation)
3. Travel to the mission destination — intra-region travel may trigger pass-thru events
4. Complete the objective — deliver cargo, defeat target, rescue NPC, gather intel
5. Earn rewards: shards, reputation with issuing faction, XP in relevant skills
6. Reputation unlocks better missions, prices, and faction-specific content

### Black Market Loop — High Risk, High Reward
Underground economy with pirate faction connections:
1. Discover a black market contact at a system (rare discovery, more common in Pirate-Controlled/Neutral)
2. Accept a black market delivery — contraband cargo loaded into special slot
3. Travel to the delivery destination — risk patrol checkpoint pass-thru events (contraband scan)
4. Delivery outcome rolled: exact delivery (50%), shortage (25%), different goods (15%), undercover bust (5%), rare bonus (5%)
5. Earn shards and pirate fame — higher pirate fame unlocks better deals and stops pirate attacks
6. Risk: legitimate faction patrols target you more, bounty hunters may appear, hub NPCs may refuse service

### Station Building Loop — Late Game Investment
Establishing personal infrastructure across the galaxy:
1. Find a Station Construction Service discovery at a system
2. Pay for construction — choose an empty address as the destination
3. Station is delivered after a delay (cost and time scale with distance)
4. Use Tier 1 for storage — stash goods to sell when prices spike
5. Upgrade to Tier 2 for repair capability — passive pirate raids begin (small losses)
6. Upgrade to Tier 3 for blueprint crafting and respawn — active raids begin (must defend or lose tier)
7. Strategic placement: Tier 3 stations in wild space create safe footholds for deep exploration

### Skill Progression Loop — Learn by Doing
Passive progression that rewards all playstyles:
1. Every action earns XP in a relevant skill (trading from trades, combat from fights, etc.)
2. Skills level up 0-10, each level gives flat incremental bonuses
3. Every 5 combined skill levels earns 1 fame point (max 12)
4. Allocate fame points to any skill for +3% bonus per point
5. Higher fame unlocks: intimidation (skip easy fights), more mission slots, better prices, NPC access
6. Fame level also determines enemy reactions — high fame enemies won't be intimidated, low fame enemies flee on sight

### Death and Recovery Loop — Setback, Not Ending
Consequence without permanent loss:
1. Ship destroyed — hull reaches zero in combat or hazard
2. Respawn at last visited Friendly Major/Moderate system (or Tier 3 station) with a loaner Shuttle
3. Original ship wreckage remains at death location — 0-50% cargo looted by enemies
4. Choice: travel to wreckage in loaner to recover ship (damaged), or buy a new ship
5. Recovered ship needs repairs before full functionality
6. No permanent stat/skill loss — player always recovers

---

## Key Design Decisions
_Record major design decisions here as they are made._

| Decision | Choice | Rationale | Date |
|----------|--------|-----------|------|
| Engine | Godot 4.3+ | Open-source, strong 2D/UI, cross-platform export | 2026-03-31 |
| Primary Language | GDScript | Native Godot integration, rapid iteration | 2026-03-31 |
| Game Timing | Turn-based | Strategic trading and exploration, no time pressure | 2026-03-31 |
| Navigation | Quadrant-region address system (Q-R) | Massive explorable galaxy, rewards curiosity | 2026-03-31 |
| Discovery | Progressive, persistent DB | Each search matters, revisits show progress | 2026-03-31 |
| Economy | Stock market model | Dynamic prices create trading opportunities | 2026-03-31 |
| Death | Respawn at last Friendly Major/Moderate | Setback not ending, always recoverable | 2026-03-31 |
| Galaxy Structure | 4 quadrants, 12x12 grid regions (10x10 usable) | Natural progression, pirate factions per quadrant, dead space border | 2026-04-03 |
| System Types | Tier (Major→Measly) + Alignment (5 types) | Any combo possible, weighted by distance | 2026-04-03 |
| Danger Scaling | Manhattan distance from origin (0,0) | 4 danger zones: Safe 1-3, Low-Med 4-6, Med-High 7-9, Extreme 10+ | 2026-04-03 |
| Pirate Factions | 4 factions, one per quadrant (Q01 easiest → Q04 hardest) | Silk Hand, Phantom Circuit, Rust Collective, Void Reavers | 2026-04-03 |
| Faction Borders | Blending within 2 regions of quadrant borders | Creates turf war zones with 75/25 → 50/50 → 25/75 ratios | 2026-04-03 |
| Regional Economy | Unique weighted specialty list per region | Top 25% become dominant industries, drives trade routes | 2026-04-03 |
| Discovery Visibility | Alignment determines known vs. hidden on arrival | Friendly/Neutral reveal 3 services; Hostile/Pirate reveal nothing | 2026-04-03 |
| Scanner Influence | Scanner tier affects discovery quality weighting | Better scanners find better locations vs. dangers | 2026-04-03 |
| Save System | Hybrid procedural — seed for unvisited, full save for visited | Keeps save files small while preserving exact state of visited systems | 2026-04-03 |
| Save-Scumming | Explicitly allowed — manual saves never overwritten on death | Casual-friendly design, player controls difficulty | 2026-04-03 |
| Population Density | 200 populated systems per region (20% of 1,000 addresses) | Enough variety without overwhelming, rewards exploration | 2026-04-03 |
| Story Pacing | Player-triggered only | No timers, player controls the pace | 2026-03-31 |

---

## Scope and Milestones

### MVP (Minimum Viable Product)
- [ ] Core turn-based game loop functional (search, visit, travel)
- [ ] Quadrant-region address system with jump path routing
- [ ] 1 region with systems of varying tier and alignment
- [ ] Progressive discovery with persistent database
- [ ] Basic trading with stock market price model
- [ ] Basic ship with jump drive upgrades
- [ ] Simple pilot stats and skills
- [ ] Death/respawn with loaner ship
- [ ] Save/load system
- [ ] Basic Godot UI with placeholder art
- [ ] Tutorial sequence (stranded ship → delivery → hub)

### Version 1.0
- [ ] Multiple regions across all 4 quadrants with varying difficulty
- [ ] Full system variety (all population tiers and alignments)
- [ ] Multiple ship classes with full upgrade paths
- [ ] Combat system with enemy types and faction bosses
- [ ] Mission system (delivery, bounty, rescue, clearance, exploration)
- [ ] Hostile system clearing mechanic
- [ ] Bulletin boards, NPC info sharing, star chart purchases
- [ ] Story arc (Acts 1-3)
- [ ] Polished UI with custom theme and artwork
- [ ] Full soundtrack and SFX per system type
- [ ] Player journal with price history and statistics

### Future / Stretch Goals
- [ ] Crew system
- [ ] Multiplayer trading
- [ ] Modding support
- [ ] Mobile export (Android/iOS)
- [ ] Full procedural galaxy generation
- [ ] Web export (HTML5)
- [ ] Faction war system with dynamic territory control

---

## Open Questions
_Major unresolved design questions that need answers._

1. Main story arc content (Acts 1-3)
2. Legitimate faction names, territories, and relationships
3. Procedural generation algorithms (address population, system naming, jump path routing)
4. Visual theme assignment for systems (space station vs. planet vs. nebula)
5. Combat damage formulas and balancing
6. Cockpit UI zone-to-function mapping
7. Dead system lore — connected backstory or independent events?

### Recently Resolved
- ~~Currency name~~ → **Shards**
- ~~Pirate faction names~~ → Silk Hand (Q01), Phantom Circuit (Q02), Rust Collective (Q03), Void Reavers (Q04)
- ~~Galaxy structure~~ → 4 quadrants, 12x12 grid regions, Manhattan distance danger scaling
- ~~Discovery visibility rules~~ → Alignment-based (Friendly/Neutral reveal 3; others reveal nothing)
- ~~Regional economy specialties~~ → Unique weighted specialty list per region, top 25% dominant

---

## Notes
_Add general design notes, inspiration, and references here._
