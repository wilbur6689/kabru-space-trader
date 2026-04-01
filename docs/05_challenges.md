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
| Contaminated Zone | Dead | Hull/component damage | Rare materials if scanned |

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
| Derelict Ship | Trap risk | Salvage, rare goods, tow opportunity | Unknown territory |
| Patrol Checkpoint | Contraband scan | None (legal) / Fines (contraband) | Known friendly routes |
| Merchant Convoy | None | Trading opportunity | Known routes |
| Story Event | Varies | Main story or side quest trigger | New region entry |
| Pirate Turf War | Combat crossfire | Loot from both sides | Border regions between pirate factions |

---

## Combat System

### Combat Overview
- Combat is **turn-based**, consistent with the overall game
- Triggered by dangerous discoveries, travel encounters, or hostile system events
- All entities (player, enemies) use the **MasterEntity** system with fame levels
- Enemy behavior varies by **pirate faction affiliation**

### Combat Actions (5 Total)

| Action | Effect | Requirements |
|--------|--------|-------------|
| **Attack** | Deal damage based on weapons + combat skill | Always available |
| **Defend** | Reduce incoming damage, prepare counter | Always available |
| **Flee** | Attempt escape based on engine maneuverability + piloting skill | Always available |
| **Negotiate** | Attempt to talk down — costs credits, uses negotiation skill | Always available |
| **Intimidate** | Attempt to scare enemy into paying you and retreating | Only available when player fame is **equal to or up to 2 above** enemy fame |

### Intimidation System

#### Pre-Combat Fame Check
- **Player fame 3+ above enemy**: Combat **skipped entirely** — enemy flees on sight
- **Player fame equal to or up to 2 above**: Combat starts with **Intimidate** available as a 5th action
- **Player fame below enemy**: No intimidate option — must fight, flee, or negotiate

#### Intimidate Outcomes
- **Success**: Enemy pays credits to the player and retreats
- **Failure**: Enemy is angered — attacks with a **minor damage bonus** on their next turn

### Combat Flow
```
Encounter Triggered
  -> Fame comparison check
     -> If player fame 3+ above: enemy flees, combat skipped
     -> Otherwise: combat begins
  -> Present enemy info (type, strength, ship class, faction, fame level)
  -> Player chooses action:
     a) Attack — deal damage based on weapons + combat skill
     b) Defend — reduce incoming damage, prepare counter
     c) Flee — attempt escape based on maneuverability + piloting skill
     d) Negotiate — attempt to talk down (negotiation skill, costs credits)
     e) Intimidate — scare enemy (only if fame is equal to or up to 2 above)
  -> Resolve player action
  -> Enemy responds based on faction behavior
  -> Update ship status (hull, shields)
  -> Check resolution:
     - Victory: enemy defeated → loot
     - Defeat: ship destroyed → death/respawn
     - Escape: player flees → no loot, possible cargo loss
     - Negotiated: pay off or talk down → credits lost, no damage
     - Intimidated: enemy pays credits → retreats
  -> Loop or end
```

---

## Enemy System

### Enemy Fame Levels
All enemies are **MasterEntity** objects with fame levels. Fame is calculated as:

```
enemy_fame = base_fame + (region_distance / 5)
```

Where `region_distance` is the region number's distance from center (region 00).

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

### Example Scaling
| Enemy | Region 00 | Region 10 | Region 25 | Region 40 |
|-------|-----------|-----------|-----------|-----------|
| Petty Pirate | Fame 1 | Fame 3 | Fame 6 | Fame 9 |
| Pirate Captain | Fame 4 | Fame 6 | Fame 9 | Fame 12 |
| Warlord | Fame 8 | Fame 10 | Fame 13 | Fame 16 |

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

### Border Region Encounters
- Regions on the **borders between pirate faction territories** contain enemies from **both factions**
- These factions are **fighting over territory** — turf wars
- Player may encounter mixed groups or stumble into ongoing battles
- Can loot from both sides if caught in a turf war event

---

## Clearing Hostile Systems

### Progressive Clearing Mechanic
Hostile systems are controlled by a pirate faction. The player must weaken the faction's hold by discovering and eliminating enemy outposts before the boss comes looking for them.

### Clearing Process
1. Player enters a hostile system and begins **searching** for locations
2. Some discoveries are **enemy outposts** — combat encounters
3. Player must clear a **required number of outposts** to trigger the boss
4. Required outpost count **scales with region distance**:
   - Near center (region 00-10): 2 outposts
   - Mid-range (region 11-25): 3 outposts
   - Far regions (region 26-40): 4 outposts
   - Deep space (region 40+): 5 outposts
