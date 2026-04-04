# Stage 05: Pilot & Progression

## Goal
Implement the full pilot system: character creation, skill XP and leveling, fame points, faction reputation, and the journal. By the end of this stage, the player earns XP from their actions, levels up skills, earns fame, and sees their reputation change based on their choices.

**Design Docs:** [04_pilot.md](../docs/04_pilot.md), [12_balancing.md](../docs/12_balancing.md)
**Prerequisites:** Stage 04 complete (economy — so trading actions can award XP)
**Outputs:** Pilot creation screen, XP system, fame system, reputation tracking, pilot journal UI

---

## Phase 5.1 — Pilot Creation Screen

**Goal:** Build the new game character creation flow where the player names their pilot and chooses a background.

### Tasks
- [ ] Create `scenes/ui/pilot_creation.tscn`
- [ ] Implement name input:
  - Text field with character limit
  - Validation (no empty names)
- [ ] Implement background selection:
  - Display 4-6 background options with descriptions:
    - **Merchant**: +2 Trading starting XP bonus
    - **Spacer**: +2 Piloting starting XP bonus
    - **Engineer**: +2 Engineering starting XP bonus
    - **Diplomat**: +2 Negotiation starting XP bonus
    - **Bounty Hunter**: +2 Combat starting XP bonus
    - **Scout**: +2 Exploration starting XP bonus
  - Show tooltip explaining the starting bonus
  - Visual portrait placeholder per background
- [ ] On confirm:
  - Call `GameState.new_game(name, background)`
  - Apply background skill bonus
  - Set starting shards (2,000)
  - Set starting ship (Shuttle, all Tier 1 components)
  - Set starting location (tutorial Minor system)
  - Transition to cockpit scene

### Verification
- Can create a pilot with any name and background
- Background bonus applies to the correct skill
- Starting state is correct (2,000 shards, Shuttle, Tier 1 components)
- Transitions to cockpit after creation

---

## Phase 5.2 — Skill XP System

**Goal:** Implement the usage-based XP system where every action earns XP in the relevant skill.

### Tasks
- [ ] Define XP thresholds per skill level:
  ```
  Level 1:  150 XP
  Level 2:  500 XP (cumulative)
  Level 3:  1,200 XP
  Level 4:  2,500 XP
  Level 5:  4,500 XP
  Level 6:  7,500 XP
  Level 7:  11,500 XP
  Level 8:  17,000 XP
  Level 9:  23,000 XP
  Level 10: 30,000 XP
  ```
- [ ] Define XP rewards per action:
  - **Trading**: XP per trade transaction (scaled by trade value)
  - **Piloting**: XP per jump/travel
  - **Engineering**: XP per repair action
  - **Negotiation**: XP per successful haggle, negotiation in combat, bribe
  - **Combat**: XP per combat encounter (scaled by enemy fame)
  - **Exploration**: XP per new discovery, new system visited
- [ ] Implement `award_xp(skill_name: String, amount: int)` in Pilot:
  - Add XP to skill
  - Check for level up
  - If leveled up, emit `EventBus.skill_leveled_up(skill_name, new_level)`
  - Check for fame point earned (every 5 combined levels)
- [ ] Hook XP awards into existing systems:
  - `EventBus.trade_completed` → award Trading XP
  - `EventBus.travel_completed` → award Piloting XP
  - `EventBus.discovery_found` → award Exploration XP
  - `EventBus.system_entered` (first visit) → award Exploration XP

### Verification
- Trading at a marketplace awards Trading XP
- Traveling awards Piloting XP
- Discovering a location awards Exploration XP
- Skills level up at correct thresholds
- Level-up notification appears

---

## Phase 5.3 — Fame System

**Goal:** Implement the meta-progression fame system that unlocks features and affects NPC interactions.

### Tasks
- [ ] Implement fame point calculation:
  - 1 fame point earned per 5 combined skill levels across all skills
  - Maximum 12 fame points (all 6 skills at level 10 = 60 levels / 5 = 12)
- [ ] Implement fame point allocation:
  - On earning a fame point, player chooses which skill to boost
  - Each allocated point gives +3% bonus to that skill's effectiveness
  - Points are permanent once allocated
- [ ] Create fame allocation UI:
  - Popup on fame point earned
  - Show all 6 skills with current level, current fame points allocated, and bonus
  - Click to allocate
  - Confirm button
- [ ] Implement fame-gated unlocks:
  - Fame 1: Intimidation becomes available (can attempt on enemies within 2 fame levels)
  - Fame 3: +1 mission slot (4 total)
  - Fame 5: +1 mission slot (5 total), better merchant prices
  - Fame 7: +2 mission slots (7 total), exclusive NPC access
  - Fame 10: +2 mission slots (9 total)
  - Fame 12: +1 mission slot (10 total), maximum NPC access
- [ ] Display fame level prominently in pilot status

