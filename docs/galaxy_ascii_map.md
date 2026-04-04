# Galaxy Map — ASCII Reference

## Full Galaxy Grid (Simplified)

Each quadrant is a 12x12 grid. The inner 10x10 is usable space. The outer 2 cells are dead space border.
Origin (0,0) is at center — used for logic only, not in-game. Earth is at Q01-R0101-1000.

```
     Q01 — SILK HAND (Easiest)                          Q02 — PHANTOM CIRCUIT (Easy-Med)
     Top-Left Quadrant                                   Top-Right Quadrant

  12 11 10 09 08 07 06 05 04 03 02 01 | 01 02 03 04 05 06 07 08 09 10 11 12
  ─────────────────────────────────────┼─────────────────────────────────────
12│ xx xx .. .. .. .. .. .. .. .. xx xx | xx xx .. .. .. .. .. .. .. .. xx xx │12
11│ xx .. .. .. .. .. .. .. .. .. .. xx | xx .. .. .. .. .. .. .. .. .. .. xx │11
10│ .. .. ── ── ── ── ── ── ── ── .. .. | .. .. ── ── ── ── ── ── ── ── .. .. │10
09│ .. .. │E  E  E  E  E  E  E  E│ .. .. | .. .. │E  E  E  E  E  E  E  E│ .. .. │09
08│ .. .. │E  ·  ·  ·  ·  ·  ·  H│ .. .. | .. .. │H  ·  ·  ·  ·  ·  ·  E│ .. .. │08
07│ .. .. │E  ·  ·  ·  ·  ·  ·  M│ .. .. | .. .. │M  ·  ·  ·  ·  ·  ·  E│ .. .. │07
06│ .. .. │E  ·  ·  ·  ·  ·  ·  M│ .. .. | .. .. │M  ·  ·  ·  ·  ·  ·  E│ .. .. │06
05│ .. .. │E  ·  ·  ·  ·  ·  ·  L│ .. .. | .. .. │L  ·  ·  ·  ·  ·  ·  E│ .. .. │05
04│ .. .. │E  ·  ·  ·  ·  ·  ·  L│ .. .. | .. .. │L  ·  ·  ·  ·  ·  ·  E│ .. .. │04
03│ .. .. │E  ·  ·  ·  ·  ·  S  S│ .. .. | .. .. │S  S  ·  ·  ·  ·  ·  E│ .. .. │03
02│ .. .. │E  ·  ·  ·  ·  S  S  S│ .. .. | .. .. │S  S  S  ·  ·  ·  ·  E│ .. .. │02
01│ .. .. │E  ·  ·  ·  S  S  S  @│ .. .. | .. .. │@  S  S  S  ·  ·  ·  E│ .. .. │01
  ─────────────────────────────────────┼─────────────────────────────────────
01│ .. .. │E  ·  ·  ·  S  S  S  @│ .. .. | .. .. │@  S  S  S  ·  ·  ·  E│ .. .. │01
02│ .. .. │E  ·  ·  ·  ·  S  S  S│ .. .. | .. .. │S  S  S  ·  ·  ·  ·  E│ .. .. │02
03│ .. .. │E  ·  ·  ·  ·  ·  S  S│ .. .. | .. .. │S  S  ·  ·  ·  ·  ·  E│ .. .. │03
04│ .. .. │E  ·  ·  ·  ·  ·  ·  L│ .. .. | .. .. │L  ·  ·  ·  ·  ·  ·  E│ .. .. │04
05│ .. .. │E  ·  ·  ·  ·  ·  ·  L│ .. .. | .. .. │L  ·  ·  ·  ·  ·  ·  E│ .. .. │05
06│ .. .. │E  ·  ·  ·  ·  ·  ·  M│ .. .. | .. .. │M  ·  ·  ·  ·  ·  ·  E│ .. .. │06
07│ .. .. │E  ·  ·  ·  ·  ·  ·  M│ .. .. | .. .. │M  ·  ·  ·  ·  ·  ·  E│ .. .. │07
08│ .. .. │E  ·  ·  ·  ·  ·  ·  H│ .. .. | .. .. │H  ·  ·  ·  ·  ·  ·  E│ .. .. │08
09│ .. .. │E  E  E  E  E  E  E  E│ .. .. | .. .. │E  E  E  E  E  E  E  E│ .. .. │09
10│ .. .. ── ── ── ── ── ── ── ── .. .. | .. .. ── ── ── ── ── ── ── ── .. .. │10
11│ xx .. .. .. .. .. .. .. .. .. .. xx | xx .. .. .. .. .. .. .. .. .. .. xx │11
12│ xx xx .. .. .. .. .. .. .. .. xx xx | xx xx .. .. .. .. .. .. .. .. xx xx │12
  ─────────────────────────────────────┼─────────────────────────────────────

     Q03 — RUST COLLECTIVE (Med-Hard)                    Q04 — VOID REAVERS (Hardest)
     Bottom-Left Quadrant                                Bottom-Right Quadrant
```

