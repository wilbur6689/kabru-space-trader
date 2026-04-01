# Deferred Design

## Overview
This document tracks complex design decisions, algorithms, and mechanics that have been identified but intentionally deferred for later development. Each item includes context on what needs to be solved and where it was first identified.

---

## Navigation and Galaxy Layout

### Spiral Layout Algorithm
- **Source:** 02_planets.md
- **Problem:** How do region numbers (00-99) map to physical positions on a spiral galaxy layout?
- **Context:** Regions close on the spiral form natural clusters but aren't necessarily sequential (e.g., regions 3-5, 12-16, 34-37 could each be nearby). Lower numbers = closer to origin = safer.
- **Needs:** A mapping function from region number → position that produces a spiral pattern with increasing danger outward.

### Distance Calculation Between Regions
- **Source:** 02_planets.md
- **Problem:** How is "distance" measured between two regions for jump drive range checks?
- **Context:** Jump drive tiers allow travel to regions within 10/20/unlimited distance. Distance must relate to spiral position, not just number difference.
- **Needs:** A distance formula that works with the spiral layout.

### Jump Path Routing Algorithm
- **Source:** 01_core_game_loop.md, 02_planets.md
- **Problem:** When the player inputs a destination address, how does the ship calculate the shortest jump path through intermediate systems?
- **Context:** The ship must find a route through known/unknown systems, report jump count, and each intermediate hop carries encounter risk.
- **Needs:** Pathfinding algorithm adapted for the region/address system.

---

## Procedural Generation

### Address Population Algorithm
- **Source:** 02_planets.md
- **Problem:** How does the game determine which XX-XXXX addresses are populated vs. empty space?
- **Context:** Each region has 10,000 possible addresses. ~20-50 should be populated. 3-5 are known hub sub-systems, the rest are unaffiliated, dead, or hostile.
- **Needs:** A seeded generation function that deterministically maps addresses to system types (or empty).

### System Name Generation
- **Source:** 02_planets.md
- **Problem:** All system names are procedurally generated.
- **Needs:** A name generation algorithm that produces varied, sci-fi appropriate names (e.g., "Kepler-7", "Voss Prime", "NX-4821").

### Cluster Generation
- **Source:** 02_planets.md
- **Problem:** How are hub and sub-system groups created within a region?
- **Context:** Sub-systems are tied to the hub as suppliers. Their types should match the hub's specialization.
- **Needs:** Algorithm to generate a coherent cluster given a hub specialization.

### Discovery Content Generation
- **Source:** 02_planets.md
- **Problem:** How are discoverable locations generated for each system?
- **Context:** Each system type has different location counts and types. Content should be appropriate for the system type and regional danger level.
- **Needs:** Weighted random generation tables per system type.

---

## Economy

### Stock Market Simulation
- **Source:** 01_core_game_loop.md, 07_economy_and_trading.md
- **Problem:** How does the stock market pricing model work in detail?
- **Context:** Prices fluctuate dynamically based on supply, demand, player actions, and world events. Must feel like a real market with trends, spikes, and crashes.
- **Needs:** Price update algorithm, trend generation, event impact formulas, recovery rates.

### Unaffiliated System Reward Decay
- **Source:** 02_planets.md
- **Problem:** How do temporary rewards at unaffiliated systems diminish?
- **Context:** Three types — one-time caches (gone after pickup), depleting supply (finite visits), and price decay (market devaluation). Each needs different logic.
- **Needs:** Depletion rates, price decay curves, generation rules for which type each find uses.

---

## Player Stations

### Station Building Mechanics
- **Source:** 02_planets.md
- **Problem:** How does the player build a station? What ship type or module is required?
- **Context:** Player can build up to 3 stations (1 per region) at empty space addresses. Requires a special ship type that can undock a module.
- **Needs:** Ship class or module design, cost, deployment process, and what happens to the ship after deployment.

### Station Upgrade and Pirate Raid System
- **Source:** 02_planets.md
- **Problem:** How do station tier upgrades work, and how do higher tiers attract pirates?
- **Context:** Tier 1 = storage only (safe). Higher tiers add services but attract pirate raids. Player must balance upgrades vs. risk.
- **Needs:** Upgrade costs, materials, raid frequency formula, raid mechanics (what happens to stored goods).

---

## Combat

