# Discovery System

## Overview
Discovery is a core gameplay mechanic where players search systems to reveal hidden locations. Each search costs one turn and can reveal beneficial locations or dangers. All discoveries are permanently stored and never need to be re-found.

---

## Discoverable Location Counts
Discovery count is determined by **population tier**, not system type. Every system has a minimum of 3 service discoveries.

| Population Tier | Total Discoveries | Min Services | Remaining (loot/encounters) |
|----------------|------------------|-------------|---------------------------|
| Major | 12-18 | 3+ (typically 6-10) | 6-12 |
| Moderate | 7-11 | 3+ (typically 4-6) | 3-7 |
| Minor | 3-6 | 3 | 0-3 |
| Measly | 3 | 3 | 0 |

---

## Progressive Exploration
- Each system has a **variable number** of hidden discoverable locations
- Each **search action costs one turn** and reveals the next hidden location
- The game engine procedurally generates encounters within each system group
- System overview screen displays: **"X / Y locations discovered"**

---

## Persistent Discovery Database
- All discoveries are **permanently stored** in the database
- Revisiting a system shows all previously discovered locations accessible
- Players never need to re-discover known locations

---

## Discovery Risk
- Searching can reveal **dangers** — pirate hideouts, enemy bases, ambushes
- Discovery is a core risk/reward mechanic
- Better equipment (scanners) may influence discovery outcomes

### Discovery Risk by Alignment
| Alignment | Danger Chance | Notes |
|-----------|---------------|-------|
| Friendly | Low | Mostly safe, occasional surprises |
| Neutral | Low-Medium | No active threats, minor hazards |
| Hostile | Medium-High | Environmental dangers, wildlife |
| Pirate-Controlled | High | Combat encounters likely |
| Dead | Medium | No active threats but environmental hazards |

---

## Dangerous Discoveries
When a player searches a system, they may discover threats instead of beneficial locations.

| Discovery | System Type | Risk | Outcome |
|-----------|-------------|------|---------|
| Pirate Hideout | Hostile | Combat encounter | Loot if victorious, damage if not |
| Enemy Outpost | Hostile | Combat or stealth | Intel, prisoners to rescue |
| Ambush | Hostile/Dead | Surprise attack | Must fight or flee |
| Booby-Trapped Cache | Dead | Trap damage | Valuable salvage if survived |
| Unstable Ruins | Dead | Structural collapse | Artifacts if explored carefully |
| Hostile Wildlife | Friendly (primitive) | Combat | Rare biological resources |
| Contaminated Zone | Dead | Hull/component damage | Rare materials if scanned |

---

## Scanner Influence
- Scanner tier affects **discovery quality** — better scanners improve chance of finding valuable locations vs. dangers
- Scanner provides **pre-search intel** about what type of discovery is likely
- Higher tier scanners may reveal discovery count before searching

---

## Discovery Content by Alignment

Any system can have any discovery type. Alignment **weights** which types are more likely.

### Friendly Systems
- Merchants, upgrade shops, shipyards, mission boards, NPC quest givers
- Resource deposits, hidden caches
- Occasional black market contacts (rare)

### Neutral Systems
- Basic merchants, repair, refueling
- Resource deposits, hidden caches
- Independent traders, smuggler contacts

### Hostile Systems
- Environmental hazards (wildlife, contamination, unstable structures)
- Resource deposits (uncontested but dangerous)
- Ancient ruins, salvage
- Rescue targets (stranded NPCs)

### Pirate-Controlled Systems
- Enemy outposts (combat — required for system clearing)
- Imprisoned merchants (rescue for loyalty/discounts)
- Stolen cargo, black markets
- Intel / data caches (reveal info about other systems)

### Dead Systems
- Salvage wreckage, ghost towns
- Ancient ruins / artifacts
- Derelict ships (tow opportunity)
- Hidden caches from previous inhabitants
- Environmental hazards (contamination, structural collapse)

---

## Procedural Generation (TBD)
- [ ] Location count generation per system type
- [ ] Discovery content generation per system
- [ ] Weighted random generation tables per system type
- [ ] Event placement across systems
- [ ] Unaffiliated system placement and reward generation

---

## Open Design Questions
- Exact scanner tier influence on discovery outcomes
- Discovery content weighting tables per system type and danger level
- How discovery quality scales with Manhattan distance from origin
- Whether players can get hints about remaining undiscovered locations
- How derelict ship discoveries connect to the ship towing mechanic
