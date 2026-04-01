# Pilot

## Overview
This document defines the player character (pilot), their attributes, progression, and role in the game. The pilot and their ship are treated as **one entity** — the player never leaves the ship they're currently piloting.

---

## Pilot Identity

### Character Creation
- **Name** — Player-chosen
- **Portrait** — Selected from a set of pre-made portraits
- **Background** — Chosen from 5 options, affects starting skill bonus and lore

---

## Pilot Backgrounds

### Starting Background Options
All pilots begin the game with the same **tutorial scenario** — stranded with a broken ship in a sub-system. Background choice gives a **+2 bonus** to the specialty skill. All backgrounds are competent pilots — there is no dedicated Piloting background.

| Background | Starting Bonus | Skill Boost (+2) | Lore |
|------------|---------------|-------------------|------|
| Merchant | Extra credits | Trading | Former corporate trader making deliveries |
| Explorer | Scanner upgrade | Exploration | Charted unknown systems for a living |
| Smuggler | Stealth mod | Negotiation | Ran contraband through hostile territory |
| Mechanic | Repair kit | Engineering | Built and repaired ships at a shipyard |
| Military | Weapon upgrade | Combat | Discharged veteran of a faction war |

All backgrounds share the same tutorial: the delivery run that went wrong.

---

## Core Stats

- **Credits** — Currency for trading, purchases, repairs
- **Fame Level** — Meta-progression earned through skill usage (see Fame section)
- **Reputation** — Standing per legitimate faction (5 tiers)
- **Pirate Fame** — Standing per pirate faction (4 factions, separate tracking)

---

## Skill System

### Overview
- **6 skills**, each with levels **0-10**
- All skills start at **0**, with background giving **+2** to specialty skill
- Skills level up through **usage-based XP** — learn by doing, not point allocation
- Flat incremental bonus per level (each level improves effectiveness)

### Skills and XP Sources

| Skill | Effect Per Level | XP Earned By |
|-------|-----------------|-------------|
| **Trading** | Better buy/sell prices, market insight, price trend visibility | Completing buy/sell transactions, profit earned |
| **Piloting** | Maneuverability in combat, evasion chance | Successful flee attempts, traveling through unknown territory |
| **Engineering** | Cheaper repairs, self-repair ability, component maintenance | Repairing ship, using salvaged parts, building parts from blueprints, building/upgrading personal stations |
| **Negotiation** | Better mission rewards, NPC interactions, rescue outcomes | NPC interactions, mission reward bonuses, successful rescues, successful bartering with shop owners |
| **Combat** | Weapon accuracy, damage output, combat encounter success | Winning fights, dealing damage, defeating enemies |
| **Exploration** | Improved search outcomes, scanner effectiveness, discovery quality | Discovering new previously un-identified locations, visiting new systems |

---

## Fame System

### How Fame Works
- Fame is a **meta-progression layer** on top of usage-based skills
- Every **5 combined skill levels** earned across all skills grants **1 fame point**
- Fame points are **manually allocated** by the player to any area they choose
- Maximum **12 fame points** (6 skills x 10 levels = 60 total / 5 = 12)

### Fame Effects

#### Pricing
- Higher fame grants better buy/sell prices at systems that **align with the player's highest skill and/or background**
- A famous trader gets better deals at trade hubs, a famous explorer gets discounts on scanner equipment, etc.

#### Intimidation
- If the player's fame level is **more than 2 levels above** an enemy's fame level, the enemy will attempt to **flee** rather than fight
- Example: Player fame 9, pirate fame 5 → pirates flee on sight
- Works both ways — high-fame enemies won't be intimidated by low-fame players

#### NPC Access
- Certain NPCs only interact with the player at **higher fame levels**
- Information brokers, faction leaders, and exclusive merchants may require minimum fame thresholds

#### Events and Missions
- Certain story events and missions only unlock at specific fame levels
- Details deferred to main story arc design (see deferred design doc)

---

## Reputation System

### Legitimate Faction Reputation
- Reputation tracked per **faction** across their controlled regions
- Factions may control **1 to several regions** throughout the galaxy

#### 5 Reputation Tiers
| Tier | Name | Effects |
|------|------|---------|
| 1 | **Hated** | Faction actively hostile, sends bounty hunters, no access to services |
| 2 | **Unfriendly** | Higher prices, limited mission access, increased patrol scrutiny |
| 3 | **Neutral** | Standard prices and access, default starting state |
| 4 | **Friendly** | Better prices, more mission options, NPC tips and intel |
| 5 | **Allied** | Best prices, exclusive missions, faction-specific rewards and access |