### Combat Damage Formulas
- **Source:** 05_challenges.md, 12_balancing.md
- **Problem:** Exact damage calculation, hit/miss rates, defense formulas.
- **Needs:** Balanced formulas that account for weapons, skills, ship stats, and enemy difficulty.

### Hostile System Clearing Mechanics
- **Source:** 01_core_game_loop.md, 05_challenges.md
- **Problem:** How many outposts must be cleared to convert a hostile system? How does the boss encounter work?
- **Context:** Player systematically discovers and eliminates enemy outposts, then defeats the faction leader to convert the system to friendly.
- **Needs:** Clearing progression requirements, boss encounter design, conversion triggers.

---

## Story

### Main Story Arc (Acts 1-3)
- **Source:** 08_story_and_narrative.md
- **Problem:** The full main story needs to be written.
- **Context:** Player-triggered, begins after tutorial delivery at the hub. Story advances only when the player engages.
- **Needs:** Plot outline, key missions, faction involvement, endings.

### Dead System Lore
- **Source:** 02_planets.md
- **Problem:** What destroyed the dead systems? Is there a connected backstory?
- **Context:** Currently no lore — dead systems are scattered and unexplained. May be updated later to connect to the main story.
- **Needs:** Decision on whether dead systems share a common origin or are independent.

---

## Template for New Items

```
### [Short Title]
- **Source:** [Document where this was identified]
- **Problem:** [What needs to be figured out]
- **Context:** [Relevant decisions already made]
- **Needs:** [What the solution must deliver]
```

---

## Ships

### Blueprint Crafting at Personal Stations
- **Source:** 03_ship.md
- **Problem:** How do blueprints found through exploration translate into ship upgrades at personal stations?
- **Context:** Players find blueprints in dead/hostile systems. These can be used at personal stations with gathered resources for unique upgrades not available at hubs. Stations must be at a sufficient tier.
- **Needs:** Blueprint types, required resources per blueprint, station tier requirements, crafting process, unique upgrade stats vs. standard upgrades.

### Damage Types and Shield Variants
- **Source:** 03_ship.md
- **Problem:** Should the game have multiple damage types (energy, kinetic, explosive) with shields that protect differently against each?
- **Context:** Currently using a single damage type to keep things simple. Shields have HP + recharge. Multiple damage types would add combat depth but increase complexity.
- **Needs:** Decision on number of damage types, how shields interact with each, weapon type assignments per enemy.

### Black Market Ship Visual Design
- **Source:** 03_ship.md
- **Problem:** Should black market ships be visually distinct from standard ships and draw patrol attention?
- **Context:** Black market ships have unique names and superior stats in specific areas. Currently they look the same as standard ships. They are sold only at Outlaw Hubs.
- **Needs:** Decision on unique visual design, whether they are flagged as illegal by patrols, and naming convention for black market variants.

---

## Pilot and Fame

### Crew System
- **Source:** 04_pilot.md
- **Problem:** Can the pilot hire crew members with roles, pay, morale, and story arcs?
- **Context:** Optional future extension. Crew could include engineer, gunner, navigator roles with stat bonuses. Would need management mechanics (pay, morale, loyalty) and potentially crew-specific side quests.
- **Needs:** Full crew system design — roles, hiring locations, costs, bonuses, morale system, crew story arcs.

### Pilot Status Effects
- **Source:** 04_pilot.md
- **Problem:** Should the pilot have temporary status effects that impact gameplay?
- **Context:** Currently no status effects — keeping it clean for initial design. Potential effects identified:
  - **Injury**: After losing combat or dangerous discovery, reduced skill effectiveness until healed at a hub
  - **Wanted bounty**: Active bounty placed by a faction, bounty hunters appear until paid off or time passes
  - **Pirate mark**: High pirate fame causes legitimate patrols to scan you more thoroughly
- **Needs:** Decision on which effects to include, duration mechanics, how they're applied and removed.

### Fame-Gated Story Events and Missions
- **Source:** 04_pilot.md
- **Problem:** Which story events and missions require specific fame levels to unlock?
- **Context:** Fame is earned every 5 combined skill levels (max 12). Certain events and missions should only trigger at higher fame levels. Details depend on main story arc.
- **Needs:** Fame level requirements per story event, mission tier gating, NPC access thresholds.

---

## Notes
_Add additional deferred items as they come up during design sessions._