5. After clearing enough outposts, the **boss appears on the next search/turn**
6. Boss **warns the player** that an attack is coming in **2-3 turns**
7. Player has a minimum of **1 turn to leave** and prepare (repair, upgrade, restock)
8. When the player **returns to the system**, the boss attacks

### Boss Mechanics
- Boss is the **faction Warlord** — highest fame enemy in the system
- Boss fame = base 8 + region modifier
- Defeating the boss **converts the system from hostile to friendly**
- System reverts to its pre-occupation state with basic services

### Boss Rebuilds
- If the player **does not return for too long**, the faction begins **rebuilding outposts**
- Player must re-clear some outposts before the boss is triggered again
- Prevents indefinite delay — creates urgency without a hard timer

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

## Environmental Hazards

### Space Hazards (During Travel)
- Asteroid fields — hull damage risk
- Nebulae — reduced scanner effectiveness
- Gravity wells — navigation complications
- Radiation zones — component damage

### System Hazards (During Search/Discovery)
- Unstable structures in dead systems
- Contaminated zones on dead worlds
- Hostile environment on primitive planets
- Trap mechanisms in ancient ruins

---

## Mission System

### Mission Types
| Type | Description | Risk | Reward | Source |
|------|-------------|------|--------|--------|
| Delivery | Transport cargo to a destination address | Travel encounters, deadlines | Credits | Hub bulletin board |
| Smuggling | Move contraband past patrols | Patrol scans, fines | High credits, pirate fame | Pirate faction contacts |
| Bounty | Hunt a target in a hostile system | Combat | Credits, reputation | Hub mission board |
| Rescue | Free imprisoned merchant/NPC | Combat, hostile system | Reputation, loyalty discounts | Discovery or NPC tip |
| Exploration | Chart unknown system addresses | Unknown dangers | Discovery XP, reputation | Information broker |
| Clearance | Eliminate hostile faction from a system | Heavy combat | System conversion, major reputation | Story or hub mission |
| Investigation | Gather intel from data caches | Hostile territory | Lore, system addresses, story progression | Story events |

### Mission Sources

#### Legitimate Faction Missions
- Offered at hub systems affiliated with a faction
- Completing them improves reputation with that faction
- **Ripple effect**: Allied factions improve, rival factions decrease
- Types: Delivery, Bounty, Rescue, Clearance, Investigation

#### Pirate Faction Missions
- Offered through black market contacts and pirate-affiliated NPCs
- Completing them increases **pirate fame** with that faction
- May decrease legitimate faction reputation if discovered
- Types: Smuggling, Bounty (target is a legitimate faction member), Delivery (contraband)

#### Neutral Missions
- Found on bulletin boards with no faction affiliation
- Pure credit reward, no reputation effects
- Types: Delivery, Exploration, Rescue

### Mission Difficulty Scaling
- **Region distance** determines base mission difficulty and reward
- **Fame level** unlocks premium mission tiers within each region
- Higher fame + distant regions = hardest missions with best payouts

### Mission Deadlines
- **Some missions have turn limits** — delivery and rescue missions may expire
- **Other missions are open-ended** — bounty, exploration, clearance have no deadline
- Failing a deadline mission costs **reputation** with the issuing faction
- Player can manually **abandon** any mission (minor reputation loss)

### Active Mission Slots
- Number of simultaneous active missions is limited by **fame level**
- **Starting slots: 3**
- **Maximum slots: 10** at highest fame level
- Slots increase as fame grows — roughly +1 slot every 1-2 fame levels
- Forces early-game prioritization, enables late-game multi-tasking

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

### Pirate Turf War Events
- Occur in **border regions** between pirate faction territories
- Mixed enemy types from both factions
- Higher danger but opportunity to profit from the chaos
- May affect regional pirate faction influence

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

## Notes
_Add design notes, open questions, and decisions here._

### Open Design Questions
- Exact encounter probability tables for travel
- Combat damage formulas and balance (see deferred doc)
- Specific loot tables per enemy type and faction
- Boss encounter combat design (phases, special abilities?)
- Outpost rebuild rate when player delays boss fight
- Mission reward scaling formula (region distance x fame)
- Mission slot progression curve (3 to 10 across 12 fame levels)
- Intimidation success rate formula
- Negotiation cost formula
- Environmental hazard damage values
