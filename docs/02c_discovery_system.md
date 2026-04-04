# Discovery System

## Overview
Discovery is a core gameplay mechanic where players search systems to reveal hidden locations. Each search costs one turn and can reveal beneficial locations or dangers. All discoveries are permanently stored and never need to be re-found. Discovery count is determined by **population tier**, and what's known on arrival depends on **alignment**.

---

## Discoverable Location Counts
Every system has a minimum of 3 service discoveries regardless of tier.

| Population Tier | Total Discoveries | Min Services | Remaining (loot/encounters) |
|----------------|------------------|-------------|---------------------------|
| Major | 12-18 | 3+ (typically 6-10) | 6-12 |
| Moderate | 7-11 | 3+ (typically 4-6) | 3-7 |
| Minor | 3-6 | 3 | 0-3 |
| Measly | 3 | 3 | 0 |

---

## Known vs. Hidden on Arrival

What the player sees when they first arrive depends on alignment:

| Alignment | On Arrival | Searching Required For |
|-----------|-----------|----------------------|
| **Friendly** | Minimum 3 services revealed immediately | Remaining services, loot, encounters |
| **Neutral** | Minimum 3 services revealed immediately | Remaining services, loot, encounters |
| **Hostile** | Nothing known — everything hidden | All discoveries |
| **Pirate-Controlled** | Nothing known — everything hidden | All discoveries |
| **Dead** | Nothing known — everything hidden | All discoveries |

---

## Progressive Exploration
- Each system has a **variable number** of hidden discoverable locations
- Each **search action costs one turn** and reveals the next hidden location
- **Discovery order is fully random** — any search can reveal safe or hostile results at any time
- System overview screen displays: **"X / Y locations discovered"**

---

## Persistent Discovery Database
- All discoveries are **permanently stored** in the database
- Revisiting a system shows all previously discovered locations accessible
- Players never need to re-discover known locations
- **Depleted one-time discoveries** (looted caches, emptied salvage) are **removed from the system overview** — only actionable discoveries are shown

---

## Discovery Risk

### Risk by Alignment
| Alignment | Danger Chance | Notes |
|-----------|---------------|-------|
| Friendly | Low | Mostly safe, occasional surprises |
| Neutral | Low-Medium | No active threats, minor hazards |
| Hostile | Medium-High | Environmental dangers, wildlife |
| Pirate-Controlled | High | Combat encounters likely |
| Dead | Medium | No active threats but environmental hazards |

---

## Dangerous Discoveries

### Alignment-Locked Dangers
Some dangerous discoveries only appear in specific alignments:

| Discovery | Alignment | Risk | Outcome |
|-----------|-----------|------|---------|
| Enemy Outpost | Pirate-Controlled only | Combat (required for clearing) | Intel, prisoners to rescue |
| Pirate Hideout | Pirate-Controlled only | Combat encounter | Loot if victorious, damage if not |
| Hostile Wildlife | Hostile only | Combat | Rare biological resources |
| Contaminated Zone | Hostile / Dead only | Hull/component damage | Rare materials if scanned |
| Unstable Ruins | Dead only | Structural collapse | Artifacts if explored carefully |

### Universal Dangers (Any Alignment)
These can appear anywhere, but are rare in Friendly/Neutral systems:

| Discovery | Risk | Outcome |
|-----------|------|---------|
| Ambush / Trap | Surprise attack or trap damage | Must fight, flee, or disarm |
| Booby-Trapped Cache | Trap damage | Valuable salvage if survived |
| Distress Signal | Trap or genuine rescue | Reputation/rewards if genuine, combat if trap |

---

## Threat Blocking

### How Threats Block Discoveries
When a hostile discovery is found in any system, it **blocks access to a few related discoveries** — not the entire system.

**Example:** A hostile native tribe is discovered on a planet. They block access to:
- The ore deposits near their territory
- The ancient ruins in their sacred grounds

But the merchant trading post on the other continent and the repair dock in orbit remain accessible.

### Resolving Blocked Discoveries
- **Fight/resolve the threat**: Defeat the danger to unlock blocked discoveries
- **Skip past it**: Search again (costs an extra turn) to find a different discovery instead
- **Come back later**: The threat persists until dealt with — blocked discoveries remain locked on revisits

### Threats in Friendly/Neutral Systems
- Rare but possible — adds stakes even in safe territory
- Must be resolved before accessing the blocked discoveries
- Does not change the system's alignment

---

## Scanner Pre-Search Intel

### What the Scanner Shows
Before committing to a search, the scanner provides a preview of the next discovery:
- **Category**: Service / Loot / Encounter / Danger
- **Risk level**: Safe / Low risk / Moderate danger / High danger
- **Example display**: *"High-value loot detected (moderate danger)"*

### Scanner Tier Influence
- Scanner tier affects **discovery quality** — better scanners improve chance of finding valuable locations vs. dangers
- Higher tier scanners provide **more detailed pre-search previews**
- Scanner quality influences the overall ratio of beneficial vs. dangerous discoveries found

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

## Discovery Types Reference

### Services (interactive, repeatable)
- Merchant / trading post
- Weapon upgrade shop
- Engine/computer upgrade shop
- Armor/hull upgrade shop
- Jump drive upgrade shop
- Shipyard (buy/sell ships)
- Repair dock
- Refueling station
- Mission board / bulletin board
- Black market contact
- Information broker
- Star chart vendor
- NPC quest giver

### Loot (one-time or depleting — removed from UI when exhausted)
- Hidden cache (shards/goods)
- Stolen cargo
- Salvage wreckage
- Resource deposit (raw materials)
- Ancient ruins / artifacts
- Derelict ship (tow opportunity)
- Intel / data cache (reveals addresses)
- Ghost town (abandoned supplies)

### Encounters (events that trigger on discovery)
- Pirate hideout (combat) — Pirate-Controlled only
- Enemy outpost (combat, required for clearing) — Pirate-Controlled only
- Ambush / trap — any alignment
- Distress signal (rescue or trap) — any alignment
- Hostile wildlife — Hostile only
- Contaminated zone (hull damage) — Hostile/Dead only
- Unstable ruins (structural collapse) — Dead only
- Imprisoned NPC (rescue) — Pirate-Controlled only
- Space anomaly — any alignment
- Pirate turf war (crossfire) — Pirate-Controlled / border regions

### Passive/Environmental
- Patrol checkpoint (contraband scan)
- Merchant convoy (trading opportunity)
- Smuggler's route
- Player station slot (empty address for building)
- Story event trigger

---

## Procedural Generation (TBD)
- [ ] Discovery count generation per population tier
- [ ] Discovery content generation weighted by alignment
- [ ] Threat blocking — which discoveries block which others (relationship mapping)
- [ ] Scanner preview generation logic
- [ ] Event placement across systems

---

## Open Design Questions
- Exact scanner tier influence on discovery quality percentages
- How scanner preview detail scales with tier (what info at each tier?)
- Threat blocking relationship rules — how to determine which discoveries a threat blocks
- How many discoveries a single threat can block (1-3? percentage-based?)
- Whether the player can see how many discoveries are blocked by a threat
- How derelict ship discoveries connect to the ship towing mechanic
- Whether discovery order weighting exists beyond pure random (e.g., services slightly more likely early)
