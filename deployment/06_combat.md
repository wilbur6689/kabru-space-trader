# Stage 06: Combat

## Goal
Implement the full turn-based combat system: encounters, enemy generation, damage resolution, flee/negotiate/intimidate options, loot, death, and respawn. By the end of this stage the player can fight enemies, use fame to intimidate weaker foes, die and respawn with a loaner ship, and recover their wreckage.

**Design Docs:** [05a_combat_system.md](../docs/05a_combat_system.md), [05b_enemies_and_factions.md](../docs/05b_enemies_and_factions.md), [03_ship.md](../docs/03_ship.md) (death/recovery), [12_balancing.md](../docs/12_balancing.md)
**Prerequisites:** Stage 05 complete (fame system, combat skill, pilot stats)
**Outputs:** CombatManager, enemy generation, combat UI, death/respawn system, loot system

---

## Phase 6.1 — Enemy Generation

**Goal:** Create the system that generates enemies appropriate to the current system's danger level, alignment, and faction.

### Tasks
- [ ] Create `scripts/systems/enemy_generator.gd`
- [ ] Define enemy type templates using MasterEntity:
  | Type | Base Fame | Description |
  |------|-----------|-------------|
  | Petty Pirate | 1 | Weak, flees easily |
  | Gang Enforcer | 3 | Tougher, faction-loyal |
  | Pirate Captain | 4 | Commands a crew |
  | Patrol Ship | 4 | Legitimate authority |
  | Bounty Hunter | 5 | Targets wanted players |
  | Pirate Fleet | 6 | Multiple ships |
  | Alien Vessel | 7 | Unknown tech |
  | Warlord | 8 | Faction boss |
- [ ] Implement fame scaling by distance:
  ```
  enemy_fame = base_fame + floor(manhattan_distance / 2.5)
  ```
- [ ] Implement stat generation per enemy:
  - Hull, shields, damage, accuracy, defense, flee_chance
  - Scale with fame level
  - Faction-specific modifiers:
    - **Silk Hand**: Higher negotiation resistance, prefer bribery, lower damage
    - **Phantom Circuit**: Better accuracy, info-based loot, avoid prolonged combat
    - **Rust Collective**: Higher hull/shields, won't flee, lower accuracy
    - **Void Reavers**: High damage, aggressive, never negotiate
- [ ] Implement `generate_enemy(system_data: SystemData) -> MasterEntity`:
  - Select enemy type weighted by system alignment and danger zone
  - Apply faction modifiers
  - Generate loot table
- [ ] Implement loot table generation:
  - Shards (scaled by enemy fame)
  - Cargo goods (chance based on enemy type)
  - Rare drops (low chance, better at higher fame)
  - Faction-specific loot items

### Verification
- Enemies in safe zones have low fame (1-3)
- Enemies in extreme zones have high fame (8+)
- Faction modifiers create noticeably different enemy behaviors
- Loot tables produce appropriate rewards

---

## Phase 6.2 — Combat Manager

**Goal:** Implement the core combat resolution system — turn-based, action selection, damage calculation, and outcome determination.

### Tasks
- [ ] Create `scripts/systems/combat_manager.gd`
- [ ] Implement pre-combat fame check:
  - Player fame 3+ above enemy fame → enemy flees on sight (no combat)
  - Award intimidation credits (small shard reward)
  - Emit `EventBus.combat_avoided(enemy, "intimidation")`
- [ ] Implement combat turn loop:
  1. Display enemy info
  2. Player selects action
  3. Resolve player action
  4. If enemy alive, resolve enemy action
  5. Check for end conditions
  6. Repeat
- [ ] Implement player actions:
  - **Attack**: Deal damage based on weapon tier + combat skill + fame bonus
    ```
    damage = base_weapon_damage × (1 + combat_skill × 0.03) × (1 + fame_bonus) - enemy_defense
    ```
  - **Defend**: Reduce incoming damage by 50%, shield recharge doubled this turn
  - **Flee**: Success chance based on engine tier + piloting skill vs enemy speed
    - Success: escape combat, may lose some cargo
    - Failure: enemy gets free attack
  - **Negotiate**: Pay shards to end combat peacefully
    - Cost based on enemy fame level
    - Success chance based on Negotiation skill
    - Some enemies refuse negotiation (Void Reavers, bosses)
  - **Intimidate**: Available when player fame is 1-2 above enemy fame
    - Success: enemy pays credits and retreats
    - Failure: enemy attacks with +25% damage bonus
    - Success chance: 60% at +1 fame, 85% at +2 fame
