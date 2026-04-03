# Game Balancing

## Overview
This document defines the numerical balance, progression curves, and tuning parameters for the game. Balance is critical because the game's economy operates on a **dynamic stock market model** and player progression is gated by **jump drive upgrades**. There are no difficulty settings — the galaxy itself is the difficulty selector based on where the player chooses to explore.

---

## Economy Balance

### Currency
- **Name:** Shards
- **Starting Amount:** 2,000 shards (earned after tutorial delivery)

### Common Goods Base Prices

#### Budget Tier (5-18 shards/unit)
| Good | Base Price (shards/unit) |
|------|------------------------|
| Food & Rations | 5-10 |
| Water & Ice | 5-8 |
| Agricultural Supplies | 8-12 |
| Textiles & Fibers | 10-18 |
| Recycled Scrap | 8-15 |

#### Low Tier (12-35 shards/unit)
| Good | Base Price (shards/unit) |
|------|------------------------|
| Raw Ores & Minerals | 12-20 |
| Fuel (Hydrogen, Helium-3) | 15-25 |
| Chemical Compounds | 18-30 |
| Atmospheric Gases | 20-35 |
| Civilian Tools & Equipment | 20-35 |

#### Mid Tier (25-65 shards/unit)
| Good | Base Price (shards/unit) |
|------|------------------------|
| Refined Metals | 25-45 |
| Construction Materials | 25-40 |
| Energy Cells & Batteries | 30-50 |
| Industrial Materials | 35-55 |
| Mechanical Parts | 40-65 |
| Consumer Goods | 40-70 |

#### Mid-High Tier (50-100 shards/unit)
| Good | Base Price (shards/unit) |
|------|------------------------|
| Manufactured Components | 50-80 |
| Medical Supplies | 55-90 |
| Ship Maintenance Supplies | 60-100 |

#### High Tier (65-140 shards/unit)
| Good | Base Price (shards/unit) |
|------|------------------------|
| Basic Electronics | 65-110 |
| Data Storage Devices | 70-120 |
| Terraforming Supplies | 80-140 |

### Rare Goods Base Prices

#### Low Rare (100-300 shards/unit)
| Good | Base Price (shards/unit) |
|------|------------------------|
| Smuggled Goods | 100-200 |
| Stellar Cartography Data | 120-220 |
| Luxury Items | 150-280 |
| Weapons & Ammunition | 150-300 |

#### Mid Rare (180-500 shards/unit)
| Good | Base Price (shards/unit) |
|------|------------------------|
| Designer Drugs & Stimulants | 180-350 |
| Cultural Treasures (Art, Sculptures) | 200-400 |
| Alien Flora & Fauna Specimens | 250-450 |
| Forbidden Research Data | 300-500 |

#### High Rare (350-950 shards/unit)
| Good | Base Price (shards/unit) |
|------|------------------------|
| Genetic Enhancements | 350-600 |
| Black Market Cybernetics | 400-700 |
| Experimental Technology | 450-800 |
| High-End AI Cores | 500-900 |
| Quantum Processors | 550-950 |

#### Premium Rare (600-1,500 shards/unit)
| Good | Base Price (shards/unit) |
|------|------------------------|
| Prototype Ship Components | 600-1,000 |
| Timeworn Relics (unknown function) | 600-1,200 |
| Psionic Amplifiers | 700-1,300 |
| Pre-Collapse Technology | 800-1,500 |

#### Elite Rare (900-4,000 shards/unit)
| Good | Base Price (shards/unit) |
|------|------------------------|
| Ancient Artifacts | 900-1,800 |
| Alien Relics | 1,000-2,000 |
| Rare Crystals & Exotic Matter | 1,200-2,500 |
| Sentient AI Fragments | 1,500-3,000 |
| Antimatter Containers | 2,000-4,000 |

---

## Ship Prices

### Three Ship Categories
| Category | Standard Tier | Premium Tier |
|----------|--------------|-------------|
| **Cargo** | Freighter | Hauler |
| **Speed/Agility** | Corvette | Clipper |
| **Military** | Gunship | Battleship |

