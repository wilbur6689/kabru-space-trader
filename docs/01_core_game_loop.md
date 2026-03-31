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
└── Regional Clusters
    ├── Hub Systems (1-2 per cluster) — major cities of space
    └── Sub-Systems (several per cluster) — smaller outposts
```

### Hub Systems (Major)
- Respawn points for player death
- Shipyards for buying new ships
- Full merchant access and services
- Main quest events and story progression
- Bulletin boards with known neighboring system addresses
- NPCs and traders who share intel, tips, and warnings

### Sub-Systems (Minor)
- Industrial colonies
- Mining colonies
- Side quest locations
- Trading outposts
- Random events and encounters

---

## System Types

### Friendly Systems
Discoverable locations include:
- Merchants / Trading posts
- Shipyards
- Resource deposits
- Black markets
- Quest givers / NPCs
- Refueling stations

### Dead Systems
Discoverable locations include:
- Salvage wreckage
- Ancient ruins / artifacts
- Resource deposits (uncontested but dangerous)
- Derelict ships
- Hidden caches from previous inhabitants

### Hostile Systems
Ruled by an evil gang or army. Discoverable locations include:
- Enemy outposts (combat risk)
- Imprisoned merchants (rescue for loyalty/discounts)
- Stolen cargo (high value loot)
- Black markets (better prices but more dangerous)
- Intel / data caches (reveal info about other systems)

### System State Changes
- Systems can change from friendly to hostile based on neighboring system loyalties
- Hostile systems can become friendly if the player **eliminates the threat**, giving a sense of progression
- Previously visited friendly systems have a small chance of becoming hostile depending on neighboring systems

---

## Search and Discovery

### Progressive Exploration
- Each system has a **variable number** of discoverable locations depending on system type and size
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

### 6-Digit Address System
- Every system in the galaxy has a unique **6-digit numerical address** (e.g., 384721)
- **Most addresses are empty space** — nothing at that location
- **Populated addresses** contain hub systems, sub-systems, dead systems, or hostile systems
- Regional networks are clusters of populated addresses near each other

### Jump Travel
- Player inputs a **destination address** into the ship's jump drive
- Ship calculates the **shortest jump path** through various systems to reach the destination
- Ship reports the **number of jumps required** to arrive
- Jump range is limited by the ship's **jump drive capability** — upgrades unlock farther destinations

### Travel Events
- **Unknown territory**: Slim chance of triggering encounters/events when passing through
- **Known routes**: Much lower chance of negative events
- **New regions**: Always have a chance to trigger side missions or main story arc events
- Player can potentially encounter events at intermediate systems along the route

---

## Information Gathering

Players can learn about new system addresses and destinations through multiple channels:

### Bulletin Boards
- Located at hub systems
- List known system addresses, trade routes, and warnings
- Updated as the game world changes

### NPCs and Traders
- Talking to merchants or locals reveals specific addresses
- Tips like "System 482103 has cheap ore"
- Warnings like "Avoid the 55xxxx region — pirates"

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
1. Player **respawns at the last merchant** they visited
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
1. Player begins **stranded** with a **broken ship** orbiting a lesser-known planet in a sub-system
2. The ship broke down while on a delivery run to pick up cargo
3. Player must **repair the ship** by finding parts and resources
4. Player must **purchase new components** to get flight-worthy
5. Player figures out how to **navigate** to the destination
6. Player **delivers the cargo** to a city space station in the **main hub** of the region
7. Upon delivery, the **tutorial ends**
8. The player discovers the **main story hook** at the hub station
9. The open galaxy is now available to explore freely

### Tutorial Teaches
- Searching and discovery mechanics
- Repairing and upgrading the ship
- Trading and purchasing
- Navigation using the 6-digit address system
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
