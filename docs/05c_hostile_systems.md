# Pirate-Controlled System Clearing

## Overview
**Pirate-Controlled** systems (alignment) are occupied by one of the 4 pirate factions. The player must weaken the faction's hold by discovering and eliminating enemy outposts, then defeating the faction boss to liberate the system. Note: this mechanic applies specifically to Pirate-Controlled alignment, not Hostile alignment (which represents environmental dangers, not faction occupation).

---

## Progressive Clearing Mechanic

### Clearing Process
1. Player enters a hostile system and begins **searching** for locations
2. Some discoveries are **enemy outposts** — combat encounters
3. Player must clear a **required number of outposts** to trigger the boss
4. Required outpost count **scales with Manhattan distance from origin**:
   - Safe/Inner (distance 1-5): 2 outposts
   - Mid regions (distance 6-10): 3 outposts
   - Far regions (distance 11-15): 4 outposts
   - Edge regions (distance 16+): 5 outposts
5. After clearing enough outposts, **environmental signals warn of an incoming ship** (sensors detect approach, 2-3 turns out)
6. Player has a window to **leave and prepare** (repair, upgrade, restock)
7. When the player **returns to the system**, the boss is waiting and **attacks immediately**

---

## Boss Mechanics

### Boss Profile
- Boss is the **faction Warlord** — highest fame enemy in the system
- Boss fame = base 8 + Manhattan distance modifier (manhattan_distance / 2.5, floored)
- Uses the same MasterEntity system as all other entities

### Boss Encounter Flow
1. Environmental warning detected after clearing required outposts (sensors pick up incoming ship)
2. Player has 2-3 turns before the boss arrives if they stay
3. Player can leave to prepare — but the boss will be waiting when they return and attacks immediately
5. Single combat encounter — fleeing is possible but **very difficult**
6. Victory: system is liberated
7. Defeat: one of three outcomes chosen at random:
   - **Death** — respawn mechanics apply (worst case)
   - **Knocked out** — player wakes up in a nearby system, loses cargo and credits
   - **Captured** — player must pay a ransom to be released, or attempt an escape event

### Post-Clear State
- Defeating the boss **converts the system from Pirate-Controlled to Friendly**
- System reverts to its **original generated state** — all systems are generated as Friendly first during world creation, then transformation rules apply pirate/hostile/dead modifiers. Clearing a system restores the original Friendly version.
- Previously hidden locations become accessible
- May open new trade routes and shift regional prices
- **Stabilizes neighboring systems** — reduces random pirate/hostile encounter frequency in adjacent friendly systems

---

## Pirate-Controlled System Properties

### While Pirate-Controlled
- System details are hidden until explored — everything must be discovered
- Player can **gather intel from NPCs in surrounding systems** before entering (faction, rough danger level, hints)
- Without asking, player only knows general regional info (e.g., "Void Reavers control this region")
- **Tax charged** before the player can conduct business (15% minus 1% per fame level)
- Higher risk but higher reward than Friendly systems
- Pirate faction enemies patrol the system
- Alignment: **Pirate-Controlled**

### After Clearing
- Alignment changes to **Friendly**
- Basic services restored
- Hidden locations become accessible
- No more pirate encounters in the system
- Tax removed
- System contributes to regional stability

---

## Resolved Design Decisions
- **Fleeing boss fights:** Possible but very difficult
- **Boss defeat outcomes:** Random — death, knocked out (lose cargo/credits, wake up nearby), or captured (ransom/escape event)
- **Outpost rebuilds:** None — once outposts are cleared, they stay cleared. Boss waits indefinitely.
- **Cleared systems:** Stay friendly permanently, never revert to pirate-controlled
- **Stabilization effect:** Clearing a system reduces pirate encounter frequency in neighboring friendly systems
- **World generation order:** All systems generated as Friendly first, then transformation rules apply pirate/hostile/dead modifiers. Clearing restores original state.
- **Warning signal:** Environmental — sensors detect incoming ship, not direct comms from boss

## Open Design Questions
- Boss encounter combat design — phases, special abilities, unique mechanics
- Specific rewards for clearing a hostile system beyond system conversion
- Whether the boss has unique loot drops
- World generation transformation rules — what determines which systems become pirate-controlled, hostile, dead, etc.