### Legend
```
  @  = Safe Zone (Space Force) — R0101 in each quadrant. Earth is Q01-R0101-1000
  S  = Safe Zone (Manhattan distance 1-3)
  L  = Low-Medium danger (distance 4-6)
  M  = Medium-High danger (distance 7-9)
  H  = High/Extreme danger (distance 10+)
  E  = Extreme — galaxy edge, wild space, pirate strongholds
  ·  = Usable region (danger determined by Manhattan distance from center)
  .. = Dead space border (outer 2 cells) — accessible but empty
  xx = Dead space corner — completely barren
  │──│ = Usable core boundary (10x10 per quadrant)
```

---

## Danger Zones by Manhattan Distance

Manhattan distance = |x| + |y| from origin (0,0).

```
                    Distance from center
                ◄────────────────────────►

                Q01               Q02
         10  9  8  7  6  5  4  3  2  1  1  2  3  4  5  6  7  8  9  10
       ┌──────────────────────────────────────────────────────────────────┐
   10  │ 20 19 18 17 16 15 14 13 12 11 11 12 13 14 15 16 17 18 19 20    │
    9  │ 19 18 17 16 15 14 13 12 11 10 10 11 12 13 14 15 16 17 18 19    │
    8  │ 18 17 16 15 14 13 12 11 10  9  9 10 11 12 13 14 15 16 17 18    │
    7  │ 17 16 15 14 13 12 11 10  9  8  8  9 10 11 12 13 14 15 16 17    │
    6  │ 16 15 14 13 12 11 10  9  8  7  7  8  9 10 11 12 13 14 15 16    │
    5  │ 15 14 13 12 11 10  9  8  7  6  6  7  8  9 10 11 12 13 14 15    │
    4  │ 14 13 12 11 10  9  8  7  6  5  5  6  7  8  9 10 11 12 13 14    │
    3  │ 13 12 11 10  9  8  7  6  5  4  4  5  6  7  8  9 10 11 12 13    │
    2  │ 12 11 10  9  8  7  6  5  4  3  3  4  5  6  7  8  9 10 11 12    │
    1  │ 11 10  9  8  7  6  5  4  3  2  2  3  4  5  6  7  8  9 10 11    │
       ├──────────────────────────────────(0,0)───────────────────────────┤
    1  │ 11 10  9  8  7  6  5  4  3  2  2  3  4  5  6  7  8  9 10 11    │
    2  │ 12 11 10  9  8  7  6  5  4  3  3  4  5  6  7  8  9 10 11 12    │
    3  │ 13 12 11 10  9  8  7  6  5  4  4  5  6  7  8  9 10 11 12 13    │
    4  │ 14 13 12 11 10  9  8  7  6  5  5  6  7  8  9 10 11 12 13 14    │
    5  │ 15 14 13 12 11 10  9  8  7  6  6  7  8  9 10 11 12 13 14 15    │
    6  │ 16 15 14 13 12 11 10  9  8  7  7  8  9 10 11 12 13 14 15 16    │
    7  │ 17 16 15 14 13 12 11 10  9  8  8  9 10 11 12 13 14 15 16 17    │
    8  │ 18 17 16 15 14 13 12 11 10  9  9 10 11 12 13 14 15 16 17 18    │
    9  │ 19 18 17 16 15 14 13 12 11 10 10 11 12 13 14 15 16 17 18 19    │
   10  │ 20 19 18 17 16 15 14 13 12 11 11 12 13 14 15 16 17 18 19 20    │
       └──────────────────────────────────────────────────────────────────┘

                Q03               Q04

  Danger Zones:
    2-3   = SAFE        (Space Force controlled, stable prices)
    4-6   = LOW-MED     (Pirate activity begins, mixed systems)
    7-9   = MED-HIGH    (Deep pirate territory, fewer hubs)
    10-13 = HIGH        (Edge of galaxy, wild space, strongholds)
    14-20 = EXTREME     (Galaxy corners, maximum danger, best loot)
```

