# Player Stations

## Overview
Players can build personal stations at empty space addresses by purchasing construction from a **Station Construction Service** discovery. Stations provide storage, repairs, crafting, and can serve as respawn points in dangerous territory. They are a significant investment that scales with distance and tier.

---

## Station Basics
- Stations are built at **any empty address** — player chooses the location
- Maximum **2 stations per quadrant**, **12 total** across the galaxy
- Stations persist permanently at their address
- **Unlimited storage capacity** — limited only by what the player can carry there in their ship's cargo bay

---

## Building a Station

### Station Construction Service
- A **service discovery** found at systems throughout the galaxy
- Player purchases station construction, selects an empty address as the destination
- Station is **delivered after a delay** — not instant

### Distance-Based Pricing and Delivery
- **Cost and delivery time scale with Manhattan distance** between the construction service and the chosen empty address
- Building near the construction service = cheaper and faster
- Building deep in wild space = expensive and slow
- Strategic consideration: find construction services close to where you want your station

### Base Costs
| Action | Base Cost | Notes |
|--------|----------|-------|
| Tier 1 construction | 25,000 shards | Storage only |
| Tier 2 upgrade | 50,000 shards | Adds basic repair |
| Tier 3 upgrade | 100,000 shards | Adds blueprint crafting |
| **Full Tier 3 total** | **175,000 shards** | Major late-game investment |

Costs are modified by distance from the construction service to the station address.

---

## Station Tiers

| Tier | Services | Raid Risk | Respawn |
|------|----------|-----------|---------|
| **Tier 1** | Storage only — drop off and pick up cargo | None — never raided | No |
| **Tier 2** | Storage + basic repair | Passive raids — small losses | No |
| **Tier 3** | Storage + basic repair + blueprint crafting | Active raids — defend or lose more | **Yes — counts as respawn point** |

### Tier 1 — Storage
- Drop off and pick up cargo at any time
- Unlimited storage capacity
- Completely safe — no pirate attention
- Strategic use: stash goods to sell later when prices are right

### Tier 2 — Storage + Repair
- Everything from Tier 1
- Basic ship repair capability
- Starts attracting minor pirate attention (passive raids)

### Tier 3 — Full Station
- Everything from Tier 2
- **Blueprint crafting** — use blueprints found in exploration + raw materials to create unique upgrades not available at any system
- **Respawn point** — player respawns here if it's their last visited station. Critical for wild space exploration.
- Attracts active pirate raids — must be defended

---

## Pirate Raids

### Raid Scaling by Tier
| Tier | Raid Type | How It Works |
|------|-----------|-------------|
| Tier 1 | None | Station is invisible to pirates |
| Tier 2 | **Passive raids** | Player receives an inbox message that a raid occurred. Small percentage of stored goods lost. No action required. |
| Tier 3 | **Active raids** | Player receives a warning message with a set number of turns to travel and defend. If defended: no losses. If ignored: larger percentage of goods lost + **station downgraded to Tier 2**. |

### Raid Frequency
- Scaled by the **danger zone** of the station's region (Manhattan distance from origin)
- Stations in safe zones (distance 1-3): Very rare raids
- Stations in mid regions (distance 4-6): Occasional raids
- Stations in far/edge regions (distance 7+): Frequent raids
- Pirate faction territory increases raid frequency further

### Failed Defense / Ignored Raids
- Stations **cannot be destroyed** — only downgraded
- A failed Tier 3 active raid **downgrades the station to Tier 2**
- Player must pay the Tier 3 upgrade cost again to restore it
- Tier 2 passive raids only lose goods — never downgrade to Tier 1

---

## Strategic Value
- **Trade route optimization**: Stash goods at stations near profitable systems, sell when prices are right
- **Wild space foothold**: Tier 3 stations provide repair, crafting, and respawn in regions with no Friendly Major systems
- **Blueprint crafting**: The only way to create unique upgrades from exploration blueprints
- **Risk management**: Place Tier 1 stations in dangerous areas for safe storage, reserve Tier 3 for areas you can defend

---

## Open Design Questions
- Exact distance multiplier formula for construction cost and delivery time
- Passive raid loss percentage (Tier 2) — flat % or scaled by goods value?
- Active raid warning window — how many turns does the player have to respond?
- Active raid combat mechanics — is it a standard combat encounter or unique?
- Active raid loss percentage when ignored vs. when defense fails
- Blueprint crafting system expansion — additional crafting recipes and capabilities (deferred)
- What blueprints exist and what unique upgrades they produce (deferred)
- Whether stations can be voluntarily decommissioned to free up a slot
- Whether station location is visible to other players in future multiplayer
