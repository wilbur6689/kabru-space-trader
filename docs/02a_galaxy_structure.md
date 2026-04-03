# Galaxy Structure

## Overview
This document defines the overall galaxy layout, regional hierarchy, address system, and the starting region. The galaxy uses a **grid-based quadrant system** centered on Earth, with danger scaling outward via Manhattan distance.

---

## Grid Layout

### Quadrant System
The galaxy is divided into **4 quadrants** around a central origin point (0,0).

```
         Q01 (top-left)      |      Q02 (top-right)
         Silk Hand            |      Phantom Circuit
         EASIEST              |      EASY-MED
                              |
    ──────────────────── (0,0) ────────────────────
                              |
         Q03 (bottom-left)    |      Q04 (bottom-right)
         Rust Collective      |      Void Reavers
         MED-HARD             |      HARDEST
```

### Grid Dimensions
- **12x12 grid per quadrant** (coordinates R0000 to R1111)
- **10x10 usable core** per quadrant (R0101 to R1010)
- **2-cell dead space border** around the entire galaxy edge
- Total grid: 24x24 across all quadrants
- Origin (0,0) is used for **logic calculations only** — not accessible in-game

---

## Address Format

### Full System Address
Every system has a unique address in the format:

```
Q[quadrant]-R[x][y]-[system]
```

**Example:** `Q02-R0307-4821` = Quadrant 2, Region at X=3 Y=7, System 4821

### Region ID
- Format: `Q[01-04]-R[xx][yy]`
- X and Y are 2-digit coordinates within the quadrant grid
- Example: `Q01-R0502` = Quadrant 1, column 5, row 2

### System Address Rules
System addresses within a region follow these constraints:
- **1st digit**: 1, 2, 3, or 4 (range 1000-4999)
- **2nd digit**: Even only (0, 2, 4, 6, 8)
- **3rd digit**: Odd only (1, 3, 5, 7, 9)
- **4th digit**: Any (0-9)

**Valid examples:** 2479, 1830, 4210
**Invalid examples:** 7239 (1st digit out of range), 3370 (3rd digit even), 1245 (2nd digit odd)

**Total valid addresses per region: 1,000** (4 x 5 x 5 x 10)

### Region Naming
- Regions are identified by **coordinate ID only** — no procedural names
- Individual systems within regions receive procedurally generated names

---

## Center and Safe Zone

### Earth
- Located at **Q01-R0101-1000**
- The origin point (0,0) sits at the corner where all four quadrants meet
- (0,0) is not accessible in-game — used only for distance calculations

### Safe Zone — Space Force Controlled
The 4 regions at R0101 in each quadrant form the **central safe zone**:
- `Q01-R0101` — Earth's region (tutorial + main story start)
- `Q02-R0101` — Safe zone, Phantom Circuit quadrant
- `Q03-R0101` — Safe zone, Rust Collective quadrant
- `Q04-R0101` — Safe zone, Void Reavers quadrant

**Safe zone properties:**
- Controlled by Earth's **Space Force**
- Prices are **stable and controlled** — change very rarely
- Prices kept **low enough to encourage players to venture outward**
- Full civilization, no pirate activity

---

## Pirate Faction Territories

### Quadrant Assignment
| Quadrant | Faction | Difficulty | Style |
|----------|---------|-----------|-------|
| Q01 | **Silk Hand** | Easiest | Smugglers, prefer bribery over violence |
| Q02 | **Phantom Circuit** | Easy-Medium | Info brokers, trade intel, least violent |
| Q03 | **Rust Collective** | Medium-Hard | Tanky scavengers, want your cargo, won't flee |
| Q04 | **Void Reavers** | Hardest | Most aggressive raiders, high damage, rarely flee |

### Faction Border Blending
Within **2 regions of a quadrant border**, factions blend based on proximity:

| Distance from Border | Home Faction | Neighbor Faction | Example |
|---------------------|-------------|-----------------|---------|
| 2 regions away | 75% | 25% | Q01-R0502: 75% Silk Hand, 25% Phantom Circuit |
| 1 region / on border | 50% | 50% | Q01-R0501 to Q02-R0501: 50/50 mix |
| 2 regions into neighbor | 25% | 75% | Q02-R0503: 75% Phantom Circuit, 25% Silk Hand |

This creates natural **turf war zones** at quadrant borders with mixed faction encounters.

---

## Danger Scaling

### Manhattan Distance
Danger is calculated using **Manhattan distance** from origin (0,0):
```
danger_distance = |x| + |y|
```

### Danger Zones
| Distance | Zone | Description |
|----------|------|-------------|
| 1-3 | Safe | Space Force controlled, civilized, low risk |
| 4-6 | Low-Medium | Pirate activity begins, mixed systems |
| 7-9 | Medium-High | Deep pirate territory, fewer hubs |
| 10+ | Extreme | Edge of galaxy, strongest enemies, best loot |

---

