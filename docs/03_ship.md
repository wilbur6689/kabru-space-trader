# Ship

## Overview
This document defines how ships are created, upgraded, and maintained throughout the game. Ships are purchased at **hub system shipyards** and are the player's primary tool for exploration and trade.

---

## Ship Attributes

### Core Stats
- Name (player-chosen)
- Hull type / class
- Hull integrity (HP)
- Shield strength
- Cargo capacity (units)
- Jump drive range (determines which systems are reachable)
- Scanner quality (may influence discovery outcomes)
- Maneuverability (combat evasion)
- Weapon slots / firepower

### Ship Classes
| Class | Cargo | Jump Range | Hull | Combat | Cost | Notes |
|-------|-------|------------|------|--------|------|-------|
| Shuttle | Small | Short | Low | Weak | Cheap | Starter/loaner ship |
| Freighter | Large | Medium | Medium | Weak | Medium | Trading focused |
| Corvette | Medium | Medium | Medium | Good | High | Balanced |
| Hauler | Very Large | Short | High | Weak | High | Max cargo |
| Clipper | Medium | Long | Low | Medium | Very High | Exploration focused |
| Gunship | Small | Medium | High | Strong | Very High | Combat focused |

---

## Ship Systems

### Components
- **Jump Drive** — Determines maximum jump range between systems. Primary exploration progression gate. Upgradeable to reach farther systems and new regional clusters.
- **Hull Plating** — Affects durability and damage resistance
- **Shields** — Absorbs damage before hull takes hits
- **Cargo Bay** — Determines cargo storage capacity
- **Scanner** — Detection quality; may affect discovery results when searching systems
- **Weapons** — Offensive capability for combat encounters
- **Engine** — Affects maneuverability in combat and travel efficiency
- **Navigation Computer** — Route planning; calculates shortest jump paths to 6-digit destinations

### Component Upgrades
- Upgrades available at **hub system shipyards** and some **industrial sub-systems**
- Multiple upgrade tiers per component
- Cost scales with tier
- Jump drive upgrades are the most impactful — they open new regions of the galaxy
- Upgrades are permanent until replaced with a higher tier

---

## Ship Maintenance

### Wear and Tear
- Ship takes damage from combat encounters, environmental hazards, and dangerous discoveries
- Repairs available at hub systems (full repair) and some sub-systems (basic repair)
- Emergency repairs may be possible with salvaged parts from dead systems

### Damage States
- Full health — fully operational
- Damaged — reduced performance (lower combat stats, scanner degradation)
- Critical — risk of component failure, visual damage indicators
- Destroyed — player dies, respawn mechanic triggers

---

## Cargo Management

### Cargo Hold
- Maximum capacity determined by ship class + cargo bay upgrades
- Cargo measured in units
- Different goods may have different unit sizes
- Contraband cargo carries risk if scanned by patrols in friendly systems

### Cargo Operations (Free Actions)
- Buying and selling cargo does **not cost a turn**
- Loading and unloading is instant
- Jettison cargo option for emergencies (permanently lost)

### Cargo and Death
- When a player dies, their ship remains at the death location
- **0% to 50%** of cargo is looted by enemies (random)
- Remaining cargo stays with the ship for recovery
- Player must return to the ship location to reclaim it

---

## Ship Acquisition

### Starting Ship
- Player begins the tutorial with a **dilapidated ship** that is broken down
- Must repair it as part of the tutorial sequence
- After tutorial, this becomes the player's first functional ship (low-tier)

### Loaner Ship (Death Respawn)
- When the player dies and respawns at the last merchant, they receive a **basic loaner/escape pod**
- Loaner has minimal stats — small cargo, short jump range, no weapons
- Enough to travel back and recover the original ship
- Can also be used to earn credits for a new ship if the original is too far away

### Buying New Ships
- New ships are available at **hub system shipyards only**
- Player can browse available ship classes
- Trade-in value for current ship applied to purchase
- Cargo must be managed before trade-in (sell, transfer, or lose)
- Upgrades on the old ship do **not** transfer — they are lost or factored into trade-in value

---

## Ship Destruction

### What Causes Destruction
- Hull integrity reaches zero during combat
- Critical damage from environmental hazards
- Certain dangerous discovery encounters

### Death and Recovery Flow
1. Ship is destroyed → player "dies"
2. Player respawns at **last visited merchant**
3. Player receives a **basic loaner ship**
4. Original ship wreckage remains at the location where the player died
5. 0-50% of cargo in the wreckage is looted by enemies
6. Player can travel back to recover the ship and remaining cargo
7. Alternatively, player can buy a new ship at the hub and start fresh

---

## Notes
_Add design notes, open questions, and decisions here._

### Open Design Questions
- Can the player own multiple ships?
- Can ships be stored at hub systems?
- Are there rare/unique ships discoverable in dead or hostile systems?
- Exact upgrade tier count and cost scaling
- Scanner impact on search/discovery mechanics
