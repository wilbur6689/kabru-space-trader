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
| [Ship](03_ship.md) | Ship classes, jump drive, upgrades, cargo, death/recovery | Draft |
| [Pilot](04_pilot.md) | Stats, skills, backgrounds, reputation, journal | Draft |
| [Challenges](05_challenges.md) | Discovery dangers, travel encounters, combat, missions | Draft |
| [Technical Design](06_technical.md) | Godot architecture, autoloads, scenes, procedural gen | Draft |
| [Economy & Trading](07_economy_and_trading.md) | Stock market pricing, trade goods, routes, black market | Draft |
| [Story & Narrative](08_story_and_narrative.md) | Tutorial, story arcs, factions, NPCs, lore | Draft |
| [Save System](09_save_system.md) | Persistent discovery DB, economy state, recovery saves | Draft |
| [UI & Presentation](10_ui_and_presentation.md) | System overview, star map, address input, HUD, journal | Draft |
| [Audio & Atmosphere](11_audio_and_atmosphere.md) | Music per system type, SFX, visual atmosphere | Draft |
| [Game Balancing](12_balancing.md) | Jump drive tiers, economy tuning, difficulty scaling | Draft |

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
| Galaxy Structure | 4 quadrants, grid-based regions | Natural progression, pirate factions per quadrant | 2026-04-03 |
| System Types | Tier (Major→Measly) + Alignment (5 types) | Any combo possible, weighted by distance | 2026-04-03 |
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
3. Procedural generation algorithm for Q-R address mapping
4. Visual theme assignment for systems (space station vs. planet vs. nebula)
5. Regional trade good specialty generation algorithm

---

## Notes
_Add general design notes, inspiration, and references here._
