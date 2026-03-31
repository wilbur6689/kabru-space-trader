# Planets and Systems

## Overview
This document defines how planetary systems are structured, generated, categorized, and how their state changes over time. Systems are organized into **regional clusters** with hub-and-spoke topology.

---

## Galaxy Structure

### Hierarchy
```
Galaxy
└── Regional Clusters
    ├── Hub Systems (1-2 per cluster)
    └── Sub-Systems (several per cluster)
```

### Hub Systems (Major)
- 1-2 per regional cluster
- Serve as the center of regional activity
- Respawn points for player death
- Shipyards for purchasing new ships
- Full merchant access and services
- Main quest events and story progression
- **Bulletin boards** listing known neighboring system addresses
- NPCs and traders who share intel, tips, and trade route info
- Display **friendly/hostile status** of known neighboring systems

### Sub-Systems (Minor)
- Connected to hub systems within a cluster
- Industrial colonies — manufacturing and production
- Mining colonies — raw material extraction
- Side quest locations
- Trading outposts
- Random events and encounters

---

## 6-Digit Address System

### How It Works
- Every system in the galaxy has a unique **6-digit numerical address** (e.g., 384721)
- Most addresses map to **empty space** — nothing at that location
- Populated addresses contain hub systems, sub-systems, dead systems, or hostile systems
- Regional networks are clusters of populated addresses near each other
- A procedural algorithm determines whether a given address belongs to a regional network

### Address Discovery
- Hub bulletin boards list known addresses in the region
- NPCs and traders share specific addresses and tips
- Star charts can be purchased to unlock batches of addresses
- Mission briefings provide destination addresses automatically
- Players can input **any number** and see what's there — rewarding curiosity

---

## System Types

### Friendly Systems
Welcoming, civilized systems with established governance.

**Discoverable Locations:**
- Merchants / Trading posts
- Shipyards
- Resource deposits
- Black markets
- Quest givers / NPCs
- Refueling stations

**Sub-Types:**
| Sub-Type | Description | Key Feature |
|----------|-------------|-------------|
| Uninhabited Planet | Empty world, no civilization | Resource gathering, low danger |
| Primitive Race | Pre-technology species | Unique trade goods, cultural items |
| Mining Colony | Resource extraction settlement | Cheap raw materials, ore, minerals |
| Industrial Colony | Manufacturing hub | Machinery, alloys, manufactured goods |

### Dead Systems
Lifeless, abandoned, or destroyed systems with no active civilization.

**Discoverable Locations:**
- Salvage wreckage
- Ancient ruins / artifacts
- Resource deposits (uncontested but dangerous)
- Derelict ships
- Hidden caches from previous inhabitants

**Sub-Types:**
| Sub-Type | Description | Key Feature |
|----------|-------------|-------------|
| No Planets | Empty void, just a star | Minimal discoveries, quick pass-through |
| Dead World | Destroyed inhabitants, ruins | Salvage, lore, artifacts |
| Asteroid Field | Broken planet chunks | Mining opportunities, navigation hazards |
| Destroyed World Chunks | Remnants of a catastrophe | High-value salvage, dangerous terrain |

### Hostile Systems
Similar to friendly systems in structure, but controlled by an evil gang or army.

**Discoverable Locations:**
- Enemy outposts (combat risk)
- Imprisoned merchants (rescue for loyalty/discounts)
- Stolen cargo (high value loot)
- Black markets (better prices but more dangerous)
- Intel / data caches (reveal info about other systems)

**Key Mechanics:**
- Controlled by hostile factions (gangs, warlords, rogue armies)
- Higher risk but higher reward than friendly systems
- Player can **eliminate the threat** to convert a hostile system to friendly
- Clearing hostiles gives a strong sense of progression

---

## System State and Dynamics

### Hostility Changes
- Previously visited **friendly systems** have a small chance of becoming **hostile** depending on neighboring system loyalties
- **Hostile systems** can become **friendly** if the player eliminates the controlling faction
- Neighboring system allegiances influence each other over time
- Clearing a hostile system may stabilize neighboring friendly systems

