# Events and Hazards

## Overview
This document covers travel encounters, environmental hazards, system events, economic challenges, and pirate turf wars. These are the non-combat dangers and dynamic world events that create risk and opportunity during gameplay.

---

## Travel Encounters

### Route-Based Risk
| Route Type | Encounter Chance | Notes |
|------------|-----------------|-------|
| Known route (previously traveled) | Very Low | Established and safer |
| Unknown territory | Slim chance | Anything could happen |
| New region entry | Moderate | Side missions, story events, or dangers |

### Travel Event Types
| Event | Risk | Reward | Trigger |
|-------|------|--------|---------|
| Pirate Ambush | Ship damage, cargo theft | Loot if victorious | Unknown territory |
| Distress Signal | Trap or time cost | Reputation, rewards, address discovery | Any route |
| Asteroid Field | Hull damage | Mineral salvage | Unknown territory |
| Space Storm | Navigation disruption | None (survival) | Unknown territory |
| Derelict Ship | Trap risk | Salvage, rare goods, tow opportunity | Unknown territory |
| Patrol Checkpoint | Contraband scan | None (legal) / Fines (contraband) | Known friendly routes |
| Merchant Convoy | None | Trading opportunity | Known routes |
| Story Event | Varies | Main story or side quest trigger | New region entry |
| Pirate Turf War | Combat crossfire | Loot from both sides | Border regions between pirate factions |

---

## Environmental Hazards

### Space Hazards (During Travel)
| Hazard | Effect | Mitigation |
|--------|--------|-----------|
| Asteroid fields | Hull damage risk | Piloting skill reduces damage |
| Nebulae | Reduced scanner effectiveness | Higher scanner tier overcomes |
| Gravity wells | Navigation complications | Engine tier helps navigate |
| Radiation zones | Component damage | Hull plating reduces exposure |

### System Hazards (During Search/Discovery)
| Hazard | Found In | Effect | Mitigation |
|--------|----------|--------|-----------|
| Unstable structures | Dead systems | Structural collapse damage | Engineering skill |
| Contaminated zones | Dead worlds | Hull/component damage | Scanner detects beforehand |
| Hostile environment | Primitive planets | Combat with wildlife | Combat skill |
| Trap mechanisms | Ancient ruins | Various damage types | Exploration skill |

---

## System Events

### Pre-Programmed Events
- Story arc triggers at specific systems or conditions
- Scripted encounters for tutorial and key story moments
- Faction war events that affect entire clusters

### Random Events
- Procedurally assigned to systems by the game engine
- Can include: merchant convoys, pirate raids, resource discoveries, distress calls
- Events can change system state (friendly -> hostile or vice versa)
- New events can generate when the player enters a new region

---

## Pirate Turf War Events
- Occur in **border regions** between pirate faction territories
- Mixed enemy types from both factions
- Higher danger but opportunity to profit from the chaos
- May affect regional pirate faction influence
- Player can choose sides or stay neutral

---

## Economic Challenges

### Market Risks (Stock Market Model)
- Price crashes — bought high, market drops before you can sell
- Price spikes — missed opportunity or scramble to buy before it rises more
- Shortages — desired goods unavailable at current location
- Regional economic shifts based on system hostility changes
- Clearing a hostile system may open new trade routes and shift regional prices

### Financial Pressure
- Ship repair costs after combat/hazards
- Ship upgrade costs for progression (especially jump drive tiers)
- New ship purchases at hub shipyards
- Contraband fines if caught by patrols
- Hostile system taxes for doing business
- Competition from NPC traders affecting supply/demand

---

## Difficulty Scaling

### Regional Difficulty
- Systems **farther from starting region** have stronger enemies and better loot
- Enemy fame scales: base fame + (region_distance / 5)
- Hostile systems in distant regions have more outposts to clear before boss
- Jump drive upgrades naturally lead the player to harder content

### Player Power Growth
- Ship upgrades (jump drive, weapons, shields, scanner)
- Pilot skill advancement (usage-based, 0-10 per skill)
- Fame level growth (intimidation, mission slots, pricing, NPC access)
- Better trade routes and wealth accumulation
- Reputation unlocking better faction missions and equipment
- Knowledge of the galaxy (known addresses, price trends, faction territories)

### Natural Progression
- Starting region (00) — learn the basics, easy enemies, safe trading
- Nearby regions — moderate challenges, first hostile system clearing
- Mid-range regions — multiple pirate factions present, tougher enemies
- Distant regions — high-fame enemies, complex faction politics
- Far edges — border region turf wars, strongest enemies, best loot

---

## Open Design Questions
- Exact encounter probability tables for travel
- Environmental hazard damage values
- How skill mitigation reduces hazard effects (percentage? flat reduction?)
- Travel encounter generation — how many per multi-hop route
- How turf war events affect faction influence over time
- Whether economic events are purely procedural or follow patterns
- Distress signal trap vs. genuine ratio
- How system state changes (friendly to hostile) are triggered
- Event cooldown periods — how often can the same event type fire
