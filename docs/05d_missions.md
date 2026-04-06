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
| Clearance | Eliminate hostile faction from a system (uses progressive clearing mechanic from [05c](05c_hostile_systems.md)) | Heavy combat | System conversion, major reputation | Story or hub mission |
| Investigation | Gather intel from data caches | Hostile territory | Lore, system addresses, story progression | Story events |

---

## Mission Sources

### Legitimate Faction Missions
- Offered at Friendly systems with mission board discoveries
- Completing them improves reputation with that faction
- **Ripple effect**: Allied factions improve, rival factions decrease
- Types: Delivery, Bounty, Rescue, Clearance, Investigation

### Pirate Faction Missions
- Offered through black market contacts and pirate-affiliated NPCs
- Completing them increases **pirate fame** with that faction
- May decrease legitimate faction reputation if discovered
- Types: Smuggling, Bounty (target is a legitimate faction member), Delivery (contraband)

### Neutral Missions
- Found on bulletin boards with no faction affiliation — easy to find, widely available
- Pure credit reward, no reputation effects
- Medium difficulty — good for targeted resource gathering (blueprint materials, side objectives)
- Types: Delivery, Exploration, Rescue

---

## Mission Difficulty Scaling
- **Region distance** determines base mission difficulty and reward
- **Fame level** unlocks premium mission tiers within each region
- Higher fame + distant regions = hardest missions with best payouts

### Reward Multipliers
- Manhattan distance multiplier: +2-5% per Manhattan distance from origin
- Fame level multiplier: +3-8% per fame level
- Combined: base reward x region modifier x fame modifier

---

## Mission Deadlines
- **Some missions have turn limits** — delivery and rescue missions may expire
- **Other missions are open-ended** — bounty, exploration, clearance have no deadline
- **Deadline outcomes:**
  - **On time:** 100% reward
  - **1 turn late:** 50% reward, no reputation penalty
  - **More than 1 turn late:** no reward, reputation loss with issuing faction
- Deadlines are hard cutoffs — no extensions or negotiation
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
- Systems with a **mission board discovery** offer available missions
- Mission board has a **minimum wait time** before refreshing (exact duration TBD)
- Number of available missions **scales with system population tier** (Major systems offer more, Measly systems offer fewer)
- Available missions reflect the system's **alignment and regional specialty**:
  - Friendly systems with weapon upgrades: More bounty and clearance missions
  - Friendly systems with merchants: More delivery missions with better credit rewards
  - Pirate-Controlled systems / black markets: Smuggling and pirate faction missions
  - Systems with jump drive upgrades: Exploration and investigation missions
- Mission difficulty and rewards scale with Manhattan distance from origin

---

## Resolved Design Decisions
- **Reward multipliers:** Random per mission within range — encourages all playstyles equally
- **Deadline strictness:** Hard cutoff — on time = 100%, 1 turn late = 50%, >1 turn = fail + reputation loss. No extensions.
- **Mission board refresh:** Minimum wait time before refresh (exact duration TBD)
- **Missions per board:** Scaled by system population tier
- **Clearance missions:** Use the progressive clearing mechanic from [05c](05c_hostile_systems.md)
- **Pirate bounty targets:** No consequences for attacking legitimate faction members on pirate missions
- **Neutral missions:** Widely available, medium difficulty, good for targeted resource gathering

## Open Design Questions
- Mission reward scaling formula (exact region distance x fame calculation)
- Mission slot progression curve (3 to 10 across 12 fame levels)
- Mission board refresh minimum wait time
- Mission count range per population tier
- Whether missions can chain into multi-part sequences
- Whether completed missions leave any lasting world effects
- Smuggling mission specifics — route planning, patrol avoidance mechanics
- Rescue mission specifics — how rescued NPCs become loyal contacts
- How investigation missions connect to the main story arc
