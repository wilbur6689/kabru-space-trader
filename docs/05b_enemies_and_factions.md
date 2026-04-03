# Enemies and Pirate Factions

## Overview
All enemies are MasterEntity objects with fame levels that scale with region distance. Four pirate factions control quadrants of the galaxy, each with distinct combat behavior, loot types, and personality.

---

## Enemy Fame System

### Fame Calculation
```
enemy_fame = base_fame + (manhattan_distance / 2.5)
```
Where `manhattan_distance` is the Manhattan distance from origin (0,0), calculated as |x| + |y| from the region's grid coordinates. Result is floored to integer.

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

### Example Scaling (Manhattan Distance from Origin)
| Enemy | Dist 2 (safe) | Dist 6 (mid) | Dist 10 (far) | Dist 16 (edge) | Dist 20 (max) |
|-------|--------------|-------------|--------------|---------------|--------------|
| Petty Pirate | Fame 1 | Fame 3 | Fame 5 | Fame 7 | Fame 9 |
| Pirate Captain | Fame 4 | Fame 6 | Fame 8 | Fame 10 | Fame 12 |
| Warlord | Fame 8 | Fame 10 | Fame 12 | Fame 14 | Fame 16 |

---

## Pirate Faction Combat Flavors

### Faction-Specific Behavior
Each pirate faction has distinct combat behavior, reflecting their identity.

| Faction | Combat Style | Negotiation | Flee Behavior | Loot Type |
|---------|-------------|-------------|---------------|-----------|
| **Void Reavers** | Most aggressive — attack first, high damage output | Hard to negotiate with, demand high price | Rarely flee, fight to the end | Weapons, raw credits |
| **Silk Hand** | Try to negotiate or bribe first, fight smart | Easy to negotiate with, accept lower offers | Flee if outmatched | Contraband, trade goods, credits |
| **Rust Collective** | Tanky — high hull/shields, slow. Try to disable and board | Moderate — want your cargo and parts | Won't flee, try to take your ship | Salvage, ship parts, resources |
| **Phantom Circuit** | Evasive — high maneuverability, hard to hit | Will trade intel to avoid combat | Flee readily if threatened | Intel, system addresses, blueprints |

### Pirate Faction Difficulty Ranking
| Quadrant | Faction | Difficulty | Combat Style |
|----------|---------|-----------|--------------|
| Q01 | **Silk Hand** | Easiest | Prefer bribery, flee when outmatched |
| Q02 | **Phantom Circuit** | Easy-Medium | Evasive, trade intel to avoid combat |
| Q03 | **Rust Collective** | Medium-Hard | Tanky, want your cargo, won't flee |
| Q04 | **Void Reavers** | Hardest | Most aggressive, high damage, rarely flee |

---

## Border Region Encounters
- Within **2 regions of a quadrant border**, factions blend based on proximity
- **2 regions away**: 75% home faction / 25% neighbor faction
- **1 region / on border**: 50/50 mix from both factions
- These factions are **fighting over territory** — turf wars
- Player may encounter mixed groups or stumble into ongoing battles
- Can loot from both sides if caught in a turf war event

---

## Enemy Ship Classes
- Enemies use the same ship class system as the player (MasterEntity)
- Enemy ship class scales with fame level:
  - Low fame: Shuttles, Small ships
  - Medium fame: Medium ships
  - High fame: Large ships, specialized variants
- Enemy equipment tiers scale with their fame level

---

## Patrol Ships (Legitimate Faction)
- Not pirates — represent law enforcement in friendly space
- Scan the player for **contraband** when encountered
- If contraband found: **fines and confiscation**
- Not hostile unless the player resists or has a bounty
- More common near hubs, rare in wild space

---

## Open Design Questions
- Specific loot tables per enemy type and faction
- Enemy ship loadout generation based on fame level
- How enemy faction affiliation is determined per hostile system
- Patrol ship scan mechanics — chance to detect contraband, bribe option
- Bounty hunter trigger conditions (what makes a player "wanted")
- Alien Vessel encounter design — unique mechanics or just high stats
- Pirate Fleet encounter — is it one fight or multiple sequential fights
- How faction territory borders interact at the 2-region blending zones
- Enemy respawn mechanics in hostile systems (do they repopulate over time?)
