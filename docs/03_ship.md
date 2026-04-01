# Ship

## Overview
This document defines how ships are created, upgraded, and maintained throughout the game. Ships are purchased at **hub system shipyards** and are the player's primary tool for exploration and trade.

---

## Ship Classes

### Class Overview
There are 6 ship classes. 5 main classes come in **small, medium, and large** variants with increasing stats in their specialty. The Shuttle is a single-size utility class.

| Class | Role | Sizes | Key Strength |
|-------|------|-------|-------------|
| Shuttle | Starter / loaner / utility | Single | Cheap, basic transport |
| Freighter | Trading focused | Small, Medium, Large | Cargo capacity |
| Corvette | Balanced all-rounder | Small, Medium, Large | No major weakness |
| Hauler | Max cargo hauling | Small, Medium, Large | Massive cargo capacity |
| Clipper | Exploration focused | Small, Medium, Large | Jump range, scanner |
| Gunship | Combat focused | Small, Medium, Large | Weapons, hull, shields |

**Total: 16 standard ships** (1 Shuttle + 15 variants of 5 classes)

### Size Overlap
Sizes overlap between classes — a small Hauler is only slightly better at cargo than a large Freighter. This creates meaningful purchase decisions where the player weighs specialization vs. versatility.

### Ship Stats by Class and Size

#### Shuttle (Single Size)
| Component | Max Tier |
|-----------|----------|
| Jump Drive | 1 |
| Hull Plating | 1 |
| Shields | 1 |
| Cargo Bay | 1 |
| Scanner | 1 |
| Weapons | 0 (none) |
| Engine | 1 |
| Nav Computer | 1 |

#### Freighter (Trading Focused)
| Component | Small | Medium | Large |
|-----------|-------|--------|-------|
| Jump Drive | 2 | 3 | 3 |
| Hull Plating | 2 | 3 | 3 |
| Shields | 1 | 2 | 2 |
| Cargo Bay | 3 | 4 | 5 |
| Scanner | 2 | 2 | 3 |
| Weapons | 1 | 1 | 2 |
| Engine | 2 | 2 | 3 |
| Nav Computer | 2 | 3 | 4 |

#### Corvette (Balanced)
| Component | Small | Medium | Large |
|-----------|-------|--------|-------|
| Jump Drive | 2 | 3 | 4 |
| Hull Plating | 2 | 3 | 4 |
| Shields | 2 | 3 | 3 |
| Cargo Bay | 2 | 3 | 3 |
| Scanner | 2 | 3 | 3 |
| Weapons | 2 | 3 | 4 |
| Engine | 2 | 3 | 4 |
| Nav Computer | 2 | 3 | 3 |

#### Hauler (Max Cargo)
| Component | Small | Medium | Large |
|-----------|-------|--------|-------|
| Jump Drive | 1 | 2 | 2 |
| Hull Plating | 3 | 3 | 4 |
| Shields | 1 | 2 | 2 |
| Cargo Bay | 4 | 5 | 5 |
| Scanner | 1 | 2 | 2 |
| Weapons | 1 | 1 | 1 |
| Engine | 1 | 1 | 2 |
| Nav Computer | 2 | 3 | 3 |

#### Clipper (Exploration Focused)
| Component | Small | Medium | Large |
|-----------|-------|--------|-------|
| Jump Drive | 3 | 4 | 5 |
| Hull Plating | 1 | 2 | 2 |
| Shields | 2 | 2 | 3 |
| Cargo Bay | 2 | 2 | 3 |
| Scanner | 3 | 4 | 5 |
| Weapons | 1 | 2 | 2 |
| Engine | 3 | 3 | 4 |
| Nav Computer | 3 | 4 | 5 |

#### Gunship (Combat Focused)
| Component | Small | Medium | Large |
|-----------|-------|--------|-------|
| Jump Drive | 2 | 2 | 3 |
| Hull Plating | 3 | 4 | 5 |
| Shields | 3 | 4 | 5 |
| Cargo Bay | 1 | 1 | 2 |
| Scanner | 2 | 2 | 3 |
| Weapons | 3 | 4 | 5 |
| Engine | 3 | 3 | 4 |
| Nav Computer | 1 | 2 | 2 |

