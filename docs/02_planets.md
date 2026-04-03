# Planets and Systems

## Overview
This document defines how planetary systems are structured, generated, categorized, and how their state changes over time. Systems are organized into **regional clusters** with hub-and-spoke topology, spread across a spiral galaxy layout.

---

## Galaxy Structure

> **Detailed in:** [02a_galaxy_structure.md](02a_galaxy_structure.md)

### Grid-Based Quadrant System
- Galaxy divided into **4 quadrants** (Q01-Q04) around central origin (0,0)
- **12x12 grid per quadrant**, 10x10 usable core, 2-cell dead space border
- Origin (0,0) is logic-only — not accessible in-game
- Earth at **Q01-R0101-1000**, 4 safe zone regions at R0101 in each quadrant

### Address Format
- Full system address: **Q[quadrant]-R[xx][yy]-[system]** (e.g., Q02-R0307-4821)
- 1,000 valid system addresses per region (constrained by digit rules)
- **200 populated systems** per region, weighted by danger zone

### Danger Scaling
- **Manhattan distance** from origin determines danger: |x| + |y|
- Safe (1-3) → Low-Med (4-6) → Med-High (7-9) → Extreme (10+)

### Pirate Factions
- Q01 = Silk Hand (easiest), Q02 = Phantom Circuit, Q03 = Rust Collective, Q04 = Void Reavers (hardest)
- 2-region faction blending at quadrant borders

---

## Hub Systems

### Hub Overview
- 1 hub per cluster region
- Serve as the center of regional activity
- **Respawn points** for player death
- Main quest events and story progression
- All core services are **immediately known** on arrival (not hidden)
- **1-3 hidden discoverable locations** (secret contacts, black market entrances, side quest triggers)

### Hub Specializations
Each hub has a unique specialization. The system is **modular** — new specializations can be added over time.

| Specialization | Focus | Unique Services |
|---|---|---|
| **Trade Hub** | Commerce center | Best marketplace prices, widest goods selection, trade route intel |
| **Military Hub** | Defense and combat | Weapons dealer, bounty board, combat missions, patrol info |
| **Tech Hub** | Engineering and upgrades | Best ship upgrades, jump drive specialists, scanner equipment |
| **Shipyard Hub** | Ship manufacturing | Widest ship selection, custom builds, best trade-in values |
| **Outlaw Hub** | Underground economy | Open black market, smuggling missions, no patrol scans |
| **Science Hub** | Research and exploration | Artifact appraisal, exploration missions, dead system intel |

### Hub Services (Always Known on Arrival)
- Full marketplace
- Shipyard (buy/sell ships — selection varies by specialization)
- Repair dock
- Refueling
- Mission board / Bulletin board
- NPC quest givers
- Star chart vendors
- Information brokers
- Specialization-specific services

---

## Sub-Systems

### Sub-System Overview
- 3-5 per small cluster, 6-10 per story cluster
- **Tied to the hub** as part of its supply chain
- Sub-system types match the hub's specialization (mining colonies feed a shipyard hub, weapons factories feed a military hub, etc.)
- Have **small specialized merchants** that sell ship parts and goods at **wholesale discount** but with **much more limited supply**
- Creates a trade-off: cheap prices at subs but limited stock vs. full selection at hub but higher prices
- Some services are **known on arrival**, additional **2-4 hidden discoverable locations**

### Sub-System Types
| Sub-Type | Description | Key Feature | Typical Hub Connection |
|----------|-------------|-------------|----------------------|
| Mining Colony | Resource extraction settlement | Cheap raw materials, ore, minerals | Shipyard, Industrial hubs |
| Industrial Colony | Manufacturing hub | Machinery, alloys, manufactured goods | Military, Tech hubs |
| Agricultural Settlement | Food and textile production | Cheap food, livestock, textiles | Trade, any hub |
| Uninhabited Planet | Empty world, no civilization | Resource gathering, low danger | Science, any hub |
| Primitive Race | Pre-technology species | Unique trade goods, cultural items | Science, Trade hubs |
| Weapons Factory | Arms manufacturing | Cheap weapons, munitions | Military hub |
| Tech Outpost | Research station | Electronics, software, scanner parts | Tech, Science hubs |

### Sub-System Services (Known on Arrival)
- Limited marketplace (based on colony type)
- Basic repair services
- Refueling
- Wholesale specialist merchant (limited stock, discounted prices)

---

## Unaffiliated Systems

