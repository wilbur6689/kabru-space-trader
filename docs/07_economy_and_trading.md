# Economy and Trading

## Overview
This document defines the economic model, trade goods, pricing mechanics, and how the player makes money. The economy operates on a **dynamic stock market model** where prices fluctuate based on supply, demand, and world events. The core gameplay loop revolves around buying and selling goods — all other systems support this.

---

## Currency

- **Name:** Shards
- **Universal:** Single currency used across all systems (legitimate and black market)
- **Starting Amount:** 2,000 shards (earned after completing the tutorial delivery mission)

---

## Trade Goods

### Good Types
All goods fall into one of three rarity tiers:

| Type | Description | Availability |
|------|-------------|-------------|
| **Common** | Found everywhere throughout the galaxy. Used for trading and common blueprint ingredients. | Every system stocks some common goods |
| **Rare** | High value, specific to 2-4 bordering regions. More valuable the farther from source. Used in expensive blueprint crafting. | Region-locked, requires travel to find |
| **Black Market** | Stolen/smuggled versions of common or rare goods not normally stocked at the current system. Purchased through shady dealers. | Hidden contacts, gamble on every deal |

### Common Goods (22)
| Good | Weight Range | Notes |
|------|-------------|-------|
| Food & Rations | 12-15 | High demand everywhere |
| Water & Ice | 12-15 | Essential, bulk commodity |
| Medical Supplies | 7-10 | Universal demand, premium in remote systems |
| Raw Ores & Minerals | 10-13 | Produced by mining colonies |
| Refined Metals | 9-12 | Processed from raw ores |
| Industrial Materials | 8-11 | Manufacturing input |
| Manufactured Components | 8-11 | Produced by industrial colonies |
| Consumer Goods | 7-10 | General demand |
| Basic Electronics | 6-9 | Moderate availability |
| Fuel (Hydrogen, Helium-3, etc.) | 11-14 | Always needed |
| Energy Cells & Batteries | 9-12 | Power supply staple |
| Construction Materials | 9-12 | Building and repair |
| Agricultural Supplies | 10-13 | Farming system specialty |
| Textiles & Fibers | 8-11 | Light trade good |
| Mechanical Parts | 7-10 | Ship and station repair |
| Ship Maintenance Supplies | 6-9 | Always in demand |
| Recycled Scrap | 4-7 | Variable, found in dead systems |
| Chemical Compounds | 6-9 | Industrial and medical use |
| Atmospheric Gases | 5-8 | Specialized demand |
| Terraforming Supplies | 5-8 | Niche, high value at frontier systems |
| Data Storage Devices | 5-8 | Tech-focused systems |
| Civilian Tools & Equipment | 7-10 | General utility |

### Rare Goods (22)
| Good | Weight Range | Notes |
|------|-------------|-------|
| Luxury Items | 3-6 | Demand varies wildly |
| Weapons & Ammunition | 5-8 | Restricted in some systems |
| Ancient Artifacts | 1-3 | Extremely rare, dead system finds |
| Alien Relics | 1-3 | Extremely rare, unique |
| Experimental Technology | 2-4 | Cutting edge, very scarce |
| Black Market Cybernetics | 2-4 | Illegal enhancements |
| Prototype Ship Components | 2-5 | Used in rare blueprints |
| High-End AI Cores | 3-5 | Tech hub specialty |
| Quantum Processors | 2-4 | Advanced computing |
| Stellar Cartography Data | 3-6 | Exploration value |
| Genetic Enhancements | 2-4 | Medical/science hubs |
| Designer Drugs & Stimulants | 2-4 | High profit, risky |
| Cultural Treasures (Art, Sculptures) | 3-6 | Luxury trade |
| Rare Crystals & Exotic Matter | 1-3 | Extremely rare |
| Antimatter Containers | 1-2 | Dangerous, very high value |
| Pre-Collapse Technology | 1-3 | Ancient tech, lore value |
| Forbidden Research Data | 2-4 | Controversial, high value |
| Smuggled Goods | 3-5 | Generic contraband |
| Sentient AI Fragments | 1-2 | Rarest tier, unique |
| Psionic Amplifiers | 1-2 | Mysterious, lore-tied |
| Alien Flora & Fauna Specimens | 2-4 | Science hub demand |
| Timeworn Relics (unknown function) | 1-3 | Mystery items, potential lore |

### Rare Goods Regional Distribution
- Each rare good is tied to **2-4 bordering regions** where it can be found
- Rare goods are **more valuable** the farther you travel from regions that sell them
- Used in **rare blueprint crafting** — a blueprint might require 3-5 common goods + 1-2 rare goods
- Creates incentive for long-distance trade runs across the galaxy