### Standard Tier Ships (Sub-50k)
| Ship | Small | Medium | Large | Category |
|------|-------|--------|-------|----------|
| **Shuttle** | 500 | — | — | Utility |
| **Freighter** | 2,500 | 12,000 | 35,000 | Cargo |
| **Corvette** | 4,000 | 15,000 | 40,000 | Speed |
| **Gunship** | 6,000 | 18,000 | 48,000 | Military |

### Premium Tier Ships (50k+)
| Ship | Small | Medium | Large | Category |
|------|-------|--------|-------|----------|
| **Hauler** | 50,000 | 95,000 | 180,000 | Cargo (Premium) |
| **Clipper** | 60,000 | 110,000 | 200,000 | Speed (Premium) |
| **Battleship** | 85,000 | 150,000 | 300,000 | Military (Premium) |

### Black Market Variants
| Ship | Price | Category |
|------|-------|----------|
| BM Freighter | 120,000 | Cargo |
| BM Corvette | 135,000 | Speed |
| BM Gunship | 150,000 | Military |
| BM Hauler | 200,000 | Cargo (Premium) |
| BM Clipper | 400,000 | Speed (Premium) |
| BM Battleship | 1,000,000 | Military (Premium) |

### Progression Flow
- **Early game** (2,000-5,000 shards): Small standard ships, pick your path
- **Mid game** (10,000-48,000 shards): Medium/large standard ships, specializing
- **Late game** (50,000-300,000 shards): Premium ships, dominant in your role
- **End game** (120,000-1,000,000 shards): Black market elite ships

---

## Component Upgrade Costs

### Cost Per Component Per Tier (shards)
| Component | Tier 1 | Tier 2 | Tier 3 | Tier 4 | Tier 5 |
|-----------|--------|--------|--------|--------|--------|
| **Jump Drive** | 1,000 | 5,000 | 15,000 | 40,000 | 100,000 |
| **Weapons** | 600 | 2,500 | 8,000 | 22,000 | 55,000 |
| **Shields** | 500 | 2,000 | 6,000 | 18,000 | 45,000 |
| **Hull Plating** | 400 | 1,500 | 5,000 | 15,000 | 40,000 |
| **Engine** | 350 | 1,400 | 4,500 | 14,000 | 35,000 |
| **Cargo Bay** | 300 | 1,200 | 4,000 | 12,000 | 30,000 |
| **Scanner** | 250 | 800 | 2,500 | 8,000 | 20,000 |
| **Nav Computer** | 200 | 700 | 2,000 | 6,000 | 15,000 |

**Full Tier 5 loadout: ~340,000 shards** (roughly equal to a large Battleship)

### Upgrade Availability by Location
| Location | Tiers Available | Pricing |
|----------|----------------|---------|
| Sub-systems (industrial) | Tier 1-2 | Wholesale (cheaper) |
| Regular hubs | Tier 1-3 | Standard |
| Specialized hubs | Tier 1-5 (specialty components) | Standard |

---

## Jump Drive Tiers

| Tier | Max Manhattan Distance | Cost | Notes |
|------|----------------------|------|-------|
| 1 | 2 regions | Starter (free) | Starting safe zone + immediate neighbors |
| 2 | 5 regions | 5,000 | Nearby quadrant territory |
| 3 | 9 regions | 15,000 | Mid-game, most hub regions reachable |
| 4 | 14 regions | 40,000 | Deep space, far edges |
| 5 | Unlimited | 100,000 | Full galaxy access |

---

## Ship Repair Costs

### Standard Ships (Freighter/Corvette/Gunship)
| Repair Level | Hull % | Cost (shards) |
|-------------|--------|---------------|
| Minor | 75-99% | 100-300 |
| Moderate | 50-74% | 300-800 |
| Major | 25-49% | 800-2,000 |
| Critical | 1-24% | 2,000-5,000 |

