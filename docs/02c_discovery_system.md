# Discovery System

## Overview
Discovery is a core gameplay mechanic where players search systems to reveal hidden locations. Each search costs one turn and can reveal beneficial locations or dangers. All discoveries are permanently stored and never need to be re-found.

---

## Discoverable Location Counts
| System Type | Hidden Locations | Notes |
|-------------|-----------------|-------|
| Hub System | 1-3 | Core services already known; hidden locations are bonus extras |
| Friendly Sub-System | 2-4 | Basic services known; hidden locations are additional finds |
| Dead System | 3-6 | Nothing known; everything must be searched |
| Hostile System | 2-5 | Nothing known until explored or cleared |

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

### Discovery Risk by System Type
| System Type | Danger Chance | Notes |
|-------------|---------------|-------|
| Friendly | Low | Mostly safe, occasional surprises |
| Dead | Medium | No active threats but environmental hazards |
| Hostile | High | Controlled by enemies, combat likely |

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

## Discovery Content by System Type

### Hub Hidden Discoveries
- Secret contacts (unlock special missions or black market access)
- Black market entrances (hidden economy)
- Side quest triggers (NPC interactions)

### Friendly Sub-System Discoveries
- Additional merchants / specialized traders
- Resource deposits
- Black markets
- Quest givers / NPCs
- Hidden caches

### Dead System Discoveries
- Salvage wreckage
- Ancient ruins / artifacts
- Resource deposits (uncontested but dangerous)
- Derelict ships
- Hidden caches from previous inhabitants
- Ghost towns with abandoned supplies

### Hostile System Discoveries
- Enemy outposts (combat risk — required for system clearing)
- Imprisoned merchants (rescue for loyalty/discounts)
- Stolen cargo (high value loot)
- Black markets (better prices but more dangerous)
- Intel / data caches (reveal info about other systems)

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
- How discovery quality scales with region distance
- Whether players can get hints about remaining undiscovered locations
- How derelict ship discoveries connect to the ship towing mechanic