### Verification
- Fame point earned after every 5 combined skill levels
- Allocation popup appears and allows skill selection
- +3% bonus applies to chosen skill
- Mission slot count increases at correct fame thresholds

---

## Phase 5.4 — Faction Reputation System

**Goal:** Track player reputation with legitimate factions and pirate factions separately.

### Tasks
- [ ] Implement faction reputation tracking in Pilot:
  - Legitimate factions: Dictionary of faction_name → reputation tier (0-4)
  - Tiers: 0 = Hated, 1 = Unfriendly, 2 = Neutral, 3 = Friendly, 4 = Allied
  - Starting reputation: Neutral (2) with all legitimate factions
- [ ] Implement pirate fame tracking:
  - Per pirate faction (Silk Hand, Phantom Circuit, Rust Collective, Void Reavers)
  - Separate scale from legitimate reputation
  - Increased by: black market deliveries, pirate missions, sparing pirates
  - Decreased by: clearing pirate systems, bounty hunting pirates
- [ ] Implement reputation effects:
  - Price modifier at faction-controlled systems (better prices at higher rep)
  - Mission availability (higher rep = better missions)
  - NPC dialog changes (placeholder)
  - High pirate fame: pirate patrols leave you alone, legitimate patrols scan you more
- [ ] Implement `change_reputation(faction, amount)`:
  - Clamp to 0-4 range
  - Emit `EventBus.reputation_changed(faction, old_tier, new_tier)`
  - Show notification on tier change

### Verification
- Reputation starts at Neutral for all legitimate factions
- Pirate fame starts at 0 for all pirate factions
- Reputation changes emit correct signals
- Price modifiers apply based on reputation tier

---

## Phase 5.5 — Pilot Status UI

**Goal:** Build the pilot status screen showing skills, fame, reputation, and statistics.

### Tasks
- [ ] Create `scenes/ui/pilot_status.tscn` (displays in left monitor zone)
- [ ] **Skills panel**:
  - Show all 6 skills with: level, XP bar (progress to next level), fame points allocated, effective bonus
  - Highlight recently leveled skills
- [ ] **Fame panel**:
  - Current fame level (large display)
  - Unallocated fame points indicator
  - Allocate button (if points available)
  - List of fame-gated unlocks (achieved and upcoming)
- [ ] **Reputation panel**:
  - Legitimate faction standings with tier names and bar
  - Pirate fame per faction with current level
  - Color-coded: green for Friendly/Allied, yellow for Neutral, red for Unfriendly/Hated
- [ ] **Statistics panel**:
  - Systems visited
  - Discoveries found
  - Total shards earned
  - Total trades completed
  - Turns played
  - Current ship name and class
- [ ] Make accessible from cockpit left monitor zone

### Verification
- All skills display with correct levels and XP progress
- Fame points can be allocated from this screen
- Reputation tiers display correctly for all factions
- Statistics update as the player acts

---

## Phase 5.6 — Skill Effects Integration

**Goal:** Wire up skill levels to actually affect gameplay mechanics that already exist.

### Tasks
- [ ] **Trading skill effects**:
  - Better base prices at merchants (0.5% per level)
  - More price info shown in scanner preview
- [ ] **Piloting skill effects**:
  - Reduced chance of negative pass-thru events during travel (placeholder prep for Stage 07)
  - Slightly better flee chance in combat (placeholder prep for Stage 06)
- [ ] **Engineering skill effects**:
  - Cheaper repair costs (1% discount per level)
  - Small chance of free repair on docking (1% per level)
- [ ] **Negotiation skill effects**:
  - Better haggle success rate (+5% per level)
  - Higher haggle discount amount
  - Better bribe success rate (placeholder)
- [ ] **Combat skill effects** (placeholder for Stage 06):
  - Stub the damage bonus calculation (+3% per level)
  - Stub accuracy bonus
- [ ] **Exploration skill effects**:
  - Better scanner preview detail
  - Slightly better discovery quality weighting

### Verification
- Trading at higher levels gives visibly better prices
- Haggling with higher Negotiation has higher success rate
- Repair costs decrease with Engineering level
- Effects are subtle but verifiable through testing

---

## Stage 05 Completion Checklist

- [ ] Pilot creation works with name, background, and correct starting state
- [ ] XP awarded for all tracked actions (trading, traveling, discovering)
- [ ] Skills level up at correct thresholds with notifications
- [ ] Fame points earned every 5 combined levels, allocatable to skills
- [ ] Fame unlocks trigger at correct thresholds (intimidation, mission slots, prices)
- [ ] Faction reputation tracks independently per faction
- [ ] Pirate fame tracks per pirate faction
- [ ] Pilot status UI shows skills, fame, reputation, and statistics
- [ ] Skill effects integrate with existing systems (trading, haggling, repairs)
- [ ] All progression data saves and loads correctly

**Next Stage:** [06 — Combat](06_combat.md)
