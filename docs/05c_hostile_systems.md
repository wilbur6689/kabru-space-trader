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
5. After clearing enough outposts, the **boss appears on the next search/turn**
6. Boss **warns the player** that an attack is coming in **2-3 turns**
7. Player has a minimum of **1 turn to leave** and prepare (repair, upgrade, restock)
8. When the player **returns to the system**, the boss attacks

---

## Boss Mechanics

### Boss Profile
- Boss is the **faction Warlord** — highest fame enemy in the system
- Boss fame = base 8 + Manhattan distance modifier (manhattan_distance / 2.5, floored)
- Uses the same MasterEntity system as all other entities

### Boss Encounter Flow
1. Boss warning received after clearing required outposts
2. Player has 2-3 turns before the boss arrives
3. Player can leave to prepare (minimum 1 turn window)
4. On return (or if player stays), boss attacks
5. Single combat encounter — no fleeing (TBD: or very difficult flee?)
6. Victory: system is liberated
7. Defeat: player dies, respawn mechanics apply

### Post-Clear State
- Defeating the boss **converts the system from hostile to friendly**
- System reverts to its **pre-occupation state** with basic services
- Previously hidden locations may become known/accessible
- May open new trade routes and shift regional prices
- May stabilize neighboring friendly systems

---

## Boss Rebuilds
- If the player **does not return for too long**, the faction begins **rebuilding outposts**
- Player must re-clear some outposts before the boss is triggered again
- Prevents indefinite delay — creates urgency without a hard timer

---

## Pirate-Controlled System Properties

### While Pirate-Controlled
- Nothing known on arrival — everything hidden until explored
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

## Open Design Questions
- Boss encounter combat design — phases, special abilities, unique mechanics
- Whether the player can flee from a boss fight
- Outpost rebuild rate when player delays the boss fight
- How long before rebuilding starts after outpost clearing
- Whether cleared hostile systems can become hostile again over time
- Specific rewards for clearing a hostile system beyond system conversion
- How hostile system clearing affects pirate faction influence in the region
- Whether the boss has unique loot drops
- How the "2-3 turn warning" is communicated to the player (visual, audio, text)