### Premium Ships (Hauler/Clipper/Battleship)
| Repair Level | Hull % | Cost (shards) |
|-------------|--------|---------------|
| Minor | 75-99% | 500-1,500 |
| Moderate | 50-74% | 1,500-4,000 |
| Major | 25-49% | 4,000-10,000 |
| Critical | 1-24% | 10,000-25,000 |

### Repair Locations
- **Hub systems**: Full repair, all damage levels
- **Sub-systems**: Basic repair at **70% of hub cost**, can only repair up to 75% hull
- **Personal stations (Tier 2+)**: Basic repair capability

---

## Skill Progression Balance

### XP Curve (Per Skill, Levels 0-10)
| Level | XP Required for This Level | Total XP at This Level |
|-------|---------------------------|----------------------|
| 0 | — | 0 |
| 1 | 150 | 150 |
| 2 | 300 | 450 |
| 3 | 500 | 950 |
| 4 | 800 | 1,750 |
| 5 | 1,200 | 2,950 |
| 6 | 1,800 | 4,750 |
| 7 | 2,800 | 7,550 |
| 8 | 4,500 | 12,050 |
| 9 | 7,000 | 19,050 |
| 10 | 10,950 | 30,000 |

### XP Rewards Per Activity
| Activity | XP Reward | Skill |
|----------|-----------|-------|
| Small trade (< 500 profit) | 10-25 | Trading |
| Large trade (> 2,000 profit) | 50-100 | Trading |
| Successful haggle | 30-50 | Negotiation |
| Failed haggle | 10 | Negotiation |
| Successful bribe | 40-60 | Negotiation |
| NPC mission dialogue | 15-25 | Negotiation |
| Successful flee from combat | 25-40 | Piloting |
| Jump through unknown territory | 10-15 | Piloting |
| Win easy combat | 30-50 | Combat |
| Win medium combat | 80-120 | Combat |
| Win hard combat | 150-250 | Combat |
| Win boss combat | 400-600 | Combat |
| Discover new location | 40-60 | Exploration |
| Visit new system first time | 25-35 | Exploration |
| Repair ship | 20-40 | Engineering |
| Build/upgrade station | 100-200 | Engineering |
| Craft from blueprint | 80-150 | Engineering |

### Skill Impact Per Level (Flat Incremental)
| Skill | Bonus Per Level | At Level 5 | At Level 10 |
|-------|----------------|-----------|------------|
| **Trading** | +2% better buy/sell prices | +10% | +20% |
| **Piloting** | +3% evasion/flee chance | +15% | +30% |
| **Engineering** | +5% cheaper repairs/builds | +25% | +50% |
| **Negotiation** | +2% better mission rewards, +10% bribe success per level above NPC | +10% rewards | +20% rewards |
| **Combat** | +3% weapon damage, +2% accuracy | +15% dmg, +10% acc | +30% dmg, +20% acc |
| **Exploration** | +3% better discovery quality, +1 scanner detail tier | +15% quality | +30% quality |

---

## Fame System Balance

### Fame Earning
- 1 fame point earned every **5 combined skill levels** across all skills
- Maximum **12 fame points** (60 total skill levels / 5)

### Fame Point Allocation
- Each fame point gives **+3% boost** to a chosen skill's bonus
- Player can stack multiple points on one skill or spread across several
- Example: 4 fame points in Trading = +12% on top of skill level bonus

### Fame-Based Mission Slots
| Fame Level | Mission Slots |
|-----------|--------------|
| 0 | 3 |
| 1 | 3 |
| 2 | 4 |
| 3 | 4 |
| 4 | 5 |
| 5 | 5 |
| 6 | 6 |
| 7 | 7 |
| 8 | 7 |
| 9 | 8 |
| 10 | 9 |
| 11 | 9 |
| 12 | 10 |

### Hostile System Tax
- **Base tax: 15%** of each transaction value
- **-1% per fame level** (fame 12 = 3% tax)

---

## Combat Balance