### System Events
- Any system can be marked with a **random or preprogrammed event**
- Events include: mission triggers, enemy attacks, special discoveries, story arc moments
- Events can fire when the player first arrives or on subsequent visits
- New regional entries always have a chance to trigger side missions or main story events

---

## Discovery System

### Progressive Exploration
- Each system has a **variable number** of hidden discoverable locations
- Number of locations varies by system type and size
- Each **search action costs one turn** and reveals the next hidden location
- The game engine procedurally generates encounters within each system group

### Persistent Discovery Database
- All discoveries are **permanently stored** in the database
- Revisiting a system shows all previously discovered locations accessible
- System overview screen displays: **"X / Y locations discovered"**
- Players never need to re-discover known locations

### Discovery Risk
- Searching can reveal **dangers** — pirate hideouts, enemy bases, ambushes
- Discovery is a core risk/reward mechanic
- Better equipment (scanners) may influence discovery outcomes (future design)

---

## Planet Attributes

### Per-System Properties
- Name (generated or predefined)
- 6-digit address
- System type (friendly, dead, hostile)
- Sub-type (mining colony, dead world, etc.)
- Number of discoverable locations
- Economy level (poor, developing, average, wealthy, booming)
- Danger level
- Controlling faction (if hostile)
- Description / Lore
- Discovered locations list
- Event flags

### Planet Economy Types
| Type | Produces | Demands | Notes |
|------|----------|---------|-------|
| Agricultural | Food, Livestock, Textiles | Machinery, Medicine | Friendly sub-type |
| Industrial | Machinery, Alloys, Weapons | Raw Ore, Food | Sub-system focus |
| Mining | Raw Ore, Minerals, Gems | Food, Medicine, Machinery | Sub-system focus |
| Tech Hub | Electronics, Software, Luxury | Raw Materials, Food | Hub system feature |
| Outpost | Fuel, Salvage | Everything | Remote, often lawless |

---

## Dynamic Economy

### Stock Market Price Model
- Trade good prices fluctuate **like a stock market** across all systems
- Prices shift based on supply, demand, and world events
- Buying/selling at a system affects local supply and demand
- Events (wars, disasters, booms) create price spikes and crashes
- Creates opportunities for traders who track price trends

### Price Factors
- Base price per commodity
- System type modifier (producers = cheap, consumers = expensive)
- Economy level modifier
- Dynamic supply/demand modifier
- Random fluctuation range
- Event modifier (war, famine, boom, shortage)

---

## Navigation Between Systems

### Jump Drive
- Travel between systems is gated by the ship's **jump drive range**
- No fuel cost or movement points for travel
- Jump drive upgrades unlock access to more distant systems
- This is the primary **exploration progression gate**

### Route Calculation
- Player inputs a 6-digit destination address
- Ship calculates the **shortest jump path** through intermediate systems
- Ship reports the **number of jumps required**
- Multi-hop routes may pass through unknown territory

### Travel Risk
- **Unknown territory**: Slim chance of encounters when passing through
- **Known routes**: Much lower chance of negative events
- **New regions**: Always have a chance to trigger side missions or story events

---

## System Services

### Available at Hub Systems
- Full marketplace
- Shipyard (buy/sell ships)
- Repair dock
- Refueling
- Mission board / Bulletin board
- NPC quest givers
- Star chart vendors
- Information brokers

### Available at Sub-Systems
- Limited marketplace (based on colony type)
- Basic repair services
- Refueling
- Side quest contacts
- Resource trading

### Available at Hostile Systems (if discovered)
- Black market (high-risk, high-reward trading)
- Intel caches
- Imprisoned merchants (rescuable)
- Stolen cargo opportunities

---

## Procedural Generation

### System Generation
- [ ] Algorithm for determining if a 6-digit address is populated
- [ ] Cluster generation — how hubs and sub-systems are grouped
- [ ] System type distribution (ratio of friendly/dead/hostile per cluster)
- [ ] Location count ranges per system type
- [ ] Discovery content generation per system
- [ ] Event placement across systems

### Regional Networks
- [ ] How many systems per cluster
- [ ] How clusters connect to each other
- [ ] Distance/jump requirements between clusters
- [ ] Regional difficulty scaling (closer clusters = easier)

---

## Notes
_Add design notes, open questions, and decisions here._
