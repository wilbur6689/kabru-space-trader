# Deployment Overview

## Development Roadmap

This document maps the complete development plan for the space trading game built in Godot 4.3+. Development is organized into **10 stages**, each containing multiple **phases**. Every phase builds on the previous one, and every stage builds on the stages before it.

### Guiding Principles
- **Playable early, polished late** — the game should be testable at the end of each stage
- **Foundation first** — data models and autoloads before gameplay systems
- **One loop at a time** — get search/visit/travel working before adding trading, combat, etc.
- **Placeholder everything** — use simple UI, debug text, and test data until polish stages
- **Test each phase** — verify before moving on; bugs compound across stages

---

## Stage Map

| Stage | Name | Focus | Playable Milestone |
|-------|------|-------|--------------------|
| [01](01_foundation.md) | Foundation | Godot project, architecture, data models, autoloads | Project runs, singletons load |
| [02](02_galaxy_and_navigation.md) | Galaxy & Navigation | Galaxy generation, Q-R addresses, system data, jump routing | Can generate galaxy, navigate between systems |
| [03](03_core_game_loop.md) | Core Game Loop | Turn system, search, visit, travel, basic cockpit UI | Can search, discover locations, and travel between systems |
| [04](04_economy_and_trading.md) | Economy & Trading | Trade goods, stock market pricing, marketplace UI, cargo | Can buy low, sell high, see profit |
| [05](05_pilot_and_progression.md) | Pilot & Progression | Skills, XP, fame, reputation, pilot creation | Skills level up, fame unlocks features |
| [06](06_combat.md) | Combat | Turn-based combat, enemies, fame checks, loot | Can fight, flee, intimidate, die and respawn |
| [07](07_missions_and_events.md) | Missions & Events | Mission system, travel encounters, hazards, pass-thru events | Can accept and complete missions, encounter events while traveling |
| [08](08_advanced_systems.md) | Advanced Systems | Pirate clearing, player stations, black market, bounty hunters | Can clear pirate systems, build stations, run contraband |
| [09](09_story_and_tutorial.md) | Story & Tutorial | Tutorial sequence, story arcs, NPCs, narrative content | Full new-game experience, story progression |
| [10](10_polish_and_release.md) | Polish & Release | Audio, UI art, balancing, performance, platform export | Release-ready build |

---

## Dependency Flow

```
Stage 01: Foundation
    └── Stage 02: Galaxy & Navigation
        └── Stage 03: Core Game Loop
            ├── Stage 04: Economy & Trading
            │   └── Stage 07: Missions & Events (needs cargo, economy)
            ├── Stage 05: Pilot & Progression
            │   └── Stage 06: Combat (needs fame, skills)
            │       └── Stage 08: Advanced Systems (needs combat, economy, missions)
            │           └── Stage 09: Story & Tutorial (needs all systems)
            └────────────── Stage 10: Polish & Release (needs everything)
```

Note: Stages 04 and 05 can be developed in parallel after Stage 03 is complete. Stage 06 depends on Stage 05 (fame/skills). Stage 07 depends on Stage 04 (cargo/economy). Stage 08 depends on Stages 06 and 07.

---

## Cross-Stage References

Each stage document references:
- **Design Docs** — which design documents are being implemented
- **Prerequisites** — what must be complete before starting
- **Outputs** — what this stage produces for later stages
- **Verification** — how to confirm each phase works before moving on
