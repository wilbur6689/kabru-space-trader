# Stage 07: Missions & Events

## Goal
Implement the mission system (delivery, bounty, smuggling, rescue, exploration, clearance, investigation) and the full travel event system (pass-thru encounters, environmental hazards, dynamic world events). By the end of this stage the player has structured goals to pursue and the galaxy feels alive with random events during travel.

**Design Docs:** [05d_missions.md](../docs/05d_missions.md), [05e_events_and_hazards.md](../docs/05e_events_and_hazards.md), [05_challenges.md](../docs/05_challenges.md)
**Prerequisites:** Stage 06 complete (combat system), Stage 04 complete (economy, cargo)
**Outputs:** MissionManager, full mission types, travel event system, hazard encounters, world event generation

---

## Phase 7.1 — MissionManager System

**Goal:** Create the core mission system that generates, tracks, and resolves missions.

### Tasks
- [ ] Create `scripts/systems/mission_manager.gd`
- [ ] Implement mission generation:
  - `generate_missions(system_data: SystemData, pilot: Pilot) -> Array[Mission]`:
  - Mission count: 2-5 based on system population tier
  - Types weighted by system alignment and hub type:
    - Trade Hub: more delivery missions
    - Military Hub: more bounty and clearance missions
    - Outlaw Hub: more smuggling missions
    - Neutral/generic: balanced mix
  - Difficulty and reward scaled by Manhattan distance
  - Fame level gates premium mission tiers
- [ ] Implement mission slot tracking:
  - Active missions limited by fame-gated slot count (3 at start, up to 10)
  - Can't accept missions beyond slot limit
  - Abandoned missions free up a slot
- [ ] Implement `accept_mission(mission: Mission) -> bool`:
  - Check slot availability
  - Load any required cargo into special slots
  - Set mission status to "active"
  - Start deadline countdown (if applicable)
  - Add destination to known addresses
  - Emit `EventBus.mission_accepted`
- [ ] Implement deadline management:
  - On each `turn_ended`, decrement turns_remaining for active missions with deadlines
  - At 0 remaining: auto-fail mission
  - Emit `EventBus.mission_failed`
  - Apply reputation penalty
- [ ] Implement `complete_mission(mission_id: String)`:
  - Award shards, reputation, XP
  - Remove from active missions
  - Add to completed missions list
  - Emit `EventBus.mission_completed`
- [ ] Implement `abandon_mission(mission_id: String)`:
  - Free mission slot
  - Minor reputation penalty
  - Remove any mission cargo from special slots

### Verification
- Missions generate at mission board discoveries
- Can accept missions up to slot limit
- Deadlines count down each turn
- Failed missions apply reputation penalty
- Completed missions award correct rewards

---

## Phase 7.2 — Mission Types Implementation

**Goal:** Implement each of the 7 mission types with their unique objectives and completion conditions.

### Tasks
- [ ] **Delivery Mission**:
  - Cargo loaded into special slot on accept
  - Travel to destination system
  - Visit designated delivery point (discovery)
  - Complete: cargo removed, rewards given
  - Has deadline (turn limit)
  - Reputation with issuing faction

- [ ] **Smuggling Mission**:
  - Contraband loaded into special slot on accept
  - Travel to destination — risk patrol checkpoint events
  - Deliver to black market contact at destination
  - Delivery outcome rolled (50% exact, 25% shortage, 15% different goods, 5% bust, 5% bonus)
  - Awards shards + pirate fame
  - Risk: bust results in fine and reputation loss

- [ ] **Bounty Mission**:
  - Target NPC at a specific system
  - Travel to target system
  - Search to discover target's location
  - Combat encounter with target (may be tougher than standard enemies)
  - Complete on target defeat
  - No deadline (open-ended)

- [ ] **Rescue Mission**:
  - NPC trapped at hostile/pirate system
  - Travel to system, search to find NPC location
  - May require clearing a blocking threat first
  - Visit to rescue — NPC joins as temporary passenger (special slot)
  - Return NPC to origin system
  - Has deadline

- [ ] **Exploration Mission**:
  - Chart a list of addresses (visit X systems in a region)
  - Each visit checks off an address
  - Rewards bonus for discovering all locations in target systems
  - No deadline
  - Awards Exploration XP bonus

- [ ] **Clearance Mission**:
  - Liberate a specific pirate-controlled system
  - Requires pirate clearing mechanic (Stage 08)
  - For now, stub as "travel to system and complete clearance"
  - No deadline
  - Major reputation reward

- [ ] **Investigation Mission**:
  - Gather intel from multiple systems
  - Visit specific discoveries across 2-3 systems
  - Each visit reveals a clue
  - Return to mission giver with all clues
  - Has deadline
  - Awards Exploration + Negotiation XP

### Verification
- Each mission type can be accepted, tracked, and completed
- Delivery and rescue missions have working deadlines
- Smuggling missions trigger correct outcome rolls
- Bounty targets spawn at the correct system
- Investigation missions track clue gathering across systems

---

## Phase 7.3 — Mission Board UI

**Goal:** Build the UI for browsing and accepting missions at mission board discoveries.

### Tasks
- [ ] Create `scenes/ui/mission_board.tscn` (displays in left monitor zone)
- [ ] Show available missions list:
  - Mission title, type icon, difficulty indicator
  - Reward (shards, reputation preview)
  - Destination address and distance
  - Deadline (if applicable)
  - Required fame level (greyed out if too low)
- [ ] Mission detail panel on selection:
  - Full description
  - Objective breakdown
  - Cargo requirements (delivery/smuggling)
  - Reputation effects preview
  - "Accept" button (disabled if slots full or fame too low)
