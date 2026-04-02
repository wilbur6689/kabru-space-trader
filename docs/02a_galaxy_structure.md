# Galaxy Structure

## Overview
This document defines the overall galaxy layout, regional hierarchy, address system, and the starting region. The galaxy is arranged in a spiral pattern with danger increasing outward from the origin.

---

## Hierarchy
```
Galaxy (Spiral layout from origin)
└── Regions (15-20 total, numbered 00-99)
    ├── Hub Regions (~50% of regions) — contain a hub cluster
    │   ├── Hub System (1 per cluster, specialized)
    │   ├── Sub-Systems (3-5 per cluster, tied to hub)
    │   ├── Unaffiliated Systems (scattered, hidden)
    │   └── Dead Systems (scattered)
    └── Wild Space Regions (~50% of regions) — no hub
        ├── Unaffiliated Systems (scattered, hidden)
        └── Dead Systems (scattered)
```

---

## Region Address Format
- Every system has an address in the format **XX-XXXX** (e.g., 38-0322)
- **First 2 digits**: Region code (00-99)
- **Last 4 digits**: System address within that region (0000-9999)
- Most addresses within a region are **empty space**
- A moderate number of addresses (20-50) are populated per region
- 3-5 of those are "known" sub-systems tied to the hub

---

## Spiral Layout
- The galaxy is arranged in a **spiral pattern** radiating outward from the origin (Region 00)
- Region numbers generally **increase with distance** from the origin
- Regions that are **close on the spiral** form natural clusters, but aren't necessarily sequential (e.g., regions 3-5, 12-16, and 34-37 might each be nearby clusters on different arms)
- **Lower numbers** = generally closer to home = safer
- **Higher numbers** = generally further out = more dangerous
- Exact spiral algorithm TBD — key principle: distance from origin determines danger level

---

## Galaxy Size
- **15-20 regions** in the starting galaxy
- ~50% have hub clusters (8-10 hub regions)
- ~50% are wild space (7-10 wild regions)
- 5-8 clusters total for the main gameplay experience
- Story-critical clusters can be **medium sized** (6-10 sub-systems) while normal clusters are **small** (3-5 sub-systems)

---

## Distance-Based Danger Scaling
- Danger level weighted by **distance from starting region (00)**
- Higher region numbers = heavier weight toward hostile and dangerous systems
- Starting region is safe and friendly
- Distant regions have more hostile systems, tougher enemies, better loot
- Wild space regions are the most dangerous regardless of number

---

## Navigation Between Systems

### Jump Drive Tiers
Travel between systems is gated by the ship's **jump drive range**. No fuel cost or movement points for travel.

| Tier | Region Range | Access | Notes |
|------|-------------|--------|-------|
| Tier 1 | Up to 5 regions | Starting cluster + immediate neighbors | Starter drive |
| Tier 2 | Up to 10 regions | Nearby regions open up | |
| Tier 3 | Up to 20 regions | Mid-game, most hub regions reachable | |
| Tier 4 | Up to 40 regions | Deep space, far wild space | |
| Tier 5 | Unlimited | Full galaxy access | End-game drive |

### Route Calculation
- Player inputs a destination address (XX-XXXX format)
- Ship calculates the **shortest jump path** through intermediate systems
- Ship reports the **number of jumps required**
- Multi-hop routes may pass through unknown territory
- Region distance determines if the destination is reachable with current jump drive tier

### Travel Risk
- **Unknown territory**: Slim chance of encounters when passing through
- **Known routes**: Much lower chance of negative events
- **New regions**: Always have a chance to trigger side missions or story events

---

## Starting Region (00)

### Composition
- **1 Hub System**: Regional hub where the tutorial delivery ends and main story begins
- **2-3 Friendly Sub-Systems**: Including the tutorial starting system (where the player is stranded)
- **1 Known Hostile System**: Well-known threat that NPCs warn the player about early — serves as the player's first clearance target and goal
- **0-1 Dead Systems**: Optional, for early exploration practice

### Tutorial Flow
1. Player starts **stranded** with a broken ship at a sub-system in region 00
2. The player knows only their current address (written on a **sticky note** on the dashboard)
3. Player repairs ship, talks to NPCs, learns hub address from **NPC dialogue** ("Don't you remember? It's XX-XXXX. It's also on your nav computer.")
4. Player discovers the address is also stored in the **ship's nav computer** (teaches that the nav computer stores addresses)
5. Player delivers cargo to the hub station
6. Tutorial ends, main story hook revealed
7. NPCs at the hub **warn about the nearby hostile system** — giving the player their first real goal

### What the Player Knows at Game Start
- Their current sub-system address (sticky note)
- Nothing else — all other addresses learned through gameplay

---

## Empty Space

### Jumping to Empty Addresses
- The vast majority of addresses in any region are **empty space**
- When a player jumps to an empty address, they **commit to the jump** (it costs travel like any other)
- On arrival, the ship's computer displays a **brief atmospheric scanner readout**
- Examples: "Scanner sweep complete. Nothing but distant starlight and cosmic dust." / "Faint radiation signatures detected — remnants of an ancient stellar event. No systems present."
- Empty space jumps carry the same **travel encounter risk** as any jump through unknown territory

---

## Procedural Generation (TBD)

### System Generation
- [ ] Algorithm for determining if an address (XX-XXXX) is populated
- [ ] Spiral layout mapping — how region numbers map to galaxy positions
- [ ] Cluster generation — how hubs and sub-systems are grouped within a region
- [ ] System type distribution per region (weighted by distance from origin)
- [ ] Jump drive range calculation relative to spiral position

### Regional Networks
- [ ] Exact number of populated addresses per region (target: 20-50)
- [ ] Hub-to-sub-system relationship generation
- [ ] Wild space region content generation
- [ ] Distance-based danger weight formula

---

## Open Design Questions
- Exact spiral layout algorithm for region positioning
- Distance calculation method between regions on the spiral
- How the 4 pirate faction quadrants map onto the spiral
- Which specific region numbers correspond to which hub types
- Procedural name generation approach for systems
