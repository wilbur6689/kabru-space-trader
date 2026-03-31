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
| [Core Game Loop](01_core_game_loop.md) | Player interaction flow, game states, actions | Draft |
| [Planets](02_planets.md) | Planet generation, types, economies, star map | Draft |
| [Ship](03_ship.md) | Ship classes, upgrades, maintenance, cargo | Draft |
| [Pilot](04_pilot.md) | Player character, stats, skills, progression | Draft |
| [Challenges](05_challenges.md) | Encounters, combat, hazards, missions | Draft |
| [Technical Design](06_technical.md) | Godot architecture, project structure, scenes | Draft |
| [Economy & Trading](07_economy_and_trading.md) | Trade goods, pricing, income, expenses | Draft |
| [Story & Narrative](08_story_and_narrative.md) | Lore, factions, characters, story arcs | Draft |
| [Save System](09_save_system.md) | Save/load, file format, auto-save | Draft |
| [UI & Presentation](10_ui_and_presentation.md) | Godot UI, screens, visual style, input | Draft |
| [Audio & Atmosphere](11_audio_and_atmosphere.md) | Music, SFX, visual effects, immersion | Draft |
| [Game Balancing](12_balancing.md) | Numbers, progression curves, tuning | Draft |

---

## Game Flow Summary

### New Game
1. Player creates pilot (name, background)
2. Receive starting ship and credits
3. Tutorial / intro story sequence
4. Arrive at starting planet

### Core Loop
1. Explore planet (trade, missions, services)
2. Choose destination and depart
3. Travel (fuel consumption, random encounters)
4. Arrive at new planet
5. Repeat while advancing story and building wealth

### End Game
- [ ] Victory condition summary
- [ ] Failure condition summary
- [ ] Post-game content

---

## Key Design Decisions
_Record major design decisions here as they are made._

| Decision | Choice | Rationale | Date |
|----------|--------|-----------|------|
| Engine | Godot 4.3+ | Open-source, strong 2D/UI, cross-platform export | 2026-03-31 |
| Primary Language | GDScript | Native Godot integration, rapid iteration | 2026-03-31 |

---

## Scope and Milestones

### MVP (Minimum Viable Product)
- [ ] Core game loop functional
- [ ] 5+ planets with trading
- [ ] Basic ship with fuel and cargo
- [ ] Simple pilot stats
- [ ] Save/load
- [ ] Basic Godot UI with placeholder art

### Version 1.0
- [ ] Full planet set
- [ ] Multiple ship types
- [ ] Combat system
- [ ] Mission system
- [ ] Story arc (Acts 1-3)
- [ ] Polished UI with custom theme and artwork
- [ ] Full soundtrack and SFX

### Future / Stretch Goals
- [ ] Crew system
- [ ] Multiplayer trading
- [ ] Modding support
- [ ] Mobile export (Android/iOS)
- [ ] Procedural galaxy generation
- [ ] Web export (HTML5)

---

## Open Questions
_Major unresolved design questions that need answers._

1. 
2. 
3. 

---

## Notes
_Add general design notes, inspiration, and references here._
