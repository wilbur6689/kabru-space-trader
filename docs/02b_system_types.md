# System Types

## Overview
Every system in the galaxy is defined by two independent axes: **population tier** (how much is there) and **alignment** (who controls it and how dangerous it is). Any combination is possible but weighted by distance from origin. All systems contain **discoveries** — interactive locations the player can find through searching.

---

## System Hierarchy

```
Quadrant → many Regions → many Systems → many Discoveries
```

- A **system** is a single addressable location (e.g., Q02-R0307-4821)
- Systems contain **discoveries** — things the player can interact with
- What a system looks like visually (space station, planet, nebula, moon, etc.) is layered on top as theme flavor

---

## Axis 1: Population Tier

Population tier determines how many discoveries a system has. **Every system has a minimum of 3 service discoveries** (e.g., merchant, repair, refueling) regardless of tier.

| Tier | Total Discoveries | Min Services | Remaining (loot/encounters) | Description |
|------|------------------|-------------|---------------------------|-------------|
| **Major** | 12-18 | 3+ (typically 6-10) | 6-12 | Large, bustling systems with many services and things to do |
| **Moderate** | 7-11 | 3+ (typically 4-6) | 3-7 | Mid-sized systems with a solid range of services |
| **Minor** | 3-6 | 3 | 0-3 | Small systems with basic services and limited extras |
| **Measly** | 3 | 3 | 0 | Bare minimum — a merchant, repair, and refueling. Nothing beyond the basics. |

### Population Distribution by Danger Zone

| Tier | Safe (1-3) | Mid (4-6) | Far (7-9) | Edge (10+) |
|------|-----------|-----------|-----------|-----------|
| Major | 25% | 15% | 5% | 2% |
| Moderate | 40% | 30% | 20% | 10% |
| Minor | 25% | 35% | 40% | 38% |
| Measly | 10% | 20% | 35% | 50% |

Inner regions are mostly civilized and populated. Outer regions are mostly sparse. A Major system can still appear at the edge — it's just rare.

---

## Axis 2: Alignment

Alignment determines who controls the system, how dangerous it is, and what types of missions are generated there.

| Alignment | Description | Controlled By | Mission Types |
|-----------|-------------|--------------|---------------|
| **Friendly** | Welcoming, civilized, safe | Space Force or legitimate faction | Delivery, trade, bounty, rescue |
| **Neutral** | No allegiance, not dangerous but not helpful | Independent, self-governing | Exploration, delivery, neutral trade |
| **Hostile** | Dangerous environment, not faction-controlled | Natural threats — wildlife, contamination, unstable terrain | Exploration, rescue, investigation |
| **Pirate-Controlled** | Occupied by a pirate faction | One of the 4 pirate factions | Smuggling, bounty, clearance, pirate faction missions |
| **Dead** | Lifeless, abandoned, no active anything | No one — ruins and void | Exploration, salvage, investigation |

### Alignment Distribution by Danger Zone

| Alignment | Safe (1-3) | Mid (4-6) | Far (7-9) | Edge (10+) |
|-----------|-----------|-----------|-----------|-----------|
| Friendly | 50% | 25% | 10% | 3% |
| Neutral | 25% | 25% | 15% | 10% |
| Hostile | 10% | 15% | 20% | 22% |
| Pirate-Controlled | 5% | 25% | 40% | 50% |
| Dead | 10% | 10% | 15% | 15% |

---

## Combined Examples

All 20 tier + alignment combinations are valid but **weighted** — some are common, some are rare.

| Combination | Likelihood | Example |
|-------------|-----------|---------|
| Major + Friendly | Common (inner regions) | Large civilized station with 15+ discoveries, full services |
| Moderate + Neutral | Common (mid regions) | Independent trading post with 8 discoveries |
| Minor + Pirate-Controlled | Common (outer regions) | Small pirate outpost with 4 discoveries |
| Measly + Friendly | Common (everywhere) | Tiny refueling stop with 3 basic services |
| Major + Dead | Rare | Massive ruined city — lots of salvage, ruins, caches |
| Major + Pirate-Controlled | Uncommon | Large pirate stronghold with many outposts and black markets |
| Measly + Dead | Common (edges) | A single salvage site in empty space |
| Minor + Hostile | Common (far regions) | Small system with wildlife or contamination hazards |