### Damage Formula
```
damage = base_weapon_damage * (1 + combat_skill * 0.03) * (1 + fame_combat_bonus) - target_defense
```

### Combat Pacing
| Difficulty | Expected Turns |
|-----------|---------------|
| Easy | 2-3 turns |
| Medium | 4-6 turns |
| Hard | 6-10 turns |
| Boss (Warlord) | 8-15 turns with phases |

### Risk vs Reward
| Encounter Difficulty | Win Rate Target | Found In |
|---------------------|----------------|----------|
| Easy (Petty Pirate) | 90%+ | Travel, friendly fringe |
| Medium (Captain/Enforcer) | 60-80% | Hostile systems |
| Hard (Fleet/Bounty Hunter) | 30-50% | Deep hostile territory |
| Boss (Warlord) | 50% with prep | Hostile system boss |

### Enemy Loot Tables
| Enemy Type | Shard Loot | Item Loot | Notes |
|-----------|-----------|-----------|-------|
| Petty Pirate | 50-150 | Occasional common good | Easy fight, small reward |
| Gang Enforcer | 150-400 | Common goods, intel | Medium fight |
| Pirate Captain | 300-800 | Common + rare goods | Solid reward |
| Patrol Ship | 0 | None | Legitimate — no loot |
| Bounty Hunter | 500-1,200 | Equipment, rare goods | Hard fight, good pay |
| Pirate Fleet | 1,100-2,700 | Mixed goods, rare items | Risky, big haul |
| Alien Vessel | 700-4,000 | Unique rare items | Unpredictable |
| Warlord (Boss) | 4,000-11,000 | Rare goods, blueprints, intel | Major reward + system cleared |

**Loot does NOT scale with region distance — base values apply everywhere.**

### Enemy Fame Levels
```
enemy_fame = base_fame + floor(manhattan_distance / 2.5)
```

| Enemy | Base Fame |
|-------|----------|
| Petty Pirate | 1 |
| Gang Enforcer | 3 |
| Pirate Captain | 4 |
| Patrol Ship | 4 |
| Bounty Hunter | 5 |
| Pirate Fleet | 6 |
| Alien Vessel | 7 |
| Warlord | 8 |

### Intimidation
- **Player fame 3+ above enemy**: Combat skipped, enemy flees
- **Player fame equal to or up to 2 above**: Intimidate action available in combat
- **Intimidation success**: Enemy pays credits to player
- **Intimidation failure**: Enemy attacks with minor damage bonus

### Death Costs
- 0-50% cargo looted from destroyed ship (reduced by cargo bay tier)
- Time cost to recover ship (travel with loaner)
- Recovered ship is damaged, restricted to current region until repaired
- No permanent stat/skill loss

---

## Discovery Balance

### Locations Per Population Tier
| Population Tier | Total Discoveries | Min Services | Remaining |
|----------------|------------------|-------------|-----------|
| Major | 12-18 | 3+ (typically 6-10) | 6-12 |
| Moderate | 7-11 | 3+ (typically 4-6) | 3-7 |
| Minor | 3-6 | 3 | 0-3 |
| Measly | 3 | 3 | 0 |

### Discovery Danger Rates by Alignment
| Alignment | Chance of Dangerous Discovery | Notes |
|-----------|------------------------------|-------|
| Friendly | ~10-15% | Mostly safe |
| Neutral | ~15-20% | Minor hazards |
| Hostile | ~30-45% | Environmental dangers |
| Pirate-Controlled | ~40-60% | Combat encounters likely |
| Dead | ~25-35% | Environmental hazards |

