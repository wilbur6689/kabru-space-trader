# Enemies and Pirate Factions

## Overview
All enemies are MasterEntity objects with fame levels that scale with region distance. Four pirate factions control quadrants of the galaxy, each with distinct combat behavior, loot types, and personality.

---

## Enemy Fame System

### Fame Calculation
```
enemy_fame = base_fame + floor(manhattan_distance / 2.5)
```
Where `manhattan_distance` is the Manhattan distance from origin (0,0), calculated as |x| + |y| from the region's grid coordinates. The distance portion is floored before adding base_fame.

**Enemy fame is capped at 90% of the player's maximum fame (max 12), so enemies cannot exceed fame 10.**

### Base Fame by Enemy Type
| Enemy | Base Fame | Difficulty | Behavior |
|-------|-----------|-----------|----------|
| Petty Pirate | 1 | Easy | Flees when damaged |
| Gang Enforcer | 3 | Medium | Hostile system guards |
| Pirate Captain | 4 | Medium | Fights to the end |
| Patrol Ship | 4 | Medium | Scans for contraband (legitimate faction) |
| Bounty Hunter | 5 | Hard | Targets wanted players |
| Pirate Fleet | 6 | Hard | Overwhelming numbers |
| Alien Vessel | 7 | Very Hard | Unpredictable |
| Warlord | 8 | Boss | Faction boss, controls hostile system |

### Example Scaling (Manhattan Distance from Origin, capped at Fame 10)
| Enemy | Dist 2 (safe) | Dist 6 (mid) | Dist 10 (far) | Dist 16 (edge) | Dist 20 (max) |
|-------|--------------|-------------|--------------|---------------|--------------|
| Petty Pirate | Fame 1 | Fame 3 | Fame 5 | Fame 7 | Fame 9 |
| Pirate Captain | Fame 4 | Fame 6 | Fame 8 | Fame 10 | Fame 10 |
| Warlord | Fame 8 | Fame 10 | Fame 10 | Fame 10 | Fame 10 |

---

## Pirate Faction Combat Flavors

### Faction-Specific Behavior
Each pirate faction has distinct combat behavior, reflecting their identity.

| Faction | Combat Style | Negotiation | Flee Behavior | Loot Type |
|---------|-------------|-------------|---------------|-----------|
| **Void Reavers** | Most aggressive — attack first, high damage output | Hard to negotiate with, demand high price | Rarely flee, fight to the end | Weapons, raw credits |
| **Silk Hand** | Try to negotiate or bribe first, fight smart | Easy to negotiate with, accept lower offers | Flee if outmatched | Contraband, trade goods, credits |
| **Rust Collective** | Tanky — high hull/shields, slow. Try to disable and strip parts | Moderate — want your cargo and parts | Won't flee, try to take your ship | Salvage, ship parts, resources |
| **Phantom Circuit** | Evasive — high maneuverability, hard to hit | Will trade intel to avoid combat | Flee readily if threatened | Intel, system addresses, blueprints |

### Pirate Faction Difficulty Ranking
| Quadrant | Faction | Difficulty | Combat Style |
|----------|---------|-----------|--------------|
| Q01 | **Silk Hand** | Easiest | Prefer bribery, flee when outmatched |
| Q02 | **Phantom Circuit** | Easy-Medium | Evasive, trade intel to avoid combat |
| Q03 | **Rust Collective** | Medium-Hard | Tanky, want your cargo, won't flee |
| Q04 | **Void Reavers** | Hardest | Most aggressive, high damage, rarely flee |

> **Note:** Players are free to venture into any quadrant at any time. Missions will naturally lead players to systems across different quadrants, providing organic exposure to all factions.

### Rust Collective — Part Stripping
The Rust Collective doesn't literally board the player's ship, but a successful attack from them may result in the player losing installed ship parts (advanced components stripped during combat). This represents their scavenger identity mechanically without requiring a boarding sub-system.

---

## Border Region Encounters
- Within **2 regions of a quadrant border**, factions blend based on proximity
- **2 regions away**: 75% home faction / 25% neighbor faction
- **1 region / on border**: 50/50 mix from both factions
- The central safe zone (R0101 in each quadrant, Space Force controlled) prevents any faction blending at the galaxy center
- These factions are **fighting over territory** — turf wars

### Turf War Encounters
Two types of turf war encounters exist in blending zones:
- **Passive (common):** Player finds wreckage and debris from recent faction battles — loot from both factions available for salvage
- **Active (rare):** Player stumbles into an ongoing battle between two factions — can choose to intervene (pick a side or fight both) or attempt to slip past and scavenge

---

## Enemy Ship Classes
- Enemies use the same ship class system as the player (MasterEntity)
- Enemy ship class scales with fame level:
  - Fame 1-3: Shuttles, Small ships
  - Fame 4-6: Medium ships
  - Fame 7-10: Large ships
- Enemy equipment tiers scale with their fame level
- Randomly generated enemies use **standard ships from the player catalog** — no unique enemy-only ships
- **Story characters and special encounters** may have unique or specialized ships as exceptions

### Faction Ship Specialties
Each faction's generated ships are weighted toward loadouts that match their combat identity:
| Faction | Ship Emphasis |
|---------|--------------|
| **Void Reavers** | High damage weapons, offensive modules |
| **Silk Hand** | Fast engines, escape systems, cargo holds |
| **Rust Collective** | Heavy hull, strong shields, tractor/disable modules |
| **Phantom Circuit** | High evasion, stealth systems, scanner countermeasures |

### Defeated Enemy Ships
When a pirate **surrenders**, the player can choose:
- **Keep the ship** — add it to their fleet (if fleet mechanic exists) or swap to it
- **Scrap for parts** — strip useful components and equipment
- **Sell it** — take a lump credit payout

---

## Patrol Ships (Legitimate Faction)
- Not pirates — represent law enforcement in friendly space
- Scan the player for **contraband** when encountered
- If contraband found: **fines and confiscation**
- Not hostile unless the player resists or has a bounty
- More common near hubs, rare in wild space
- **Player can fight patrol ships** — doing so triggers a bounty/wanted status
- **Bribing patrol ships is not possible** — only the Silk Hand faction can be bribed

---

## Resolved Design Decisions
- **Loot tables:** Simple approach — each faction has one loot pool, enemy type/fame affects quantity and quality of drops, not loot type
- **Enemy ship loadouts:** Faction-specialized loadouts scaling with fame (see Faction Ship Specialties above)
- **Faction affiliation in hostile systems:** Follows quadrant ownership with standard 2-region border blending rules
- **Patrol ship bribing:** Not possible — only Silk Hand pirates can be bribed
- **Bounty hunter triggers:** Player becomes "wanted" from any of: attacking/destroying patrol ships, resisting arrest during contraband scans, accumulating unpaid fines, attacking friendly/neutral systems, or story-driven triggers
- **Alien Vessels:** Stat-heavy enemies, cannot communicate, will not attack unless provoked — not unique mechanics, just high stats and unpredictable behavior
- **Pirate Fleet:** Sequential wave combat — beat one group, next wave engages
- **Border blending:** Uses standard 2-region gradient rules from quadrant borders; safe zone at center prevents 4-way blending
- **Enemy respawn:** Cleared hostile systems stay cleared — no respawn

## Open Design Questions
- Patrol ship scan mechanics — chance to detect contraband based on what factors
- Specific bounty/wanted level thresholds and consequences
- Alien Vessel provocation rules — what counts as provoking them
- Pirate Fleet wave count — how many waves per encounter, scaling with fame?
