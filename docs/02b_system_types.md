# System Types

## Overview
This document defines every type of system players can visit, their services, properties, and how they change over time. Systems fall into categories: Hub Systems, Sub-Systems, Unaffiliated Systems, Dead Systems, Hostile Systems, and Wild Space.

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

## Friendly Systems

### Overview
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

---

## Dead Systems

### Overview
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

### Dead System Sub-Types
| Sub-Type | Description | Key Feature |
|----------|-------------|-------------|
| No Planets | Empty void, just a star | Minimal discoveries, quick pass-through |
| Dead World | Destroyed inhabitants, ruins | Salvage, ghost towns, artifacts |
| Asteroid Field | Broken planet chunks | Mining opportunities, navigation hazards |
| Destroyed World Chunks | Remnants of a catastrophe | High-value salvage, dangerous terrain |

---

## Hostile Systems

### Overview
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

## System State and Dynamics

### Hostility Changes
- Previously visited **friendly systems** have a small chance of becoming **hostile** depending on neighboring system loyalties
- **Hostile systems** can become **friendly** if the player eliminates the controlling faction
- Cleared hostile systems revert to their **pre-occupation state** with basic services
- Neighboring system allegiances influence each other over time
- Clearing a hostile system may stabilize neighboring friendly systems

### System Events
- Any system can be marked with a **random or preprogrammed event**
- Events include: mission triggers, enemy attacks, special discoveries, story arc moments
- Events can fire when the player first arrives or on subsequent visits
- New regional entries always have a chance to trigger side missions or main story events

---

## Open Design Questions
- How cleared hostile systems integrate back into the hub supply chain
- Unaffiliated system generation — rarity and content distribution
- Dead system lore (future consideration)
- System naming — procedural generation approach
- How sub-system types are matched to hub specializations algorithmically
- What specific services unlock when a hostile system is cleared
