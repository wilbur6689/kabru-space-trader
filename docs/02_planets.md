# Planets and Systems

## Overview
This document defines how planetary systems are structured, generated, categorized, and how their state changes over time. Systems are organized into **regional clusters** with hub-and-spoke topology, spread across a spiral galaxy layout.

---

## Galaxy Structure

### Hierarchy
```
Galaxy (Spiral layout from origin)
└── Regions (15-20 total, numbered 00-99)
    ├── Hub Regions (~50% of regions) — contain a hub cluster
    │   ├── Hub System (1 per cluster, specialized)
    │   ├── Sub-Systems (3-5 per cluster, tied to hub)
    │   ├── Unaffiliated Systems (scattered, hidden)
    │   └── Dead Systems (scattered)
    └── Wild Space Regions (~50% of regions) — no hub
        ├── Unaffiliated Systems (scattered, hidden)
        └── Dead Systems (scattered)
```

### Region Format
- Every system has an address in the format **XX-XXXX** (e.g., 38-0322)
- **First 2 digits**: Region code (00-99)
- **Last 4 digits**: System address within that region (0000-9999)
- Most addresses within a region are **empty space**
- A moderate number of addresses (20-50) are populated per region
- 3-5 of those are "known" sub-systems tied to the hub

### Spiral Layout
- The galaxy is arranged in a **spiral pattern** radiating outward from the origin (Region 00)
- Region numbers generally **increase with distance** from the origin
- Regions that are **close on the spiral** form natural clusters, but aren't necessarily sequential (e.g., regions 3-5, 12-16, and 34-37 might each be nearby clusters on different arms)
- **Lower numbers** = generally closer to home = safer
- **Higher numbers** = generally further out = more dangerous
- Exact spiral algorithm TBD — key principle: distance from origin determines danger level

### Galaxy Size
- **15-20 regions** in the starting galaxy
- ~50% have hub clusters (8-10 hub regions)
- ~50% are wild space (7-10 wild regions)
- 5-8 clusters total for the main gameplay experience
- Story-critical clusters can be **medium sized** (6-10 sub-systems) while normal clusters are **small** (3-5 sub-systems)

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

## Starting Region (00)

### Composition
- **1 Hub System**: Regional hub where the tutorial delivery ends and main story begins
- **2-3 Friendly Sub-Systems**: Including the tutorial starting system (where the player is stranded)
- **1 Known Hostile System**: Well-known threat that NPCs warn the player about early — serves as the player's first clearance target and goal
- **0-1 Dead Systems**: Optional, for early exploration practice

### Tutorial Flow
1. Player starts **stranded** with a broken ship at a sub-system in region 00
2. The player knows only their current address (written on a **sticky note** on the dashboard)
3. Player repairs ship, talks to NPCs, learns hub address from **NPC dialogue** ("Don't you remember? It's XX-XXXX. It's also on your nav computer.")
4. Player discovers the address is also stored in the **ship's nav computer** (teaches that the nav computer stores addresses)
5. Player delivers cargo to the hub station
6. Tutorial ends, main story hook revealed
7. NPCs at the hub **warn about the nearby hostile system** — giving the player their first real goal

### What the Player Knows at Game Start
- Their current sub-system address (sticky note)
- Nothing else — all other addresses learned through gameplay

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

### Jump Drive Tiers
Travel between systems is gated by the ship's **jump drive range**. No fuel cost or movement points for travel.

| Tier | Region Range | Access | Notes |
|------|-------------|--------|-------|
| Tier 1 | Up to 10 regions from current location | Starting cluster + nearby regions | Starter drive |
| Tier 2 | Up to 20 regions from current location | Most of the galaxy | Mid-game upgrade |
| Tier 3 | Unlimited | Entire galaxy including far wild space | End-game drive |

### Route Calculation
- Player inputs a destination address (XX-XXXX format)
- Ship calculates the **shortest jump path** through intermediate systems
- Ship reports the **number of jumps required**
- Multi-hop routes may pass through unknown territory
- Region distance determines if the destination is reachable with current jump drive tier

### Travel Risk
- **Unknown territory**: Slim chance of encounters when passing through
- **Known routes**: Much lower chance of negative events
- **New regions**: Always have a chance to trigger side missions or story events

---

## Procedural Generation

### System Naming
- All system names are **procedurally generated** (e.g., "Kepler-7", "Voss Prime", "NX-4821")
- Scalable and consistent with the modular approach
- Name generation algorithm TBD

### System Generation
- [ ] Algorithm for determining if an address (XX-XXXX) is populated
- [ ] Spiral layout mapping — how region numbers map to galaxy positions
- [ ] Cluster generation — how hubs and sub-systems are grouped within a region
- [ ] System type distribution per region (weighted by distance from origin)
- [ ] Location count generation per system type
- [ ] Discovery content generation per system
- [ ] Event placement across systems
- [ ] Unaffiliated system placement and reward generation

### Regional Networks
- [ ] Exact number of populated addresses per region (target: 20-50)
- [ ] Hub-to-sub-system relationship generation
- [ ] Wild space region content generation
- [ ] Distance-based danger weight formula
- [ ] Jump drive range calculation relative to spiral position

---

## Notes
_Add design notes, open questions, and decisions here._

### Open Design Questions
- Exact spiral layout algorithm for region positioning
- Distance calculation method between regions on the spiral
- Procedural name generation approach
- Station building: ship type, costs, deployment mechanics
- Station tier upgrade costs and pirate raid mechanics
- How cleared hostile systems integrate back into the hub supply chain
- Unaffiliated system generation — rarity and content distribution
- Dead system lore (future consideration)