- [ ] Implement damage resolution:
  - Damage hits shields first
  - Remaining damage hits hull
  - Shields recharge between player turns (recharge rate from shield component)
  - Hull does not regenerate during combat
- [ ] Implement enemy AI:
  - Each turn, enemy selects action based on faction behavior and current state:
    - Low hull → attempt flee (except Rust Collective)
    - High shields → aggressive attack
    - Silk Hand: may attempt to bribe player at low hull
    - Void Reavers: always attack, never flee
- [ ] Implement combat outcomes:
  - **Victory**: Enemy hull reaches 0 → award loot, XP, emit `EventBus.combat_ended("victory")`
  - **Defeat**: Player hull reaches 0 → trigger death sequence
  - **Escape**: Player successfully flees → no loot, some cargo may be jettisoned
  - **Negotiation**: Peace bought → shards spent, no loot
  - **Intimidation**: Enemy pays and retreats → credits earned

### Verification
- Fame 3+ above enemy skips combat entirely
- Each action resolves correctly with proper damage
- Shields absorb damage before hull
- Enemy AI behaves differently per faction
- All 5 outcomes trigger correctly
- Combat XP awarded on victory

---

## Phase 6.3 — Combat UI

**Goal:** Build the combat interface that displays within the cockpit view.

### Tasks
- [ ] Create `scenes/combat/combat_screen.tscn`
- [ ] **Cockpit window area**: Show enemy ship visual (placeholder sprite per enemy type)
- [ ] **Enemy info panel** (console screen area):
  - Enemy name, type, faction
  - Fame level (with comparison to player)
  - Hull bar, shield bar
  - Faction behavior hint (e.g., "This faction never negotiates")
- [ ] **Player status** (dashboard area):
  - Hull bar with current/max
  - Shield bar with current/max and recharge indicator
  - Weapon charge/status
- [ ] **Action buttons** (left monitor area):
  - Attack, Defend, Flee, Negotiate, Intimidate
  - Dim unavailable actions (Intimidate only when fame allows, Negotiate greyed for certain factions)
  - Tooltip showing success chances where applicable
- [ ] **Combat log** (scrolling text area):
  - "You attack for X damage. Enemy shields absorb Y."
  - "Enemy attacks for X damage. Your hull takes Y."
  - "You attempt to flee... Success!"
  - Color-coded: player actions in cyan, enemy actions in red, outcomes in yellow
- [ ] **Loot popup** on victory:
  - Show shards earned, goods received, XP gained
  - "Collect" button to add to cargo (check space)
  - If cargo full, choose what to keep/discard
- [ ] Implement visual feedback:
  - Screen shake on hull damage (placeholder)
  - Shield flash on shield hit
  - Enemy sprite flashes red on damage
  - Vignette darkens when hull below 25%

### Verification
- Combat screen replaces system overview during combat
- All action buttons work and produce combat log entries
- Enemy health bars update in real time
- Loot popup shows correct rewards
- Visual feedback fires on damage events
- Returns to system overview after combat ends

---

## Phase 6.4 — Death and Respawn System

**Goal:** Implement the death consequence: respawn at a safe location with a loaner ship, wreckage stays at death location for recovery.

### Tasks
- [ ] Implement death trigger:
  - When player hull reaches 0, emit `EventBus.player_died`
  - Save wreckage state: ship class, components, remaining cargo (0-50% looted randomly)
  - Record death location address
- [ ] Create wreckage data structure:
  - `wreckage_address: String`
  - `wreckage_ship: Ship` (damaged, partial cargo)
  - `wreckage_exists: bool`
  - Store in GameState
- [ ] Implement respawn:
  - Determine respawn point:
    1. Last visited Tier 3 player station (if any)
    2. Last visited Friendly Major or Moderate system
    3. Starting hub (fallback)
  - Give player a loaner Shuttle (Tier 1 everything, empty cargo)
  - Set current location to respawn point
  - Emit `EventBus.player_respawned`
  - Auto-save (does NOT overwrite manual saves)
- [ ] Create death screen:
  - Brief "Ship Destroyed" overlay with fade to black
  - Show respawn location
  - Show wreckage location with "Your ship remains at [address]"
  - "Continue" button → load at respawn point