### Good Properties
- **Base price** — Starting price before modifiers
- **Weight (1-15)** — Determines stock quantity when generated (see Stock Generation)
- **Rarity tier** — Common, Rare, or Black Market
- **Legality status** — Legal or restricted (some rare goods)
- **Source system types** — Where it's produced/found cheaply (weight is higher at these systems)
- **Demand system types** — Where it sells for a premium

---

## Stock Generation System

### Weight-Based Stock Ranges
Every good has an importance weight from 1-15. When a system's economy is first generated, stock quantities are determined by the weight using these ranges:

| Weight | Stock Range | Typical Goods |
|--------|-------------|---------------|
| 1 | 5 - 12 | Sentient AI Fragments, Antimatter |
| 2 | 8 - 18 | Psionic Amplifiers, Exotic Matter |
| 3 | 10 - 22 | Ancient Artifacts, Alien Relics |
| 4 | 15 - 30 | Experimental Tech, Cybernetics |
| 5 | 22 - 45 | Prototype Components, Weapons |
| 6 | 35 - 70 | Luxury Items, Atmospheric Gases |
| 7 | 50 - 110 | Medical Supplies, Mechanical Parts |
| 8 | 80 - 180 | Manufactured Components, Electronics |
| 9 | 130 - 300 | Refined Metals, Energy Cells |
| 10 | 220 - 500 | Raw Ores, Agricultural Supplies |
| 11 | 400 - 850 | Construction Materials, Textiles |
| 12 | 700 - 1,400 | Food, Water, Fuel |
| 13 | 1,200 - 2,200 | Bulk staples at major producers |
| 14 | 2,000 - 3,500 | High-production specialty systems |
| 15 | 3,000 - 5,000 | Maximum stock, major production hubs |

### How Stock Generation Works
1. Player arrives at a new system → economy is **generated for the first time**
2. System selects **30-50% of all available goods** to stock
3. Selection is **weighted by system type** — a mining region has higher chance of stocking raw materials with higher weight ranges; a medical system favors medical supplies
4. Each selected good gets stock quantity based on its weight using the range table
5. **Randomness within range** — stock is randomly chosen within the min/max for natural variation
6. **Optional modifiers** applied: station size, economy level, system type can shift the range
7. Values **rounded to clean, realistic numbers**
8. Adjacent weights have **overlapping ranges** so the economy feels natural and less predictable

### System Type Weight Modifiers
System type shifts the weight range for relevant goods:

| System Type | Boosted Goods | Weight Shift |
|-------------|--------------|-------------|
| Mining Colony | Raw Ores, Minerals, Refined Metals | +3 to +5 weight |
| Industrial Colony | Manufactured Components, Mechanical Parts | +3 to +5 weight |
| Agricultural Settlement | Food, Water, Agricultural Supplies, Textiles | +3 to +5 weight |
| Tech Hub | Electronics, AI Cores, Data Devices | +3 to +5 weight |
| Medical System | Medical Supplies, Genetic Enhancements | +3 to +5 weight |
| Military Hub | Weapons, Fuel, Ship Maintenance | +3 to +5 weight |

---

## Stock Market Pricing Model

### Per-Visit Price Calculation
Prices are **not simulated in real-time**. Instead:
1. When the player **leaves a system**, the current **star date is recorded**
2. When the player **returns**, the game compares current star date with departure date
3. Prices are **fluctuated accordingly** based on elapsed time
4. First visit to a system generates the economy from scratch

### Price Formula
```
final_price = base_price
    * system_type_modifier      # Producers sell cheap, consumers buy expensive
    * economy_level_modifier    # Wealthy systems have higher prices overall
    * supply_demand_modifier    # Dynamic based on trade volume and stock levels
    * time_fluctuation          # Based on star date difference since last visit
    * event_modifier            # Wars, shortages, booms, system hostility changes
    * fame_modifier             # Player fame + highest skill alignment
```

### Price Modifiers
| Factor | Effect | Example |
|--------|--------|---------|
| System produces good | -30% to -60% | Ore is cheap at mining colonies |
| System demands good | +30% to +80% | Ore is expensive at industrial colonies |
| Economy level (poor) | -20% overall | Everything cheaper but less variety |
| Economy level (booming) | +30% overall | Everything more expensive |
| Recent player purchase | +5% to +15% locally | Buying drives price up |
| Recent player sale | -5% to -15% locally | Selling drives price down |
| War/conflict event | +50% to +200% weapons, medicine | Scarcity pricing |
| System cleared of hostiles | Prices stabilize | New trade route opens |
| High player fame (aligned skill) | -5% to -15% | Better prices in matching systems |