### Black Market Ships
- Available only at **Outlaw Hubs**
- Special variant of each of the 5 main classes (not Shuttle)
- **Unique names** (naming convention TBD)
- **Visually identical** to standard ships (for now — see deferred design doc)
- **Very expensive** but have superior stats in specific areas
- 5 total black market ships (one per main class)

---

## Ship Availability by Location

| Location | Available Ships |
|----------|----------------|
| **Shipyard Hub** | All 5 main classes — **small size only** |
| **Military Hub** | Gunships and Corvettes — medium + large |
| **Trade Hub** | Freighters and Haulers — medium + large |
| **Tech Hub** | Clippers and Corvettes — medium + large |
| **Science Hub** | Clippers — medium + large |
| **Outlaw Hub** | Black market variants of all 5 main classes |
| **Sub-systems** | No ships — parts and upgrades only |

---

## Ship Systems and Components

### Components (8 Total)
Every ship has all 8 component types. The **maximum upgrade tier** per component varies by ship class and size (see tables above).

#### Jump Drive
- Determines maximum jump range between regions
- **Primary exploration progression gate**
- 5 tiers with increasing region range:

| Tier | Region Range | Notes |
|------|-------------|-------|
| 1 | Up to 5 regions | Starting cluster + immediate neighbors |
| 2 | Up to 10 regions | Nearby regions open up |
| 3 | Up to 20 regions | Mid-game, most hub regions reachable |
| 4 | Up to 40 regions | Deep space, far wild space |
| 5 | Unlimited | Full galaxy access |

#### Hull Plating
- Increases **hull integrity (HP)** and **damage reduction** percentage
- Higher tiers = more HP + higher percentage of incoming damage absorbed

#### Shields
- Provides a **shield HP buffer** that must be depleted before hull takes damage
- Higher tiers = more shield HP + **shield recharge** between combat turns
- Single damage type for now (see deferred design doc for future damage type variants)

#### Cargo Bay
- Increases **cargo capacity** (units)
- Higher tiers reduce **cargo loss percentage on death** (e.g., Tier 5 loses less than Tier 1)
- Includes **special slots** for non-trade items (station modules, mission objects, special cargo)
- **Tier 1 starts with 1 special slot**, higher tiers add more

#### Scanner
- Improves **discovery quality** when searching systems (better chance of valuable locations vs. dangers)
- Provides **pre-jump intel** about destination systems (type, danger level, number of locations)
- Higher tiers reveal more detailed info before committing to a jump

#### Weapons
- Tiers 1-2: Increase **damage output**
- Tiers 3-4: Increase **damage + accuracy** (hit chance)
- Tier 5: Unlocks **double weapons** — two weapons fire per combat turn

#### Engine
- Increases **maneuverability** (combat evasion chance)
- Higher tiers increase **flee success rate** when attempting to escape combat

#### Navigation Computer
- Optimizes **route calculation** (fewer intermediate jumps to destination)
- Reveals **system intel** about destinations: available resources, specialties, trade opportunities
- Higher tiers provide more detailed trading info before jumping — directly supports the core trading gameplay

### Component Upgrade Availability

| Location | Tiers Available | Notes |
|----------|----------------|-------|
| Sub-systems (industrial) | Tier 1-2 | Wholesale prices, cheaper |
| Regular hubs | Tier 1-3 | Standard pricing |
| Specialized hubs | Tier 1-5 (specialty components) | Tech Hub: scanners, nav computers. Military Hub: weapons, shields. Etc. |

---

## Ship Maintenance

### Repair Only — No Ongoing Costs
- No docking fees at any hub
- No periodic maintenance costs
- Player only pays for **repairs after taking damage**
- Hostile systems may charge a **tax** before the player can conduct business

### Damage Sources
- Combat encounters
- Environmental hazards
- Dangerous discoveries during searches

### Damage States
- **Full health** — Fully operational, all components working normally
- **Damaged** — Reduced performance (lower combat stats, scanner degradation)
- **Critical** — Risk of component failure, visual damage indicators
- **Destroyed** — Player dies, respawn mechanic triggers

### Repair Services
- **Hub systems**: Full repair, all components
- **Sub-systems**: Basic repair (hull only, limited)
- **Dead systems**: Emergency repairs possible with salvaged parts
- **Personal stations**: Repair available at Tier 2+ stations

---

## Cargo Management

### Cargo Hold
- Maximum capacity determined by ship class + cargo bay upgrades
- Cargo measured in **units**
- Different goods may have different unit sizes
- Contraband cargo carries risk if scanned by patrols in friendly systems