### Discovery Reward Values
| Discovery Type | Reward | System Type |
|---------------|--------|-------------|
| Merchant / Trading Post | Access to trade | Friendly |
| Shipyard | Access to ships | Friendly |
| Resource Deposit | 200-800 shards of raw materials | Friendly, Dead |
| Quest Giver / NPC | Mission access | Friendly |
| Refueling Station | Access to fuel | Friendly |
| Black Market Contact | Access to deals | Friendly, Hostile |
| Salvage Wreckage | 300-1,200 shards of scrap/components | Dead |
| Ancient Ruins / Artifacts | 500-2,500 shards of artifacts | Dead |
| Derelict Ship | 400-1,500 shards + possible tow | Dead |
| Hidden Cache | 900-3,000 shards of mixed goods | Dead |
| Imprisoned Merchant | Rescued NPC, loyalty discount, reputation | Hostile |
| Stolen Cargo | 500-2,000 shards of goods | Hostile |
| Intel / Data Cache | System addresses, enemy info, story intel | Hostile |

---

## Travel Balance

### Travel Model
- **Inter-region jumps**: No pass-thru events. Only destination can trigger arrival events.
- **Intra-region travel**: Pass-thru events at intermediate systems, scaled by danger zone.

### Intra-Region Pass-Thru Risk
| Danger Zone (Manhattan Dist) | Negative Event Chance | Positive Event Chance |
|------------------------------|----------------------|----------------------|
| Safe (1-3) | Low | Low |
| Low-Medium (4-6) | Moderate | Moderate |
| Medium-High (7-9) | High | Moderate |
| Extreme (10+) | Very High | Moderate |

Events are also influenced by what exists at the pass-thru system (e.g., Pirate Turf War Zone, Distress Signal).

---

## Stock Market Tuning

### Price Fluctuation Parameters
| Parameter | Value | Notes |
|-----------|-------|-------|
| Random fluctuation per visit | +/- 8% | Base variance on each return |
| Additional fluctuation per 10 star dates away | +/- 3% | Longer absence = more drift |
| Max price drift from baseline | +/- 40% | Hard cap on price swing |
| Player buy impact | +2% per 50 units bought | Buying drives price up |
| Player sell impact | -2% per 50 units sold | Selling drives price down |
| **Player trade impact cap** | **16% max** | Prevents market destruction |
| Price recovery rate | 1% per 5 star dates | Slowly returns toward baseline |
| Event spike magnitude | +50% to +200% | Wars, shortages, booms |
| Event duration | 30-80 star dates | How long before event fades |

### Market Health Targets
- Average profit per trade run: **positive but not trivial**
- Best legitimate routes: **30-80% profit margins**
- Black market deals (when successful): **100-300% profit margins**
- Random fluctuation creates occasional **windfall** and **loss** scenarios
- Player rewarded for tracking trends and timing trades

---

## Mission Reward Scaling

### Base Rewards
| Mission Type | Base Reward (shards) | Per Manhattan Distance | Per Fame Level |
|-------------|---------------------|--------------------:|---------------:|
| Delivery | 200-500 | +2% | +3% |
| Smuggling | 500-1,200 | +4% | +3% |
| Bounty | 400-1,000 | +4% | +5% |
| Rescue | 300-800 | +2% | +5% |
| Exploration | 250-600 | +2% | +3% |
| Clearance | 2,000-5,000 | +5% | +8% |
| Investigation | 300-700 | +2% | +3% |

### Example Calculation
Clearance mission, Manhattan distance 14, fame 8:
- Base: 3,500 shards
- Distance bonus: 14 × 5% = +70% → +2,450
- Fame bonus: 8 × 8% = +64% → +2,240
- **Total: ~8,190 shards**

---

## Hostile System Clearing

### Outposts Required (Scales with Manhattan Distance)
| Distance Range | Outposts to Clear |
|---------------|-------------------|
| Distance 1-5 (safe/inner) | 2 outposts |
| Distance 6-10 (mid) | 3 outposts |
| Distance 11-15 (far) | 4 outposts |
| Distance 16+ (edge) | 5 outposts |

### Boss Mechanics
- Boss appears on next turn after clearing required outposts
- **2-3 turn warning** before attack
- Player has minimum 1 turn to leave and prepare
- Boss attacks when player returns
- Boss rebuilds outposts if player delays too long

---

## Difficulty Design