---

## Pirate Faction Territories

```
            ┌─────────────────────┬─────────────────────┐
            │                     │                     │
            │   Q01: SILK HAND    │  Q02: PHANTOM       │
            │   (Easiest)         │  CIRCUIT (Easy-Med) │
            │                     │                     │
            │   Smugglers         │  Info brokers       │
            │   Prefer bribery    │  Trade intel        │
            │   Flee if outmatched│  Least violent      │
            │                     │                     │
            ├──────────┬──@ @──┬──┼──────────────────────┤
            │          │ SAFE │  │                     │
            │  ~~~~~~~~│ ZONE │~~│~~~~~  Blending      │
            │  Faction │  @   │  │       zones within  │
            │  blending│ EARTH│  │       2 regions of  │
            │  zones   │      │  │       any border    │
            │  ~~~~~~~~│      │~~│~~~~~                │
            │          │      │  │                     │
            ├──────────┴──────┴──┼──────────────────────┤
            │                     │                     │
            │   Q03: RUST         │  Q04: VOID REAVERS  │
            │   COLLECTIVE        │  (Hardest)          │
            │   (Med-Hard)        │                     │
            │                     │  Most aggressive    │
            │   Tanky scavengers  │  High damage        │
            │   Want your cargo   │  Rarely flee        │
            │   Won't flee        │  Fight to the end   │
            │                     │                     │
            └─────────────────────┴─────────────────────┘
```

### Faction Border Blending (2-Region Gradient)
```
  Example: Q01/Q02 horizontal border (top quadrants)

  Q01 territory                    Q02 territory
  ◄──────────────────────────────────────────────►

  100% Silk  75/25   50/50   25/75  100% Phantom
  ┌────────┬────────┬────────┬────────┬────────┐
  │ Pure   │ Mostly │ Mixed  │ Mostly │ Pure   │
  │ Silk   │ Silk + │ Turf   │ Phntm +│ Phntm  │
  │ Hand   │ Phntm  │ War    │ Silk   │ Circuit│
  │        │        │ Zone   │        │        │
  └────────┴────────┴────────┴────────┴────────┘
  R0301    R0501    Border   R0501    R0301
  (Q01)    (Q01)    line     (Q02)    (Q02)
```

---

## Jump Drive Range Visualization