- [ ] Show active missions panel:
  - List current missions with status, deadline, progress
  - Click for details
  - "Abandon" button with confirmation
- [ ] Show mission slot counter: "Active: X / Y"
- [ ] Implement mission log in pilot journal:
  - Completed missions with dates and rewards
  - Failed missions with reason

### Verification
- Mission board shows generated missions for current system
- Can browse details and accept missions
- Active mission panel shows correct progress and deadlines
- Slot counter updates on accept/complete/abandon
- Journal tracks mission history

---

## Phase 7.4 — Travel Event System

**Goal:** Replace the placeholder pass-thru events with a full event system that triggers during intra-region travel.

### Tasks
- [ ] Create `scripts/systems/encounter_manager.gd`
- [ ] Implement event trigger on each intra-region travel hop:
  - Base chance scales with danger zone:
    - Safe: 10% per hop
    - Low-Medium: 20% per hop
    - Medium-High: 30% per hop
    - Extreme: 40% per hop
  - Piloting skill reduces chance slightly (-1% per level)
- [ ] Define travel event types and implement each:

  **Combat Events:**
  - **Pirate Ambush**: Standard combat encounter, faction based on region
  - **Bounty Hunter**: Appears if player has high pirate fame, tough single enemy
  - **Turf War**: Two faction enemies fighting; player can join either side, flee, or wait

  **Non-Combat Events:**
  - **Distress Signal**: Choose to help (costs 1 turn, chance of reward or trap) or ignore
  - **Derelict Ship**: Salvage for goods/components or leave (risk of trap)
  - **Merchant Convoy**: Random trader with unique goods at decent prices
  - **Asteroid Field**: Piloting check — success avoids damage, failure takes hull damage
  - **Space Storm**: Navigation check — success passes through, failure loses 1 extra turn
  - **Patrol Checkpoint**: Scanned for contraband — clean ships pass freely

  **Story Events** (placeholder for Stage 09):
  - Hook for story-triggered travel events

- [ ] Implement event resolution UI:
  - Popup during travel showing event description
  - Choice buttons (help/ignore, salvage/leave, fight/flee)
  - Outcome text
  - Rewards or consequences displayed
  - "Continue Journey" button

### Verification
- Events trigger during intra-region travel at correct rates
- Each event type resolves with appropriate outcomes
- Piloting skill reduces encounter rate
- Patrol checkpoints detect contraband
- Events don't trigger during inter-region jumps

---

## Phase 7.5 — Environmental Hazards

**Goal:** Implement environmental dangers that exist as system discoveries and travel obstacles.

### Tasks
- [ ] Implement hazard discovery types:
  - **Unstable Ruins**: Exploration check to safely loot, failure = hull damage
  - **Contamination Zone**: Engineering check to extract resources, failure = component damage
  - **Hostile Wildlife**: Combat with environmental creature (non-faction)
  - **Trap Mechanism**: Piloting check to avoid, failure = cargo loss
  - **Radiation Zone**: Timer-based — each turn spent in system causes minor hull damage
- [ ] Implement hazard resolution:
  - Skill check against difficulty rating
  - Difficulty scales with Manhattan distance
  - Success: reward (loot, resources, XP)
  - Failure: consequence (damage, loss, extra turns)
- [ ] Show hazard info:
  - Risk level indicator in discovery list
  - Skill check preview (which skill, approximate difficulty)
  - Reward preview for success

### Verification
- Hazard discoveries appear in hostile and dead systems
- Skill checks use the correct skill
- Difficulty scales with distance
- Success gives appropriate rewards
- Failure applies consequences without being game-ending

---

## Phase 7.6 — World Events (Dynamic Galaxy)

**Goal:** Implement dynamic world events that affect regions or systems, making the galaxy feel alive.

### Tasks
- [ ] Implement world event generation:
  - Each turn, small chance of new world event (1-2%)
  - Events affect a region or specific system
  - Duration: 10-50 turns
- [ ] Define world event types:
  - **Trade Boom**: Region-wide price increase for specific goods
  - **Pirate Surge**: Increased pirate activity in a region (more combat encounters)
  - **Faction Conflict**: Two factions clashing, border regions destabilized
  - **Supply Shortage**: Specific good scarce across multiple systems
  - **Discovery Rush**: Bonus discoveries temporarily appear in a region
  - **Pirate Retreat**: Reduced pirate activity after player clears nearby systems
- [ ] Wire events into existing systems:
  - Economy events → EconomyManager event modifiers (built in Phase 4.6)
  - Combat events → EncounterManager rate modifiers
  - Discovery events → SearchManager bonus discoveries
- [ ] Show active world events:
  - Bulletin board at hubs displays current events
  - Star map indicators for affected regions
  - Journal notification when new events start/end
- [ ] Store active events in GameState for save/load

### Verification
- World events generate over time
- Trade events visibly affect prices
- Pirate surge increases encounter rates
- Bulletin board displays current events
- Events expire after duration
- Events save and load correctly

---

## Stage 07 Completion Checklist

- [ ] MissionManager generates, tracks, and resolves all 7 mission types
- [ ] Mission board UI allows browsing, accepting, and abandoning missions
- [ ] Mission slots gated by fame level
- [ ] Deadlines count down and auto-fail expired missions
- [ ] Travel events trigger during intra-region travel at correct rates
- [ ] All travel event types resolve with appropriate outcomes
- [ ] Environmental hazards use skill checks with consequences
- [ ] World events dynamically affect economy, encounters, and discoveries
- [ ] Bulletin board shows active world events
- [ ] All mission and event data saves/loads correctly

**Next Stage:** [08 — Advanced Systems](08_advanced_systems.md)