#### Reputation Changes
- **Gained by**: Completing missions, trading, rescuing NPCs, clearing hostile systems in faction territory
- **Lost by**: Smuggling contraband, attacking friendlies, failing missions, aiding rival factions

#### Ripple Effect
- Factions have **relationships** with each other (allied, neutral, rival)
- **Major actions** (clearing a hostile system, completing faction quests) affect related factions:
  - Allied factions: Reputation improves
  - Rival factions: Reputation decreases
- This influences which parts of the galaxy the player can safely explore
- Factions control territory across multiple regions — allegiance choices have galaxy-wide consequences

---

## Pirate Factions

### 4 Pirate Factions
Each pirate faction controls a **quadrant of the galaxy**, with influence strongest near their hub and at the galaxy's edges. The center (near region 00) is the safest zone where all four factions are weakest.

| Faction | Specialty | Quadrant | Style |
|---------|-----------|----------|-------|
| **The Void Reavers** | Raiding and combat | Quadrant 1 (spinward) | Aggressive fleet attacks, ship destruction, demand tribute. Military rejects and war criminals. Strongest naval force of the four. |
| **The Silk Hand** | Smuggling and contraband | Quadrant 2 (coreward) | Subtle, trade-focused. Most profitable contraband networks. Prefer bribery over violence. Well-connected informants everywhere. |
| **The Rust Collective** | Salvage and territory control | Quadrant 3 (rimward) | Scavengers who strip dead systems and claim territory. Hoard resources and tech. Build improvised stations from wreckage. Most likely to occupy hostile systems. |
| **The Phantom Circuit** | Black market trading and intel | Quadrant 4 (trailing) | Information brokers and tech dealers. Run the best black markets. Sell system addresses, blueprints, and stolen data. Least violent but most manipulative. |

### Pirate Fame (Per Faction)
- Tracked **separately** from legitimate faction reputation
- **Increased by**: Any discovered illegal activity in the regional pirate faction's territory
- **Effects of high pirate fame**:
  - Pirates stop attacking you
  - Better black market prices
  - Pirate-exclusive missions offered
  - Access to pirate intel (hidden system addresses, stolen data)
- **Consequences of high pirate fame**:
  - Legitimate faction patrols target you more
  - Some hub NPCs may refuse to deal with you
  - Bounty hunters may appear

### Pirate Engagement Reasons
| Faction | Reason to Engage |
|---------|-----------------|
| **Void Reavers** | High combat fame, tribute protection, combat missions |
| **Silk Hand** | Best smuggling missions and contraband routes |
| **Rust Collective** | Rare salvage access and resource deals |
| **Phantom Circuit** | Intel, blueprints, and hidden system addresses |

---

## Death and Respawn

### What Happens When the Pilot Dies
1. Player respawns at the **last visited hub**
2. Receives a **temporary loaner Shuttle** (despawns when no longer needed)
3. Original ship and remaining cargo (minus 0-50% looted) stay at death location
4. Player can travel to recover ship (automatic swap, ship is damaged, restricted to current region)
5. Player can buy a new ship at the hub if they have credits
6. **No permanent stat or skill loss** — death is a setback, not a reset
7. Player is always able to recover

---

## Pilot Log / Journal

### Tracked Information
- All visited systems (with XX-XXXX addresses)
- Discovery progress per system (X / Y locations)
- Known system addresses (from exploration, bulletin boards, NPCs, star charts, missions)
- Active and completed missions / quest history
- Trade price history and trends per system
- **Faction reputation standings** (all legitimate factions)
- **Pirate fame per faction** (all 4 pirate factions)
- **Overall fame level** and progress toward next fame point
- **Owned ships** and their current storage locations
- **Personal station locations** and tier levels
- Statistics:
  - Total profit earned
  - Systems visited
  - Discoveries made
  - Enemies defeated
  - Hostile systems cleared
  - Missions completed

### Journal as Gameplay Tool
- The log serves as the player's **personal star chart**
- Previously discovered addresses are always accessible
- Price history helps inform trading decisions
- Discovery progress motivates returning to partially explored systems
- Faction standings help plan travel routes through friendly territory
- Ship/station tracking helps manage the player's assets across the galaxy

---

## Notes
_Add design notes, open questions, and decisions here._

### Open Design Questions
- Exact XP thresholds per skill level (0-10 curve)
- Flat incremental bonus values per skill per level
- Fame point allocation options (what can fame points be spent on specifically?)
- Legitimate faction names, territories, and relationship matrix
- Pirate faction hub locations and influence radius per quadrant
- Portrait art style and number of options
- Bartering mechanic details (how does negotiation skill affect shop interactions?)
