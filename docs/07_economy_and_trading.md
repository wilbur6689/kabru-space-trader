# Economy and Trading

## Overview
This document defines the economic model, trade goods, pricing mechanics, and how the player makes money. The economy operates on a **dynamic stock market model** where prices fluctuate based on supply, demand, and world events.

---

## Trade Goods

### Commodity Categories
| Category | Examples | Base Price Range | Notes |
|----------|----------|-----------------|-------|
| Food | Grain, Livestock, Rations | Low | High demand everywhere, produced by agricultural systems |
| Raw Materials | Ore, Timber, Water | Low-Medium | Produced by mining colonies, needed by industrial systems |
| Manufactured | Machinery, Alloys, Tools | Medium | Produced by industrial colonies |
| Technology | Electronics, Software, AI Cores | High | Produced only by tech hubs in hub systems |
| Medicine | Med Kits, Vaccines, Bionics | Medium-High | Universal demand, premium in remote systems |
| Luxury | Jewels, Art, Spirits, Spice | High | Demand varies wildly by system |
| Weapons | Arms, Munitions, Explosives | High | Restricted in some friendly systems, common in hostile |
| Contraband | Narcotics, Stolen Tech, Slaves | Very High | Illegal in friendly systems, sold at black markets |
| Salvage | Scrap Metal, Relics, Components | Variable | Found in dead systems, sold at industrial/hub systems |
| Artifacts | Ancient Tech, Alien Relics | Very High | Rare discoveries in dead/hostile systems |

### Commodity Properties
- Base price
- Weight / volume per unit (affects cargo capacity)
- Legality status (legal, restricted, contraband)
- Rarity (common, uncommon, rare, very rare)
- Source system types (where it's produced/found cheaply)
- Demand system types (where it sells for a premium)

---

## Stock Market Pricing Model

### Price Calculation
```
final_price = base_price
    * system_type_modifier      # Producers sell cheap, consumers buy expensive
    * economy_level_modifier    # Wealthy systems have higher prices overall
    * supply_demand_modifier    # Dynamic based on trade volume and events
    * market_fluctuation        # Random daily variance (stock market behavior)
    * event_modifier            # Wars, shortages, booms, system hostility changes
```

### Market Behavior
- Prices **fluctuate like a stock market** — rising and falling over time
- Each commodity has its own price trend per system
- Trends can be upward, downward, stable, or volatile
- Major events cause **price shocks** — sudden spikes or crashes
- Player trading affects local supply/demand (buying drives price up, selling drives it down)

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

---

## Supply and Demand

### Dynamic Market
- Buying and selling **affects local prices** — large trades move the market
- Prices **gradually recover** toward baseline over time (turns)
- NPC traders also affect the market (background simulation)
- Each system has **independent** supply/demand tracking

### Market Information
- **At current system**: Full price visibility (free)
- **At hub bulletin boards**: Known prices at neighboring systems (may be delayed/outdated)
- **From NPCs/traders**: Tips about price trends ("Ore prices are spiking at system 284510")
- **Purchased star charts**: Include basic economic data for revealed systems
- **Player's journal**: Tracks price history for visited systems — helps identify profitable routes

---

## Trade Routes

### How Routes Work
- A trade route is any **repeatable path** between systems where goods can be bought low and sold high
- Players discover routes organically through exploration and information gathering
- The best routes span between **producing sub-systems** and **consuming hub systems**

### Route Profitability Factors
- Distance (more jumps = more travel risk but potentially more profit)
- System types at each end (mining → industrial is a natural route)
- Market conditions (dynamic — a good route today may not be tomorrow)
- Route safety (known routes are safer, unknown routes carry encounter risk)
- Contraband routes (black market → black market) offer highest profit but highest risk

### Route Discovery Methods
- Personal exploration (try addresses, track what you find)
- Hub bulletin boards (list known trade connections)
- NPC tips ("System 482103 has cheap ore, system 193847 pays double")
- Purchased star charts (batch address + economic data)
- Mission destinations (delivery missions reveal new trade endpoints)

---

## Income Sources

### Primary
- **Commodity trading** — Buy low at producers, sell high at consumers
- **Mission rewards** — Credits from completed deliveries, bounties, and quests

### Secondary
- **Salvage** — Collected from dead systems, derelict ships, and combat victories
- **Bounty hunting** — Rewards for eliminating specific targets
- **Artifact sales** — Rare items from dead systems and hostile system caches
- **Intel sales** — Selling discovered system data or enemy intel
- **Hostile system clearance rewards** — Major payout for liberating a system

### Passive / Indirect
- Better reputation = better prices at allied merchants
- Trading skill improvements = better buy/sell margins
- Knowledge of price trends = smarter trading decisions

---

## Expenses

### Recurring
- Ship repairs (after combat, hazards, dangerous discoveries)
- Ship component maintenance
- Contraband fines (if caught by patrols)

### One-Time / As-Needed
- Ship upgrades (especially jump drive)
- New ship purchases (at hub shipyards)
- Star chart purchases
- NPC information / bribes
- Equipment and supplies

### Death Costs
- Cargo loss (0-50% looted from destroyed ship)
- Time cost to recover ship or buy a new one
- Potential loss of trade position (prices may shift while recovering)

---

## Black Market

### How It Works
- Black markets are **discoverable locations** in hostile systems and some friendly systems
- Trade in contraband and stolen goods
- Higher prices for rare and illegal items
- No legality restrictions — anything can be bought or sold
- Risk: black market locations may be traps or attract bounty hunters

### Contraband Mechanics
- Contraband goods are flagged as illegal in friendly systems
- Patrol ships at friendly systems may **scan cargo** and levy fines
- Black markets in hostile systems have no patrol risk but other dangers
- High risk/reward — contraband profit margins are the largest in the game

---

## Currency
- [ ] Currency name (Credits, Marks, Units, etc.)
- [ ] Single universal currency across all systems
- [ ] Starting amount (enough for basic repairs + first trade in tutorial)
- [ ] Wealth milestones that unlock gameplay options (afford upgrades, new ships, etc.)

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

---

## Notes
_Add design notes, open questions, and decisions here._

### Open Design Questions
- Exact base prices per commodity
- Price recovery rate after player trades
- NPC trader simulation complexity
- How many price data points are tracked in player journal history
- Currency name
- Starting credit amount