### Market Behavior
- Each commodity has its own price trend per system
- Trends can be upward, downward, stable, or volatile
- Major events cause **price shocks** — sudden spikes or crashes
- Player trading affects local supply/demand (buying drives price up, selling drives it down)
- Prices **gradually recover** toward baseline over time (turns)

---

## Supply and Demand

### Dynamic Market
- Buying and selling **affects local prices** — large trades move the market
- Prices gradually recover toward baseline based on elapsed star dates
- NPC traders also affect the market (background simulation)
- Each system has **independent** supply/demand tracking

### Wholesale Pricing at Sub-Systems
- Sub-systems sell goods at a **discount that varies by current stock level**
- **High stock** = big discount (up to 30-40% below hub prices)
- **Low stock** = discount shrinks toward normal pricing
- Creates a first-come advantage — early buyers get the best deals

### Stock Replenishment
- **Time-based**: Stock slowly refills over time (calculated on return visit using star date difference)
- **Trade-accelerated**: Selling goods the sub-system **demands** speeds up production of goods they **produce**
- Example: Bring food to a mining colony → they produce more ore faster
- Creates a **symbiotic trading loop** between players and sub-systems

---

## Bartering System

### How Haggling Works
- Player gets **1 haggle attempt per merchant** per visit
- Player initiates an **active haggle** on a specific transaction
- Success is based on **Negotiation skill level** vs. merchant resistance
- **Success**: Player gets a discount on the transaction
- **Failure**: Merchant **raises the price** on that item for this visit

### Haggle Mechanics
- Each merchant has a hidden resistance value
- Higher negotiation skill = better chance of success
- Player must choose wisely which transaction to haggle on — only one shot
- Earns **Negotiation XP** on successful haggles

---

## Bribe System

### When Bribes Occur
- Bribes can be attempted when caught by authorities (e.g., undercover security in black market deals, or other legal violations)

### Bribe Mechanics
- Each NPC has a **hidden tipping percentage** (unknown to the player)
- **Negotiation skill bonus**: Every skill level the player has **above** the NPC adds **+10%** to success chance
- **Shard bonus**: Every **10 shards** offered adds **+1%** to success chance
- Player must decide how many shards to offer without knowing the exact threshold
- **Success**: Authorities look the other way
- **Failure**: Player is taken to the **regional hub** and hit with a **major fine** by the court

---

## Black Market

### How Black Market Goods Work
- Black market goods are **common or rare goods not normally available** at the current system
- They are **not marked** as "black market" in inventory — they look like normal goods once purchased
- The risk is in the **transaction itself**, not in carrying the goods afterward
- Black market goods fill gaps in a system's inventory — if refined metals aren't stocked locally, a dealer might offer them

### Finding Black Market Dealers
- **Discoverable location**: Some dealers found through searching systems (not all are findable this way)
- **NPC referral**: Complete side missions for NPCs and they tip you off about dealers
- **Pirate faction fame**: Higher pirate fame unlocks more black market contacts
- Dealers are part of the underground network — not openly available

### Black Market Transaction Flow
1. **Find a dealer** through search, NPC referral, or pirate contacts
2. **Negotiate terms** — agree on goods and price (goods are cheaper than legitimate sources)
3. **Delivery notification** — player receives a message that delivery has arrived
4. **Open delivery** — one of 5 outcomes:

| Outcome | Chance | Result |
|---------|--------|--------|
| **Undercover Security** | 5% | Dealer was undercover. Player fined **3x the negotiated deal price**. |
| **Shortage** | 25% | Received **less** than what was negotiated |
| **Exact Delivery** | 50% | Received **exactly** what was negotiated |
| **Different Product** | 15% | Received a **different product of equal value** |
| **Rare Bonus** | 5% | Received a **different rare product of much greater value** |

### Black Market Balance
- **Negotiation skill has no effect** on delivery outcomes — shady dealers don't care who you are
- Percentages are **fixed** — every deal is a gamble regardless of player power
- The discount on purchase price must be enough that the 50% exact delivery rate still makes it profitable on average
- The 5% rare bonus creates exciting moments that keep players coming back

---

## Market Information

### At Current System
- Full price visibility for all stocked goods (free)
- Stock quantities visible

### Hub Bulletin Boards
- Known prices at neighboring systems (may be slightly outdated)
- Major hubs **always communicate** day-to-day pricing with each other — hub prices are reliable
- Sub-system prices from bulletin boards may be delayed