```
  Showing reachable regions from Earth (Q01-R0101) per jump drive tier.
  Each number = Manhattan distance from origin.

                    Q01                Q02
              10  8  6  4  2    2  4  6  8  10
            ┌────────────────────────────────────┐
        10  │                                    │
         8  │              ┌─────────┐           │
         6  │           ┌──┤  T3: 9  ├──┐        │
         4  │        ┌──┤  ├─────────┤  ├──┐     │
         2  │     ┌──┤  │  │ T2: 5   │  │  ├──┐  │
         1  │     │  │  │  ├─────────┤  │  │  │  │
            ├─────┤  │  │  │T1:2 @ E │  │  │  ├──┤
         1  │     │  │  │  ├─────────┤  │  │  │  │
         2  │     └──┤  │  │         │  │  ├──┘  │
         4  │        └──┤  ├─────────┤  ├──┘     │
         6  │           └──┤  T4: 14 ├──┘        │
         8  │              └─────────┘           │
        10  │        T5: Unlimited               │
            └────────────────────────────────────┘
                    Q03                Q04

  Tier 1:  2 Manhattan distance  — Safe zone only
  Tier 2:  5 Manhattan distance  — Nearby quadrant territory
  Tier 3:  9 Manhattan distance  — Mid-game, most regions reachable
  Tier 4: 14 Manhattan distance  — Deep space, far edges
  Tier 5: Unlimited              — Full galaxy access
```

---

## Region Address Examples

```
  Full system address format: Q[quadrant]-R[column][row]-[system]

  Q01-R0101-1000  =  Earth (Quadrant 1, column 01, row 01, system 1000)
  Q02-R0307-4821  =  Quadrant 2, column 03, row 07, system 4821
  Q04-R0909-2470  =  Deep in Void Reaver territory, near galaxy edge
  Q03-R1010-1210  =  Farthest usable corner of Q03 (distance 20 from center)

  System address digit rules:
    1st digit: 1-4  (range 1000-4999)
    2nd digit: even (0, 2, 4, 6, 8)
    3rd digit: odd  (1, 3, 5, 7, 9)
    4th digit: 0-9

  Valid:   2479, 1830, 4210, 3650
  Invalid: 7239 (1st out of range), 3370 (3rd even), 1245 (2nd odd)

  1,000 valid addresses per region
  200 populated systems per region (20% density)
```

---

## System Distribution at a Glance

```
  Near Center (dist 1-3)          Galaxy Edge (dist 10+)
  ┌──────────────────────┐        ┌──────────────────────┐
  │ ★★★★ Major    25%    │        │ ★     Major     2%   │
  │ ★★★  Moderate  40%   │        │ ★     Moderate  10%  │
  │ ★★   Minor    25%    │        │ ★★★★ Minor     38%   │
  │ ★    Measly   10%    │        │ ★★★★★ Measly   50%  │
  ├──────────────────────┤        ├──────────────────────┤
  │ Friendly      50%    │        │ Friendly        3%   │
  │ Neutral       25%    │        │ Neutral        10%   │
  │ Hostile       10%    │        │ Hostile        22%   │
  │ Pirate-Ctrl    5%    │        │ Pirate-Ctrl    50%   │
  │ Dead          10%    │        │ Dead           15%   │
  └──────────────────────┘        └──────────────────────┘

  Safe, civilized core               Sparse, dangerous edge
  Many Major Friendly systems        Mostly Measly Pirate-Controlled
  Space Force patrolled               Wild space — no respawn
```

---

## Dead Space Border

```
  The outermost 2 cells of each quadrant are dead space.
  Accessible but empty — only Dead + Measly systems and void.

  ┌──────────────────────────────────────────────┐
  │ xx xx xx xx xx xx xx xx xx xx xx xx xx xx xx │  <- Dead space (row 12)
  │ xx ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  · xx│  <- Dead space (row 11)
  │ ·  ·  ┌─────────────────────────────┐  ·  · │
  │ ·  ·  │                             │  ·  · │
  │ ·  ·  │     USABLE CORE (10x10)     │  ·  · │
  │ ·  ·  │                             │  ·  · │
  │ ·  ·  │     200 systems per region   │  ·  · │
  │ ·  ·  │     1,000 valid addresses    │  ·  · │
  │ ·  ·  │                             │  ·  · │
  │ ·  ·  └─────────────────────────────┘  ·  · │
  │ xx ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  · xx│
  │ xx xx xx xx xx xx xx xx xx xx xx xx xx xx xx │
  └──────────────────────────────────────────────┘

  Nav computer: "Entering uncharted dead space. No viable systems detected."
```