## Regional Economy
- Each region has a **randomly weighted specialty list** for all trade goods
- The **top 25% of specialties** become the region's dominant industries
- Major systems focus their services around the dominant specialties
- Surrounding systems act as supply chain, supporting dominant industries
- Inner regions (distance 1-3) generate **3-5 Major systems** per region
- Mid regions (distance 4-6) generate **1-3 Major systems**
- Outer regions (distance 7-9) generate **0-1 Major systems**
- Edge regions (distance 10+) generate **0 Major systems** (Wild Space)

---

## Populated Systems Per Region

### System Count
- **200 populated systems** per region (out of 1,000 valid addresses)
- 20% population density — remaining 800 addresses are empty space
- Each system is generated with a **population tier** and **alignment** (see [02b_system_types.md](02b_system_types.md))

### Population Tier Distribution (Weighted by Distance)

| Tier | Safe (1-3) | Mid (4-6) | Far (7-9) | Edge (10+) |
|------|-----------|-----------|-----------|-----------|
| Major (12-18 discoveries) | 25% | 15% | 5% | 2% |
| Moderate (7-11 discoveries) | 40% | 30% | 20% | 10% |
| Minor (3-6 discoveries) | 25% | 35% | 40% | 38% |
| Measly (3 discoveries) | 10% | 20% | 35% | 50% |

### Alignment Distribution (Weighted by Distance)

| Alignment | Safe (1-3) | Mid (4-6) | Far (7-9) | Edge (10+) |
|-----------|-----------|-----------|-----------|-----------|
| Friendly | 50% | 25% | 10% | 3% |
| Neutral | 25% | 25% | 15% | 10% |
| Hostile | 10% | 15% | 20% | 22% |
| Pirate-Controlled | 5% | 25% | 40% | 50% |
| Dead | 10% | 10% | 15% | 15% |

All 20 tier + alignment combinations are valid but weighted — some are common, some are rare.

---

## Navigation Between Systems

### Jump Drive Tiers
Travel between **regions** is gated by the ship's jump drive range. Each region counts as **one jump**.

| Tier | Max Manhattan Distance | Access |
|------|----------------------|--------|
| Tier 1 | 2 regions | Starting safe zone + immediate neighbors |
| Tier 2 | 5 regions | Nearby quadrant territory |
| Tier 3 | 9 regions | Mid-game, most hub regions reachable |
| Tier 4 | 14 regions | Deep space, far edges |
| Tier 5 | Unlimited | Full galaxy access |

### Inter-Region Jumps
- Player selects a destination region
- Jump drive calculates the route through intermediate regions
- **No pass-thru events** during inter-region travel
- Only the **destination region** can trigger an arrival event
- Jump drive tier must cover the Manhattan distance to the destination

### Intra-Region Travel
- Moving between systems **within the same region** routes through intermediate systems
- **Pass-thru events may occur** at systems along the route
- Event chance is **scaled by danger zone**:
  - Near center (distance 1-3): Low chance
  - Mid-range (distance 4-6): Moderate chance
  - Far regions (distance 7-9): High chance
  - Edge regions (distance 10+): Very high chance
- Events are also influenced by **what exists at the pass-thru system** (e.g., passing near a Pirate Turf War Zone has high pull-in chance, a Distress Signal may offer a choice to stop)
- Events can be **negative** (ambush, trap, turf war crossfire) or **positive** (distress signal rescue, ancient beacon discovery)

---

## Dead Space Border

### Border Regions (Outer 2 Cells)
- The outermost 2 cells of each quadrant's 12x12 grid are **dead space**
- Accessible but empty — only dead systems and void
- Nav computer warns: *"Entering uncharted dead space. No viable systems detected."*
- No hubs, no friendly systems, no pirate activity
- Serves as a natural galaxy boundary without a hard wall

---

## Starting Region (Q01-R0101)

### Composition
- **Earth** at Q01-R0101-1000 — main hub, Space Force headquarters, main story start
- **2-3 Friendly Sub-Systems** — including the tutorial starting system (player is stranded here)
- **1 Known Hostile System** — NPCs warn about it early, first clearance target
- **0-1 Dead Systems** — optional early exploration practice

### Tutorial Flow
1. Player starts **stranded** with a broken ship at a sub-system in Q01-R0101
2. Player learns basic mechanics: repair ship, talk to NPCs
3. Player flies to a **second sub-system** to deliver first goods
4. At the delivery sub-system, player learns about the **main story line**
5. Player is directed to fly to **Earth** (Q01-R0101-1000) to begin the main story
6. NPCs at Earth **warn about the nearby hostile system** — first real goal

### What the Player Knows at Game Start
- Their current sub-system address (sticky note on dashboard)
- Nothing else — all other addresses learned through gameplay

---

## Open Design Questions
- Exact pass-thru event chance percentages per danger zone
- How inter-region route calculation works (shortest path algorithm)
- Intra-region routing — how does the ship choose which systems to pass through
- Which hub types appear in which quadrants (themed distribution)
- How Space Force pricing is set to encourage outward exploration
- Dead space border — any hidden secrets for explorers, or truly empty
- Procedural name generation approach for individual systems