### Overview
- Scattered throughout regions, **not tied to any hub**
- **Hard to find** — only discoverable by punching in random addresses or through rare intel
- Can be friendly or hostile
- **High reward but temporary**:
  - **One-time caches**: Loot and leave, resources/goods gone forever
  - **Depleting supply**: Finite stock that decreases with each visit, eventually empty
  - **Price decay**: Goods are available but sell price drops as the player floods the market (stock market model handles this naturally)
- Mix of the above — each unaffiliated system is different
- No known services on arrival — everything must be discovered through searching
- Found in both hub regions and wild space regions

---

## Wild Space Regions

### Overview
- ~50% of regions are wild space (7-10 regions)
- **No hub system** — no respawn point, no safe harbor
- Contains scattered unaffiliated systems, dead systems, and empty space
- Venturing into wild space is a **serious commitment**
- If the player dies in wild space, they respawn at their **last visited hub** — potentially many jumps away
- Ship remains at death location in wild space — recovery is difficult and dangerous
- **Highest risk, highest reward** exploration territory

---

## System Types

### Friendly Systems
Welcoming, civilized systems with established governance. Some services are **known on arrival**.

**Known Locations (Immediate):**
- Basic merchant / trading post
- Repair services (if available)
- Refueling

**Hidden Discoverable Locations (2-4):**
- Additional merchants / specialized traders
- Resource deposits
- Black markets
- Quest givers / NPCs
- Hidden caches

### Dead Systems
Lifeless, abandoned, or destroyed systems with no active civilization. **Nothing known on arrival** — everything must be searched for. Ghost towns still exist and can be plundered.

**Hidden Discoverable Locations (3-6):**
- Salvage wreckage
- Ancient ruins / artifacts
- Resource deposits (uncontested but dangerous)
- Derelict ships
- Hidden caches from previous inhabitants
- Ghost towns with abandoned supplies

**Properties:**
- Scattered individually throughout regions (not clustered)
- **Permanently dead** — cannot be repopulated or restored
- No lore connection for now (may be updated later)
- Environmental hazards during exploration

**Sub-Types:**
| Sub-Type | Description | Key Feature |
|----------|-------------|-------------|
| No Planets | Empty void, just a star | Minimal discoveries, quick pass-through |
| Dead World | Destroyed inhabitants, ruins | Salvage, ghost towns, artifacts |
| Asteroid Field | Broken planet chunks | Mining opportunities, navigation hazards |
| Destroyed World Chunks | Remnants of a catastrophe | High-value salvage, dangerous terrain |

### Hostile Systems
Controlled by an evil gang or army. **Nothing known on arrival** — everything hidden until the player explores or clears the threat.

**Hidden Discoverable Locations (2-5):**
- Enemy outposts (combat risk)
- Imprisoned merchants (rescue for loyalty/discounts)
- Stolen cargo (high value loot)
- Black markets (better prices but more dangerous)
- Intel / data caches (reveal info about other systems)

**Key Mechanics:**
- Controlled by hostile factions (gangs, warlords, rogue armies)
- Higher risk but higher reward than friendly systems
- Player can **eliminate the threat** to convert the system
- **After clearing**: System reverts to its **original pre-occupation state** as a basic friendly system with the basic services it had before the gang took over
- Previously hidden locations may become known/accessible after clearing

---

## Discovery System

### Discoverable Location Counts
| System Type | Hidden Locations | Notes |
|-------------|-----------------|-------|
| Hub System | 1-3 | Core services already known; hidden locations are bonus extras |
| Friendly Sub-System | 2-4 | Basic services known; hidden locations are additional finds |
| Dead System | 3-6 | Nothing known; everything must be searched |
| Hostile System | 2-5 | Nothing known until explored or cleared |

### Progressive Exploration
- Each system has a **variable number** of hidden discoverable locations
- Each **search action costs one turn** and reveals the next hidden location
- The game engine procedurally generates encounters within each system group
- System overview screen displays: **"X / Y locations discovered"**

### Persistent Discovery Database
- All discoveries are **permanently stored** in the database
- Revisiting a system shows all previously discovered locations accessible
- Players never need to re-discover known locations

### Discovery Risk
- Searching can reveal **dangers** — pirate hideouts, enemy bases, ambushes
- Discovery is a core risk/reward mechanic
- Better equipment (scanners) may influence discovery outcomes (future design)

---

## System State and Dynamics

### Hostility Changes
- Previously visited **friendly systems** have a small chance of becoming **hostile** depending on neighboring system loyalties
- **Hostile systems** can become **friendly** if the player eliminates the controlling faction
- Cleared hostile systems revert to their **pre-occupation state** with basic services
- Neighboring system allegiances influence each other over time
- Clearing a hostile system may stabilize neighboring friendly systems