### No Difficulty Settings
The game has **no Easy/Normal/Hard modes**. Difficulty is determined entirely by:
- **Where the player explores** — higher Manhattan distance from origin = harder
- **Which pirate faction quadrant** they enter (Q01 easiest → Q04 hardest)
- **Whether they venture into edge regions** (distance 10+, no hubs, no respawn points)

### Pirate Faction Difficulty Ranking
| Quadrant | Faction | Difficulty | Combat Style |
|----------|---------|-----------|-------------|
| Q01 | **Silk Hand** | Easiest | Prefer bribery, flee when outmatched |
| Q02 | **Phantom Circuit** | Easy-Medium | Evasive, trade intel to avoid combat |
| Q03 | **Rust Collective** | Medium-Hard | Tanky, want your cargo, won't flee |
| Q04 | **Void Reavers** | Hardest | Most aggressive, high damage, rarely flee |

### Opening NPC Warning
At the start of the game (after tutorial), an NPC at the hub warns the player about the galaxy's danger zones:
- Which quadrants contain which pirate factions (Q01=Silk Hand easiest → Q04=Void Reavers hardest)
- General difficulty of each quadrant
- Warning about edge regions and dead space border
- Recommendation to stay near the center safe zones until stronger

### Regional Difficulty Scaling (Manhattan Distance from Origin)
- **Safe zone (distance 1-3)**: Space Force controlled, easy enemies, stable economy
- **Inner regions (distance 4-6)**: Moderate, first hostile systems, pirate activity begins
- **Mid regions (distance 7-9)**: Deep pirate territory, tougher enemies, faction border turf wars
- **Edge regions (distance 10+)**: Strongest enemies, best loot, pirate strongholds, no hubs
- **Dead space border (outermost 2 cells)**: Accessible but empty, no content

---

## Progression Targets

### Milestone Timeline
| Milestone | Approx. Turn Range | Shards | Ship | Jump Drive |
|-----------|-------------------|--------|------|------------|
| Tutorial complete | Turn 5-10 | 2,000 | Small Freighter (awarded) | Tier 1 |
| First profitable trade | Turn 10-15 | 2,500-3,500 | Small Freighter | Tier 1 |
| First component upgrade | Turn 15-25 | 3,000-5,000 | Small Freighter + upgrades | Tier 1 |
| First new ship purchase | Turn 30-50 | 8,000-15,000 | Medium standard ship | Tier 2 |
| First hostile system cleared | Turn 40-70 | 10,000-20,000 | Upgraded standard ship | Tier 2 |
| New region accessible | Turn 50-80 | 15,000-30,000 | Large standard ship | Tier 3 |
| First premium ship | Turn 100-150 | 50,000-85,000 | Small premium ship | Tier 3-4 |
| Mid-game wealth | Turn 150-250 | 100,000-200,000 | Medium premium ship | Tier 4 |
| End-game wealth | Turn 300+ | 300,000+ | Large premium / BM ship | Tier 5 |
| Ultimate goal | Turn 500+ | 1,000,000 | BM Battleship | Tier 5 |

---

## Playtesting Metrics to Track
- Average shards earned per turn
- Player death frequency and primary causes
- Most/least traded commodities
- Average turns to reach each progression milestone
- Most common trade routes
- Percentage of systems explored vs. available
- Hostile systems cleared per playthrough
- Average game length (turns to story completion)
- Skill level distribution at each milestone
- Fame level progression rate
- Black market deal success rate
- Which pirate quadrant players explore first (Q01-Q04)

---

## Notes
_Add design notes, open questions, and decisions here._

### Open Design Questions
- All values need playtesting to validate
- Exact combat damage formulas and defense calculations
- Boss encounter phases and special abilities
- Outpost rebuild rate when player delays boss fight
- Exact loot tables (item drops vs. shard drops)
- How cargo bay tier affects death loot percentage specifically
- Intimidation success rate formula
- Negotiation haggle success rate formula
- Battleship component tier caps (see ship doc)
