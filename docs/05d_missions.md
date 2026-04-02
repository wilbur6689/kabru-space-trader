# Mission System

## Overview
Missions provide structured objectives with rewards, reputation effects, and story progression. They come from legitimate factions, pirate factions, or neutral bulletin boards. Active mission slots are gated by fame level.

---

## Mission Types
| Type | Description | Risk | Reward | Source |
|------|-------------|------|--------|--------|
| Delivery | Transport cargo to a destination address | Travel encounters, deadlines | Credits | Hub bulletin board |
| Smuggling | Move contraband past patrols | Patrol scans, fines | High credits, pirate fame | Pirate faction contacts |
| Bounty | Hunt a target in a hostile system | Combat | Credits, reputation | Hub mission board |
| Rescue | Free imprisoned merchant/NPC | Combat, hostile system | Reputation, loyalty discounts | Discovery or NPC tip |
| Exploration | Chart unknown system addresses | Unknown dangers | Discovery XP, reputation | Information broker |
| Clearance | Eliminate hostile faction from a system | Heavy combat | System conversion, major reputation | Story or hub mission |
| Investigation | Gather intel from data caches | Hostile territory | Lore, system addresses, story progression | Story events |

---

## Mission Sources

### Legitimate Faction Missions
- Offered at hub systems affiliated with a faction
- Completing them improves reputation with that faction
- **Ripple effect**: Allied factions improve, rival factions decrease
- Types: Delivery, Bounty, Rescue, Clearance, Investigation

### Pirate Faction Missions
- Offered through black market contacts and pirate-affiliated NPCs
- Completing them increases **pirate fame** with that faction
- May decrease legitimate faction reputation if discovered
- Types: Smuggling, Bounty (target is a legitimate faction member), Delivery (contraband)

### Neutral Missions
- Found on bulletin boards with no faction affiliation
- Pure credit reward, no reputation effects
- Types: Delivery, Exploration, Rescue

---

## Mission Difficulty Scaling
- **Region distance** determines base mission difficulty and reward
- **Fame level** unlocks premium mission tiers within each region
- Higher fame + distant regions = hardest missions with best payouts

### Reward Multipliers
- Region distance multiplier: +2-5% per region from center
- Fame level multiplier: +3-8% per fame level
- Combined: base reward x region modifier x fame modifier

---

## Mission Deadlines
- **Some missions have turn limits** — delivery and rescue missions may expire
- **Other missions are open-ended** — bounty, exploration, clearance have no deadline
- Failing a deadline mission costs **reputation** with the issuing faction
- Player can manually **abandon** any mission (minor reputation loss)

---

## Active Mission Slots
- Number of simultaneous active missions is limited by **fame level**
- **Starting slots: 3**
- **Maximum slots: 10** at highest fame level
- Slots increase as fame grows — roughly +1 slot every 1-2 fame levels
- Forces early-game prioritization, enables late-game multi-tasking

---

## Mission Board Mechanics
- Each hub has a **mission board** with available missions
- Mission board refreshes periodically (every N turns or on each visit)
- Available missions reflect the hub's specialization:
  - Military Hub: More bounty and clearance missions
  - Trade Hub: More delivery missions with better credit rewards
  - Outlaw Hub: Smuggling and pirate faction missions
  - Science Hub: Exploration and investigation missions
- Mission difficulty and rewards scale with the region

---

## Open Design Questions
- Mission reward scaling formula (exact region distance x fame calculation)
- Mission slot progression curve (3 to 10 across 12 fame levels)
- Mission board refresh rate and generation rules
- How many missions are available per board per visit
- Whether missions can chain into multi-part sequences
- Mission failure consequences beyond reputation loss
- Whether completed missions leave any lasting world effects
- Smuggling mission specifics — route planning, patrol avoidance mechanics
- Rescue mission specifics — how rescued NPCs become loyal contacts
- How investigation missions connect to the main story arc
