# Challenges

## Overview
This document defines the obstacles, threats, and challenges players will face. Danger is a core part of the game — **searching for discoveries can reveal threats**, and **traveling through unknown space carries risk**.

---

## Discovery Dangers

### Dangerous Discoveries
When a player searches a system, they may discover threats instead of beneficial locations. This is a core risk/reward mechanic.

| Discovery | System Type | Risk | Outcome |
|-----------|-------------|------|---------|
| Pirate Hideout | Hostile | Combat encounter | Loot if victorious, damage if not |
| Enemy Outpost | Hostile | Combat or stealth | Intel, prisoners to rescue |
| Ambush | Hostile/Dead | Surprise attack | Must fight or flee |
| Booby-Trapped Cache | Dead | Trap damage | Valuable salvage if survived |
| Unstable Ruins | Dead | Structural collapse | Artifacts if explored carefully |
| Hostile Wildlife | Friendly (primitive) | Combat | Rare biological resources |
| Contaminated Zone | Dead | Status effect | Rare materials if scanned |

### Discovery Risk by System Type
| System Type | Danger Chance | Notes |
|-------------|---------------|-------|
| Friendly | Low | Mostly safe, occasional surprises |
| Dead | Medium | No active threats but environmental hazards |
| Hostile | High | Controlled by enemies, combat likely |

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
| Derelict Ship | Trap risk | Salvage, rare goods | Unknown territory |
| Patrol Checkpoint | Contraband scan | None (legal) / Fines (contraband) | Known friendly routes |
| Merchant Convoy | None | Trading opportunity | Known routes |
| Story Event | Varies | Main story or side quest trigger | New region entry |

---

## Combat System

### Combat Overview
- Combat is **turn-based**, consistent with the overall game
- Triggered by dangerous discoveries, travel encounters, or hostile system events
- Player can choose to fight, flee, or negotiate (if applicable)

### Combat Flow
```
Encounter Triggered
  -> Present enemy info (type, strength, ship class)
  -> Player chooses action:
     a) Attack — deal damage based on weapons + combat skill
     b) Defend — reduce incoming damage, prepare counter
     c) Flee — attempt escape based on maneuverability + piloting skill
     d) Negotiate — attempt to talk down (negotiation skill, costs credits)
  -> Resolve player action
  -> Enemy responds (attack, pursue, retreat)
  -> Update ship status (hull, shields)
  -> Check resolution:
     - Victory: enemy defeated → loot
     - Defeat: ship destroyed → death/respawn
     - Escape: player flees → no loot, possible cargo loss
     - Negotiated: pay off or talk down → credits lost, no damage
  -> Loop or end
```

### Enemy Types
| Enemy | Difficulty | Loot | Behavior | Found In |
|-------|-----------|------|----------|----------|
| Petty Pirate | Easy | Small credits | Flees when damaged | Hostile, unknown travel |
| Pirate Captain | Medium | Credits, cargo | Fights to the end | Hostile systems |
| Pirate Fleet | Hard | Big loot | Overwhelming force | Deep hostile territory |
| Patrol Ship | Medium | None | Scans for contraband | Friendly routes |
| Bounty Hunter | Hard | Equipment | Targets wanted players | Anywhere |
| Gang Enforcer | Medium | Intel, credits | Hostile system guards | Hostile systems |
| Warlord | Very Hard | Rare gear, system control | Faction boss | Hostile system (boss) |
| Alien Vessel | Variable | Unique items | Unpredictable | Dead systems, rare |

### Clearing Hostile Systems
- Hostile systems are controlled by a faction (gang, army, warlord)
- Player can systematically discover and eliminate enemy outposts
- Defeating the **controlling boss/faction leader** converts the system from hostile to friendly
- This is a major progression mechanic — the galaxy gets safer as the player acts

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
- Ship upgrade costs for progression
- New ship purchases at hub shipyards
- Contraband fines if caught by patrols
- Competition from NPC traders affecting supply/demand

---

## Environmental Hazards

### Space Hazards (During Travel)
- Asteroid fields — hull damage risk
- Nebulae — reduced scanner effectiveness
- Gravity wells — navigation complications
- Radiation zones — potential status effects

### System Hazards (During Search/Discovery)
- Unstable structures in dead systems
- Contaminated zones on dead worlds
- Hostile environment on primitive planets
- Trap mechanisms in ancient ruins

---

## Mission Challenges

### Mission Types
| Type | Description | Risk | Reward | Source |
|------|-------------|------|--------|--------|
| Delivery | Transport cargo to a destination address | Travel encounters | Credits | Hub bulletin board |
| Smuggling | Move contraband past patrols | Patrol scans, fines | High credits | Black market contacts |
| Bounty | Hunt a target in a hostile system | Combat | Credits, reputation | Hub mission board |
| Rescue | Free imprisoned merchant/NPC | Combat, hostile system | Reputation, loyalty discounts | Discovery or NPC tip |
| Exploration | Chart unknown system addresses | Unknown dangers | Discovery XP, reputation | Information broker |
| Clearance | Eliminate hostile faction from a system | Heavy combat | System conversion, major reputation | Story or hub mission |
| Investigation | Gather intel from data caches | Hostile territory | Lore, system addresses, story progression | Story events |

### Mission Complications
- Multi-hop travel through unknown territory
- Rival traders/pilots competing for the same mission
- Betrayals and plot twists
- Moral dilemmas — help one faction at the cost of another
- Time-sensitive information (addresses or intel that may become stale)

---

## System Events

### Pre-Programmed Events
- Story arc triggers at specific systems or conditions
- Scripted encounters for tutorial and key story moments
- Faction war events that affect entire clusters

### Random Events
- Procedurally assigned to systems by the game engine
- Can include: merchant convoys, pirate raids, resource discoveries, distress calls
- Events can change system state (friendly → hostile or vice versa)
- New events can generate when the player enters a new region

---

## Difficulty Scaling

### How Challenges Scale
- Systems **farther from starting region** have stronger enemies and better loot
- Hostile systems in distant clusters have higher-tier faction bosses
- Jump drive upgrades naturally lead the player to harder content
- Clearing hostile systems in one region prepares the player for the next

### Player Power Growth
- Ship upgrades (jump drive, weapons, shields)
- Pilot skill advancement
- Better trade routes and wealth accumulation
- Reputation unlocking better missions and equipment
- Knowledge of the galaxy (known addresses, price trends)

---

## Notes
_Add design notes, open questions, and decisions here._

### Open Design Questions
- Exact encounter probability tables
- Combat damage formulas and balance
- Specific loot tables per enemy type
- How many outposts must be cleared to convert a hostile system?
- Boss encounter design for faction leaders
