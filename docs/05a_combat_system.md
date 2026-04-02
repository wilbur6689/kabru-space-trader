# Combat System

## Overview
Combat is **turn-based**, consistent with the overall game. It is triggered by dangerous discoveries, travel encounters, or hostile system events. All entities (player, enemies) use the **MasterEntity** system with fame levels.

---

## Combat Actions (5 Total)

| Action | Effect | Requirements |
|--------|--------|-------------|
| **Attack** | Deal damage based on weapons + combat skill | Always available |
| **Defend** | Reduce incoming damage, prepare counter | Always available |
| **Flee** | Attempt escape based on engine maneuverability + piloting skill | Always available |
| **Negotiate** | Attempt to talk down — costs credits, uses negotiation skill | Always available |
| **Intimidate** | Attempt to scare enemy into paying you and retreating | Only available when player fame is **equal to or up to 2 above** enemy fame |

---

## Intimidation System

### Pre-Combat Fame Check
- **Player fame 3+ above enemy**: Combat **skipped entirely** — enemy flees on sight
- **Player fame equal to or up to 2 above**: Combat starts with **Intimidate** available as a 5th action
- **Player fame below enemy**: No intimidate option — must fight, flee, or negotiate

### Intimidate Outcomes
- **Success**: Enemy pays credits to the player and retreats
- **Failure**: Enemy is angered — attacks with a **minor damage bonus** on their next turn

---

## Combat Flow
```
Encounter Triggered
  -> Fame comparison check
     -> If player fame 3+ above: enemy flees, combat skipped
     -> Otherwise: combat begins
  -> Present enemy info (type, strength, ship class, faction, fame level)
  -> Player chooses action:
     a) Attack — deal damage based on weapons + combat skill
     b) Defend — reduce incoming damage, prepare counter
     c) Flee — attempt escape based on maneuverability + piloting skill
     d) Negotiate — attempt to talk down (negotiation skill, costs credits)
     e) Intimidate — scare enemy (only if fame is equal to or up to 2 above)
  -> Resolve player action
  -> Enemy responds based on faction behavior
  -> Update ship status (hull, shields)
  -> Check resolution:
     - Victory: enemy defeated -> loot
     - Defeat: ship destroyed -> death/respawn
     - Escape: player flees -> no loot, possible cargo loss
     - Negotiated: pay off or talk down -> credits lost, no damage
     - Intimidated: enemy pays credits -> retreats
  -> Loop or end
```

---

## Combat Resolution

### Victory
- Enemy defeated, loot dropped based on enemy type and faction
- Combat XP awarded based on enemy fame level
- Ship damage persists — must be repaired at a hub or station

### Defeat
- Ship destroyed, player dies
- Respawn at last visited hub with loaner Shuttle
- Ship wreckage remains at death location for recovery

### Escape
- Player successfully flees combat
- No loot, possible cargo loss during escape
- Piloting XP awarded for successful flee

### Negotiation
- Credits paid to enemy based on negotiation skill and enemy disposition
- No damage taken, no loot gained
- Negotiation XP awarded

### Intimidation
- Enemy pays credits and retreats
- No combat occurs, no damage
- Fame-based mechanic — becomes more available as player grows

---

## Damage Mechanics

### Damage Order
1. Incoming damage hits **shields** first
2. When shields are depleted, damage hits **hull**
3. Hull reaches zero = ship destroyed

### Shield Recharge
- Shields regenerate a small amount between combat turns
- Recharge rate based on shield tier
- Hull does NOT regenerate during combat

### Defend Action
- Reduces incoming damage for that turn
- May enable a counter-attack on the next turn (TBD)

---

## Open Design Questions
- Exact damage formulas (weapons + combat skill vs. hull/shields)
- Hit/miss chance calculation (accuracy vs. evasion)
- Defend action specifics — damage reduction percentage, counter-attack mechanics
- Negotiate cost formula (base cost modified by negotiation skill and enemy type)
- Intimidation success rate formula
- Flee success rate formula (engine tier + piloting skill vs. enemy speed)
- Whether combat has a maximum turn limit
- Multi-enemy encounters (pirate fleets) — simultaneous or sequential
- Loot generation tables per enemy type
- Combat XP award formula
