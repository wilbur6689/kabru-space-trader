# Core Game Loop

## Overview
This document defines the primary game loop and how players interact with the space trading game at every level.

---

## Main Loop Structure

### Turn-Based System
- The game is **turn-based** — only **searching** and **traveling** advance the game clock
- All other actions (visiting locations, buying, selling, trading, interacting with NPCs) are **free actions** that do not consume a turn
- The turn count is a background tracker with no direct mechanical effect
- The world reacts to **player actions**, not to time passing

### Player Action Flow
```
Start Turn
  -> Player is in a planetary system
  -> View system overview (name, type, danger level, discovery progress "X / Y locations")
  -> Choose action:
     a) SEARCH - Discover the next hidden location (costs 1 turn)
     b) VISIT - Interact with a discovered location (free action)
     c) TRAVEL - Input a 6-digit address and jump to another system
  -> If SEARCH: Reveal next location, add to persistent database
  -> If VISIT: Open location interaction (trade, quest, repair, etc.)
  -> If TRAVEL: Plot route, execute jumps, arrive at destination
  -> Update world state (prices, system hostility, events)
  -> Repeat
```

---

## Galaxy Structure

### Hierarchy
```
Galaxy
└── Quadrants (Q01-Q04)
    └── Regions (12x12 grid per quadrant, 10x10 usable core)
        └── Systems (200 populated per region, each with discoveries)
```

### System Population Tiers
- **Major** (12-18 discoveries): Large, bustling systems — full services, missions, NPCs
- **Moderate** (7-11 discoveries): Mid-sized systems with solid services
- **Minor** (3-6 discoveries): Small systems with basic services
- **Measly** (3 discoveries): Bare minimum — merchant, repair, refueling

### System Alignments
- **Friendly**: Civilized, safe, governed
- **Neutral**: Independent, self-governing
- **Hostile**: Dangerous environment (wildlife, contamination)
- **Pirate-Controlled**: Occupied by a pirate faction
- **Dead**: Lifeless, abandoned ruins

---

## System Discoveries
Any system can contain any type of discovery. Population tier determines how many.

**Services**: Merchants, weapon/engine/armor/jump drive upgrades, shipyards, repair, refueling, mission boards, black markets, information brokers, NPC quest givers

**Loot**: Hidden caches, salvage, resource deposits, ancient ruins, derelict ships, intel caches, ghost towns

**Encounters**: Pirate hideouts, enemy outposts, ambushes, distress signals, hostile wildlife, contaminated zones, space anomalies

### System State Changes
- Alignment can shift over time based on player actions and neighboring system loyalties
- Pirate-Controlled systems can become Friendly if the player eliminates the controlling faction
- Previously Friendly systems can become Pirate-Controlled or Hostile depending on regional dynamics

---

## Search and Discovery

### Progressive Exploration
- Each system has a **variable number** of discoverable locations depending on population tier
- Each search action uses **one turn** and reveals the **next hidden location** in the system
- The game engine **procedurally generates** the discoverable encounters within each group of systems
- Discoveries are **dangerous** — searching may reveal pirate hideouts, enemy bases, or other threats

### Persistent Discovery Database
- All discovered locations are **permanently stored** in the database
- Revisiting a system shows all previously discovered locations still accessible
- The system overview screen displays **discovery progress** (e.g., "3 / 7 locations discovered")
- Players never have to re-discover locations they've already found

---

## Navigation

### Quadrant-Region Address System
- The galaxy is divided into **4 quadrants** (Q01-Q04) around a central origin
- Every system has a unique address: **Q[quadrant]-R[xx][yy]-[system]** (e.g., Q02-R0307-4821)
- Each quadrant has a **12x12 grid** of regions, with a **10x10 usable core** and a 2-cell dead space border
- **Most addresses are empty space** — nothing at that location
- **Populated addresses** contain hub systems, sub-systems, dead systems, or hostile systems

### Jump Travel
- Player inputs a **destination address** into the ship's jump drive
- **Inter-region jumps**: Ship calculates the route through intermediate regions — **no pass-thru events**, only the destination can trigger arrival events
- **Intra-region travel**: Moving between systems within a region routes through intermediate systems — **pass-thru events may occur** based on danger zone and system contents
- Jump range is limited by the ship's **jump drive tier** (Manhattan distance from current region)

### Travel Events
- **Intra-region pass-thru**: Events triggered by what exists at systems along the route (ambushes, distress signals, turf wars)
- **Pass-thru event chance**: Scaled by danger zone — low near center, high at edges
- **New regions**: Arrival events can trigger side missions or main story arc events

---

## Information Gathering

Players can learn about new system addresses and destinations through multiple channels:

### Bulletin Boards
- Located at hub systems
- List known system addresses, trade routes, and warnings
- Updated as the game world changes

### NPCs and Traders
- Talking to merchants or locals reveals specific addresses
- Tips like "Q01-R0305-2410 has cheap ore"
- Warnings like "Avoid deep Q04 — Void Reavers"

### Purchased Star Charts
- Pay credits to unlock a batch of known addresses in a region

### Mission Briefings
- Accepting a quest gives the destination address automatically

---

## Dynamic World

### Price Fluctuations
- Trade good prices fluctuate **like a stock market** across all systems
- Prices shift based on supply, demand, and world events
- Creating opportunities for savvy traders who track the market

### System Hostility
- System allegiance can shift based on player actions and neighboring system loyalties
- Clearing hostile threats from a system changes it to friendly
- Neglected regions can fall to hostile forces over time

---

## Death and Respawn

### When the Player Dies
1. Player **respawns at the last Friendly Major or Moderate system** they visited
2. A **basic loaner/escape pod** is provided for travel
3. The player's ship remains **where they died**
4. **0% to 50% of cargo** in the abandoned ship is looted by enemies
5. Player can travel back to recover their ship and remaining cargo
6. Player can also **buy a new ship** at the merchant if they have credits

---

## Win/Lose Conditions

### No Permanent Game Over
- Death is a **setback, not an ending** — player always respawns
- The player can always recover from any situation

### Story Progression
- **Main story arc**: Player-triggered, will be developed in the future
- **Side quests**: Available throughout the game at various systems
- Story only advances when the player **chooses to engage** — no timers or pressure

---

## Tutorial / Opening Sequence

### Starting Scenario: Stranded
1. Player begins **stranded** with a **broken ship** orbiting a lesser-known planet in a Minor system within Q01-R0101
2. The ship broke down while on a delivery run to pick up cargo
3. Player must **repair the ship** by finding parts and resources
4. Player must **purchase new components** to get flight-worthy
5. Player figures out how to **navigate** to the destination
6. Player **delivers the cargo** to a second system, then is directed to **Earth** (Q01-R0101-1000)
7. Upon delivery, the **tutorial ends**
8. The player discovers the **main story hook** at the hub station
9. The open galaxy is now available to explore freely

### Tutorial Teaches
- Searching and discovery mechanics
- Repairing and upgrading the ship
- Trading and purchasing
- Navigation using the quadrant-region address system
- Interacting with NPCs and locations
- Traveling between systems

---

## Notes
_Add design notes, open questions, and decisions here._

### Open Design Questions
- Exact procedural generation algorithm for system clusters and addresses
- How many locations per system type (ranges to be defined)
- Specific encounter/event probability tables for travel
- Detailed price fluctuation model (stock market simulation)
- Main story arc details (to be developed separately)