### Special Cargo Slots
- Separate from regular cargo capacity
- Used for: station deployment modules, mission-specific objects, special cargo
- Starting at **1 special slot** at Cargo Bay Tier 1
- Higher cargo bay tiers add more special slots

### Cargo Operations (Free Actions)
- Buying and selling cargo does **not cost a turn**
- Loading and unloading is instant
- Jettison cargo option for emergencies (permanently lost)

### Cargo and Death
- When a player dies, their ship remains at the death location
- **0% to 50%** of cargo is looted by enemies (reduced by cargo bay tier)
- Remaining cargo stays with the ship for recovery
- Player must return to the ship location to reclaim it

---

## Ship Ownership

### Multiple Ships
- Player can own up to **5 ships total** (including the one currently being flown)
- Ships can be stored at any hub — **1 stored ship per hub**
- Player can swap to a stored ship when visiting that hub

### Ship Transfer Service
- Hubs offer a service to **move stored ships** between hubs for a fee
- Delivery is **delayed** — ship arrives after a number of turns based on distance between hubs
- Player must plan ahead for which ships they want where

---

## Ship Acquisition

### Tutorial Sequence
1. Player begins with a **broken Shuttle** in a sub-system (single cargo slot, barely functional)
2. Player repairs the Shuttle as part of the tutorial
3. Player delivers cargo to the regional hub
4. The NPC receiving the cargo is impressed and **awards the player a Small Freighter**
5. The Shuttle is **traded in automatically** as part of the deal
6. Player begins the open game with the Small Freighter

### Buying New Ships
- New ships available at **hub shipyards** (class/size varies by hub specialization)
- Player browses available ships and compares stats
- **Trade-in value** applied to purchase: percentage of ship's base price + percentage of credits spent on upgrades
- Cargo must be managed before trade-in (sell, transfer to station, or lose)
- Upgrades on the old ship do **not transfer** — factored into trade-in value only

### Loaner Ship (Death Respawn)
- When the player dies, they respawn at the last visited hub with a **temporary loaner Shuttle**
- Loaner has minimal stats — small cargo, Tier 1 jump drive, no weapons
- Enough to travel back and recover the original ship or earn credits for a new one
- **Temporary ownership only** — loaner despawns when the player recovers their ship or buys a new one
- Loaner is not tracked by the game after despawn

---

## Ship Destruction and Recovery

### What Causes Destruction
- Hull integrity reaches zero during combat
- Critical damage from environmental hazards
- Certain dangerous discovery encounters

### Death and Recovery Flow
1. Ship is destroyed → player "dies"
2. Player respawns at **last visited hub**
3. Player receives a **temporary loaner Shuttle**
4. Original ship wreckage remains at the death location
5. **0-50% of cargo** in the wreckage is looted by enemies
6. Player travels to the wreckage location in the loaner
7. **Automatic swap** — loaner despawns, player is back in their ship
8. Recovered ship is **damaged** — travel restricted to current region until repaired
9. Alternatively, player can buy a new ship at a hub and abandon the wreckage

---

## Discoverable Ships

### Derelict Ships
- Rare finds in **dead systems** and **hostile systems**
- Heavily damaged and non-functional
- Player selects the derelict to be **towed to a hub shipyard**
- Pays a **towing fee** — ship is delivered to the destination hub (delayed, like ship transfer)
- Once at the hub, player pays for repairs to make it functional
- May be unique variants or standard classes in poor condition

### Blueprints
- Found through exploration in dead and hostile systems
- Used at **personal stations** with gathered resources for **unique ship upgrades**
- Not available at any hub — exclusive to the blueprint crafting system
- Details TBD (see deferred design doc)

---

## Notes
_Add design notes, open questions, and decisions here._

### Open Design Questions
- Black market ship names and specific stat bonuses
- Exact base costs per ship class and size
- Exact upgrade costs per component per tier
- Trade-in value percentages (base price % and upgrade %)
- Cargo unit sizes per trade good type
- Special slot count progression per cargo bay tier
- Ship transfer and towing delivery time formula
- Hostile system tax amounts
- Derelict ship repair costs at hub
- Blueprint crafting system (deferred)
- Damage type variants for shields/weapons (deferred)
- Black market ship visual design (deferred)