### NPC Tips
- NPCs share price information from systems they've visited
- Example dialogue: *"Oh yeah, I was just at system 11-321 and here are the prices I saw last time I was there."*
- Provides updated price info for sub-systems that bulletin boards may not have

### Player Journal
- Tracks **last known current prices** at every visited system
- **Regional goods overview** — shows what goods are more common in a specific region
- Hints at what goods are in **high supply** (cheap) and what goods are in **demand** (expensive)
- Helps players plan trade routes at a macro level
- Price history tracking deferred for later (see deferred design doc)

---

## Trade Routes

### How Routes Work
- A trade route is any **repeatable path** between systems where goods can be bought low and sold high
- Players discover routes organically through exploration and information gathering
- The best routes span between **producing sub-systems** and **consuming hub systems**
- Wholesale sub-system pricing creates natural buy points
- Hub demand creates natural sell points

### Route Profitability Factors
- Distance (more jumps = more travel risk but potentially more profit)
- System types at each end (mining → industrial is a natural route)
- Market conditions (dynamic — a good route today may not be tomorrow)
- Route safety (known routes are safer, unknown routes carry encounter risk)
- Rare goods routes (region-locked goods sold far from source = highest legitimate profit)

### Route Discovery Methods
- Personal exploration (try addresses, track what you find)
- Hub bulletin boards (list known trade connections and prices)
- NPC tips (specific system prices and goods availability)
- Purchased star charts (batch address + economic data)
- Mission destinations (delivery missions reveal new trade endpoints)
- Journal regional goods overview (macro-level supply/demand hints)

---

## Income Sources

### Primary
- **Commodity trading** — Buy low at producers/wholesalers, sell high at consumers
- **Mission rewards** — Shards from completed deliveries, bounties, and quests

### Secondary
- **Salvage** — Collected from dead systems, derelict ships, and combat victories
- **Bounty hunting** — Rewards for eliminating specific targets
- **Artifact sales** — Rare items from dead systems and hostile system caches
- **Intel sales** — Selling discovered system data or enemy intel
- **Hostile system clearance rewards** — Major payout for liberating a system
- **Intimidation credits** — Shards paid by intimidated enemies who flee
- **Black market dealing** — High-risk purchases at steep discounts

### Passive / Indirect
- Better faction reputation = better prices at allied merchants
- Higher trading skill = better buy/sell margins
- Fame-aligned pricing bonuses
- Knowledge of price trends and regional goods = smarter trading decisions
- Wholesale relationships with sub-systems

---

## Expenses

### Recurring
- Ship repairs (after combat, hazards, dangerous discoveries)
- Hostile system taxes for conducting business
- Failed haggle price increases
- Black market deal losses (undercover busts, shortages)

### One-Time / As-Needed
- Ship upgrades (especially jump drive tiers)
- New ship purchases (at hub shipyards)
- Ship storage and transfer fees between hubs
- Star chart purchases
- NPC information / bribes
- Station building and upgrades
- Blueprint crafting resource costs
- Court fines from failed bribes

### Death Costs
- Cargo loss (0-50% looted from destroyed ship)
- Time cost to recover ship or buy a new one
- Potential loss of trade position (prices shift while recovering)
- Ship towing/recovery fees for derelicts

---

## Economic Impact of World Events

### System Hostility Changes
- Clearing a hostile system **opens new trade routes** and stabilizes prices in the region
- A friendly system going hostile **disrupts trade** and spikes prices for scarce goods
- Neighboring system changes ripple through regional economies

### Faction Events
- Wars between factions increase weapon and medicine prices
- Peace treaties stabilize markets and reduce patrol encounters
- Blockades cut off supply lines to specific systems
- Pirate faction turf wars in border regions affect local economies

### Regional Goods Shifts
- As the player clears hostile systems and opens trade routes, **regional goods availability can shift**
- New sub-systems becoming friendly may introduce new goods to a region's economy
- Player station placement can create new trade nodes

---

## Notes
_Add design notes, open questions, and decisions here._

### Open Design Questions
- Exact base prices per good in shards
- Price recovery rate formula (how fast prices return to baseline over star dates)
- NPC trader background simulation complexity
- Time fluctuation formula (how much prices shift per elapsed star date)
- Exact wholesale discount curve (stock level → discount percentage)
- Stock replenishment rate (time-based and trade-accelerated formulas)
- System type weight modifier exact values
- Rare goods regional assignment (which rare goods go in which 2-4 regions)
- Black market dealer generation rules (how many per system, refresh rate)
- Bribe base success percentage before skill/shard modifiers
- Court fine amounts for failed bribes
- Journal price history tracking (deferred)