### Distance-Based Danger Scaling
- Danger level weighted by **distance from starting region (00)**
- Higher region numbers = heavier weight toward hostile and dangerous systems
- Starting region is safe and friendly
- Distant regions have more hostile systems, tougher enemies, better loot
- Wild space regions are the most dangerous regardless of number

### System Events
- Any system can be marked with a **random or preprogrammed event**
- Events include: mission triggers, enemy attacks, special discoveries, story arc moments
- Events can fire when the player first arrives or on subsequent visits
- New regional entries always have a chance to trigger side missions or main story events

---

## Starting Region (Q01-R0101)

> **Detailed in:** [02a_galaxy_structure.md](02a_galaxy_structure.md)

### Composition
- **Earth** at Q01-R0101-1000 — main hub, Space Force HQ, main story start
- **2-3 Friendly Sub-Systems** — including the tutorial starting system
- **1 Known Hostile System** — first clearance target
- **0-1 Dead Systems** — optional early exploration

### Tutorial Flow
1. Player starts stranded at a sub-system in Q01-R0101
2. Repairs ship, learns basics
3. Flies to second sub-system to deliver goods (learns main story hook)
4. Directed to fly to Earth (Q01-R0101-1000) to begin main story

---

## Empty Space

### Jumping to Empty Addresses
- The vast majority of addresses in any region are **empty space**
- When a player jumps to an empty address, they **commit to the jump** (it costs travel like any other)
- On arrival, the ship's computer displays a **brief atmospheric scanner readout**
- Examples: "Scanner sweep complete. Nothing but distant starlight and cosmic dust." / "Faint radiation signatures detected — remnants of an ancient stellar event. No systems present."
- Empty space jumps carry the same **travel encounter risk** as any jump through unknown territory

---

## Player Stations

### Overview
- Players can build **personal stations** at empty space addresses
- Maximum **3 stations** total across the galaxy
- Maximum **1 station per region**
- Stations are placed at otherwise empty addresses, turning them into personal outposts

### Station Tiers
| Tier | Services | Risk |
|------|----------|------|
| Tier 1 | Storage only — drop off and pick up cargo | None |
| Tier 2 | Storage + basic repair | Low — minor attention |
| Tier 3+ | Additional services (TBD) | Higher tiers **attract pirates** |

### Station Building
- Requires a special ship type or module (details TBD)
- Player flies to an empty space address and deploys the station
- Station persists at that address permanently
- Exact building mechanics, costs, and ship requirements to be designed later

### Strategic Value
- Stash goods to sell later when prices are right
- Create a foothold in wild space for deep exploration
- Reduce travel distance for trade routes
- Higher tier upgrades add more services but increase risk

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
- Wholesale discount at sub-systems (limited stock)

---

## Navigation Between Systems

> **Detailed in:** [02a_galaxy_structure.md](02a_galaxy_structure.md)

### Jump Drive Tiers (Manhattan Distance)
| Tier | Max Distance | Notes |
|------|-------------|-------|
| Tier 1 | 2 regions | Starting safe zone |
| Tier 2 | 5 regions | Nearby quadrant territory |
| Tier 3 | 9 regions | Mid-game, most hubs reachable |
| Tier 4 | 14 regions | Deep space, far edges |
| Tier 5 | Unlimited | Full galaxy access |

### Travel Model
- **Inter-region jumps**: No pass-thru events, only destination arrival events
- **Intra-region travel**: Pass-thru events may occur at intermediate systems, scaled by danger zone

---

## Procedural Generation

### System Naming
- All system names are **procedurally generated** (e.g., "Kepler-7", "Voss Prime", "NX-4821")
- Scalable and consistent with the modular approach
- Name generation algorithm TBD

### System Generation
- [ ] Algorithm for determining if a system address is populated (within 1,000 valid addresses per region)
- [ ] System type distribution per region (weighted by Manhattan distance — see 02a distribution table)
- [ ] Location count generation per system type
- [ ] Discovery content generation per system
- [ ] Event placement across systems
- [ ] Unaffiliated system placement and reward generation

### Regional Networks
- [ ] Hub-to-sub-system relationship generation per region
- [ ] Distance-based danger weight formula using Manhattan distance
- [ ] Pirate faction blending at quadrant borders

---

## Notes
_Add design notes, open questions, and decisions here._

### Open Design Questions
- Procedural name generation approach for systems
- Station building: ship type, costs, deployment mechanics
- Station tier upgrade costs and pirate raid mechanics
- How cleared hostile systems integrate back into the hub supply chain
- Unaffiliated system generation — rarity and content distribution
- Dead system lore (future consideration)