- [ ] Implement wreckage recovery:
  - Travel to wreckage address
  - Discover wreckage as a special discovery at that system
  - Visit wreckage: recover ship (damaged hull, partial cargo)
  - Player chooses: swap to recovered ship or keep current ship
  - Recovered ship needs repair before full functionality
  - Wreckage disappears after recovery
- [ ] Implement ship purchase alternative:
  - At any shipyard, player can buy a new ship instead of recovering
  - Wreckage remains but is no longer needed

### Verification
- Player hull reaching 0 triggers death screen
- Respawn occurs at correct safe location with loaner Shuttle
- Manual saves are NOT overwritten on death
- Wreckage exists at death location with correct cargo (0-50% looted)
- Traveling to wreckage allows ship recovery
- Recovered ship is damaged and needs repair
- Can buy a new ship instead of recovering

---

## Phase 6.5 — Encounter Triggers

**Goal:** Wire up combat encounters to the discovery and travel systems so combat happens naturally during gameplay.

### Tasks
- [ ] Hook combat into discovery system:
  - When a hostile discovery is visited (pirate hideout, enemy outpost, ambush), trigger combat
  - Generate enemy appropriate to discovery type and system danger
  - After combat victory, mark discovery as resolved
- [ ] Hook combat into travel system (placeholder for full events in Stage 07):
  - During intra-region travel, each hop has a chance to trigger a combat encounter
  - Chance scales with danger zone (safe: 5%, low-med: 15%, med-high: 25%, extreme: 35%)
  - Generate enemy appropriate to the region
  - Player can fight or flee; combat doesn't consume extra turns (it's part of the hop)
- [ ] Implement patrol checkpoint encounters:
  - In Friendly/Neutral systems, legitimate patrols may scan for contraband
  - If player has contraband in special slots, risk detection
  - Detection → confiscation and fine (not combat, unless player has very low legitimate reputation)
- [ ] Add combat avoidance through fame:
  - Before any combat encounter, check fame differential
  - If player fame 3+ above, enemy flees without combat starting
  - Show brief "Enemy recognized your reputation and fled" message

### Verification
- Visiting a pirate hideout discovery triggers combat
- Intra-region travel can trigger random encounters
- Encounter rate scales with danger zone
- Fame auto-intimidation works for weak enemies
- Patrol checkpoints check for contraband

---

## Phase 6.6 — Repair System

**Goal:** Implement ship repair at repair service discoveries and hubs.

### Tasks
- [ ] Implement repair interaction at repair service discoveries:
  - Show current hull and shield status
  - Show repair cost (based on damage amount, ship class, system type)
  - Hub repair: full repair available, standard pricing
  - Sub-system repair: max 75% repair, 70% of hub pricing
  - Player station Tier 2+: full repair, no cost
- [ ] Apply Engineering skill discount:
  - 1% cost reduction per Engineering level
- [ ] Implement repair execution:
  - Restore hull/shield to selected amount
  - Deduct shards
  - Award Engineering XP
  - Free action (no turn cost)
- [ ] Create repair UI panel:
  - Hull repair slider (how much to repair)
  - Cost display updates as slider moves
  - Shield status (shields auto-recharge, but show current state)
  - "Repair" button

### Verification
- Repair costs scale with damage and ship class
- Sub-system repair caps at 75% and is cheaper
- Engineering skill discount applies
- Repairing awards Engineering XP
- Hull restores to correct amount after repair

---

## Stage 06 Completion Checklist

- [ ] Enemies generate with correct fame scaling and faction behavior
- [ ] All 5 combat actions work (Attack, Defend, Flee, Negotiate, Intimidate)
- [ ] Damage resolution handles shields → hull correctly
- [ ] Enemy AI varies by faction
- [ ] Combat UI shows health bars, actions, combat log
- [ ] Loot awarded on victory
- [ ] Death triggers respawn with loaner ship at safe location
- [ ] Wreckage remains at death location and is recoverable
- [ ] Manual saves preserved on death
- [ ] Discovery-triggered and travel-triggered combat works
- [ ] Fame auto-intimidation skips combat for weak enemies
- [ ] Repair system works at hubs, sub-systems, and stations

**Next Stage:** [07 — Missions & Events](07_missions_and_events.md)