---

## Discovery Types

Any system can have any discovery type. More populated systems simply have more. Discoveries are what the player interacts with when they arrive at or search a system.

### Services (interactive, repeatable)
- Merchant / trading post (buy/sell goods)
- Weapon upgrade shop
- Engine/computer upgrade shop
- Armor/hull upgrade shop
- Jump drive upgrade shop
- Shipyard (buy/sell ships)
- Repair dock
- Refueling station
- Mission board / bulletin board
- Black market contact
- Information broker (sell system addresses, intel)
- Star chart vendor
- NPC quest giver

### Loot (one-time or depleting)
- Hidden cache (shards/goods)
- Stolen cargo
- Salvage wreckage
- Resource deposit (raw materials)
- Ancient ruins / artifacts
- Derelict ship (tow opportunity)
- Intel / data cache (reveals addresses)
- Ghost town (abandoned supplies)

### Encounters (events that trigger on discovery)
- Pirate hideout (combat)
- Enemy outpost (combat, required for system clearing)
- Ambush / trap
- Distress signal (rescue or trap)
- Hostile wildlife
- Contaminated zone (hull damage)
- Unstable ruins (structural collapse)
- Imprisoned NPC (rescue)
- Space anomaly
- Pirate turf war (crossfire)

### Passive/Environmental
- Patrol checkpoint (contraband scan)
- Merchant convoy (trading opportunity)
- Smuggler's route
- Player station slot (empty address for building)
- Story event trigger

---

## Regional Economy

### Trade Good Specialties
- Each region is generated with a **randomly weighted specialty list** covering all trade goods (food, ore, weapons, electronics, etc.)
- This creates a **unique economic fingerprint** per region — no two regions have the same supply profile
- The **top 25% of specialties** by weight become the region's dominant industries
- Dominant specialties determine:
  - What goods are **abundant and cheap** in that region
  - What Major systems focus their services around
  - What sub-systems support (supply chain feeding the dominant industries)
- Players discover trade routes by finding which regions have high supply (cheap) vs. low supply (expensive) for specific goods

### Respawn Points
- The player's **last visited Friendly Major or Moderate system** serves as their respawn point
- If the player dies, they respawn there with a loaner Shuttle
- Regions with no Friendly Major/Moderate systems have **no respawn point** — this is what makes them dangerous

---

## Wild Space

- Any region with **0 Friendly Major systems** is labeled **"Wild Space"**
- Nav computer warns: *"Entering wild space. No major friendly systems detected. No respawn available."*
- Typically found at Manhattan distance 10+ but determined dynamically by region content
- Highest risk, highest reward exploration territory

---

## Dead Space Border

- The outermost 2 cells of each quadrant's 12x12 grid are **dead space**
- Accessible but empty — only Dead + Measly systems and void
- Nav computer warns: *"Entering uncharted dead space. No viable systems detected."*
- Serves as a natural galaxy boundary without a hard wall

---

## Open Design Questions
- How alignment affects discovery weighting (e.g., Pirate-Controlled systems have more black markets, Hostile systems have more environmental hazards)
- Exact discovery type weights per alignment
- How regional trade good specialties are generated (number of goods, weight ranges)
- Visual theme assignment — how a system gets labeled as space station vs. planet vs. nebula
- Whether alignment can shift over time without player action (e.g., Neutral → Pirate-Controlled)
- How the minimum 3 services are selected per system (always merchant/repair/refuel, or varied?)
- How Pirate-Controlled systems differ from the old "hostile system clearing" mechanic
