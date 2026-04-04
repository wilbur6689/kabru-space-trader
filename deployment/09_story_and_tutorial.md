# Stage 09: Story & Tutorial

## Goal
Build the tutorial sequence that teaches new players every core mechanic, and implement the story arc framework that gives the game narrative structure. By the end of this stage, a new player can start the game and be guided through searching, repairing, trading, traveling, and reaching Earth — then continue into the open galaxy with story beats available at hubs.

**Design Docs:** [08_story_and_narrative.md](../docs/08_story_and_narrative.md), [01_core_game_loop.md](../docs/01_core_game_loop.md) (tutorial flow)
**Prerequisites:** Stages 01-08 complete (all gameplay systems functional)
**Outputs:** Tutorial sequence, story arc framework, NPC interactions, narrative events, bulletin board content

---

## Phase 9.1 — Tutorial: Stranded Start

**Goal:** Build the opening sequence where the player is stranded with a broken ship and learns the search and repair mechanics.

### Tasks
- [ ] Create tutorial starting system:
  - Pre-defined Minor system in Q01-R0101 (or nearby safe region)
  - System name: hand-crafted (not procedural)
  - Alignment: Friendly
  - Limited discoveries (3-4 total, hand-placed):
    1. **Wreckage Salvage** (known on arrival) — tutorial teaches "visit" action
    2. **Repair Parts Cache** (discoverable) — tutorial teaches "search" action
    3. **Basic Merchant** (discoverable) — tutorial teaches trading basics
    4. **Delivery Contact** (discoverable) — gives the mission to Earth
- [ ] Implement broken ship state:
  - Starting Shuttle has damaged hull (50% HP)
  - Jump drive disabled until repaired
  - Tutorial guides player through repair
- [ ] Build tutorial overlay system:
  - Semi-transparent instruction panels that point to UI elements
  - Step-by-step prompts that advance on player action
  - Can be dismissed (skip tutorial option)
  - Tracks which tutorial steps have been completed
- [ ] Tutorial Step 1 — "Assess Your Situation":
  - Highlight system overview panel
  - Explain: system name, discovery count, your damaged ship
  - Prompt: "Visit the nearby wreckage to find useful salvage"
- [ ] Tutorial Step 2 — "Visit a Location":
  - Guide player to click the known wreckage discovery
  - Show visit interaction
  - Player receives scrap metal (cargo) from wreckage
  - Explain: visiting locations is a free action (no turn cost)
- [ ] Tutorial Step 3 — "Search for More":
  - Highlight the Search button
  - Explain: searching costs 1 turn, discovers new locations
  - Guide player to search — finds Repair Parts Cache
  - Explain: scanner preview and discovery permanence
- [ ] Tutorial Step 4 — "Repair Your Ship":
  - Guide player to visit repair parts cache
  - Receive repair components
  - Guide player through self-repair interaction (Engineering action)
  - Ship hull restored, jump drive enabled
  - Explain: repair at hubs and stations costs shards, self-repair uses found parts

### Verification
- New game starts at the tutorial system with damaged ship
- Tutorial overlays guide the player through each step
- Player can visit wreckage, search, find repair parts, and fix ship
- Jump drive enables after repair
- Skip tutorial option works and sets up correct state

---

## Phase 9.2 — Tutorial: First Trade and Mission

**Goal:** Teach the player trading basics and give them their first mission to reach Earth.

### Tasks
- [ ] Tutorial Step 5 — "Search Again":
  - Guide player to search — finds Basic Merchant
  - Explain: merchants buy and sell goods
- [ ] Tutorial Step 6 — "Your First Trade":
  - Guide player to visit the merchant
  - Open marketplace with simplified view
  - Explain: buy low, sell high; price colors indicate deals
  - Player sells the scrap metal from wreckage for shards
  - Explain: shards are the galaxy's currency
- [ ] Tutorial Step 7 — "Accept a Mission":
  - Guide player to search — finds Delivery Contact
  - Delivery Contact offers a mission: deliver a package to Earth
  - Explain: missions give shards and reputation, some have deadlines
  - Guide player to accept
  - Package loaded into special cargo slot
- [ ] Tutorial Step 8 — "Navigate to Earth":
  - Highlight the navigation panel
  - Explain the Q-R address system
  - Earth's address provided: Q01-R0101-1000
  - Guide player to input the address
  - Show jump info: distance, hops, drive requirements
  - Explain: jump drive tier limits how far you can travel
  - Execute travel to Earth (within Tier 1 range)

### Verification
- Player can trade at the tutorial merchant
- First sale completes with shard gain visible
- Delivery mission accepts and loads cargo
- Navigation to Earth works within Tier 1 range
- Address system explanation is clear

---

## Phase 9.3 — Tutorial: Arrival at Earth

**Goal:** Complete the tutorial by arriving at Earth, delivering the package, and opening up the galaxy.

### Tasks
- [ ] Create Earth system (Q01-R0101-1000):
  - Pre-defined Major system, Friendly alignment
  - Trade Hub specialization
  - Hand-crafted discoveries:
    - Central Market (known) — full marketplace
    - Spaceport Authority (known) — repair + shipyard
    - Mission Board (known) — standard missions
    - Bulletin Board (known) — news, events, trade intel
    - Additional discoverable locations (6-10 more)
- [ ] Tutorial Step 9 — "Welcome to Earth":
  - Arrival cinematic (text-based narrative, cockpit window visual)
  - Explain: Earth is the central hub, many services available
  - Guide player to deliver the package at the Spaceport
  - Delivery completes: receive shards + reputation
- [ ] Tutorial Step 10 — "The Galaxy Awaits":
  - Brief overview of what's now available:
    - Trade routes between systems
    - Missions for structured goals
    - Exploration to discover new systems
    - Upgrading your ship to reach farther
  - Mention: jump drive upgrades open new regions
  - Mention: pirate-controlled systems can be liberated
  - Main story hook teased (if story content exists)
  - Tutorial flag set: `GameState.is_tutorial_complete = true`
  - All tutorial overlays dismissed
  - Player free to explore
- [ ] Implement tutorial state tracking:
  - Track completed tutorial steps in GameState
  - If player loads a save mid-tutorial, resume at correct step
  - Tutorial completion saves correctly

### Verification
- Arriving at Earth shows welcome sequence
- Package delivery completes the mission with rewards
- Tutorial completion unlocks full gameplay
- Tutorial state persists across save/load
- Player can freely explore after tutorial ends

---

## Phase 9.4 — Story Arc Framework

**Goal:** Build the framework for the main story arc (Acts 1-3) even if the full narrative content is not yet written. The system should support story progression, branching choices, and gated content.

### Tasks
- [ ] Create `scripts/systems/story_manager.gd`:
  - Track story progress via flags in GameState.story_flags
  - Define story beats as data (JSON or Resource):
    ```
    story_beat:
      id: "act1_inciting_incident"
      trigger: {type: "visit", address: "Q01-R0101-1000", flag_required: "tutorial_complete"}
      content: {dialog, choices, consequences}
      next_beats: ["act1_choice_a", "act1_choice_b"]
    ```
  - Support multiple trigger types: visit a system, complete a mission, reach fame level, clear a system
- [ ] Implement story beat triggers:
  - On `system_entered`, check if any story beats trigger at this address
  - On `mission_completed`, check for story-linked missions
  - On fame level change, check for fame-gated story events
  - If trigger conditions met, show story content
- [ ] Implement story dialog system:
  - NPC dialog with portrait and text
  - Choice buttons for branching decisions
  - Consequences applied to GameState (reputation, shards, flags)
  - Can trigger follow-up missions
- [ ] Implement story flag system:
  - Set/check arbitrary flags: `story_flags["act1_chose_alliance"] = true`
  - Gates future content based on past choices
  - Saved with GameState
- [ ] Create placeholder story content:
  - Act 1 hook at Earth after tutorial (brief teaser of the main conflict)
  - 2-3 placeholder story beats to test the system
  - Placeholder NPC with dialog tree

### Verification
- Story beats trigger at correct locations with correct conditions
- Dialog system shows NPC conversations with choices
- Choices set flags that gate future content
- Story progress saves and loads correctly
- Placeholder content plays through without errors

---

## Phase 9.5 — NPC Interaction System

**Goal:** Build the NPC system for story characters, quest givers, and information sources.

### Tasks
- [ ] Create NPC data structure:
  - Name, portrait (placeholder), faction, location address
  - Dialog trees (branching conversations)
  - Available services (missions, intel, story progression)
  - Relationship state (affected by player reputation and choices)
- [ ] Implement NPC discoveries:
  - NPCs can be found via searching (special discovery type)
  - Some NPCs are permanent at hubs (known on arrival)
  - Some NPCs are one-time encounters
- [ ] Implement NPC dialog UI:
  - Portrait + name on left
  - Dialog text with typing effect (optional)
  - Response buttons for player choices
  - Can branch into: more dialog, mission offer, shop, story beat, farewell
- [ ] Implement NPC intel sharing:
  - NPCs can reveal addresses (added to known addresses)
  - NPCs can share price information (added to journal)
  - NPCs can warn about dangers (events, pirate activity)
  - Intel quality based on NPC type and player reputation
- [ ] Implement fame-gated NPC access:
  - Some NPCs only talk to players above certain fame levels
  - Show "They don't seem interested in talking to someone of your reputation" for low fame
  - Higher fame unlocks exclusive NPC contacts with better intel and missions

### Verification
- NPCs appear as discoveries and at hub systems
- Dialog trees branch correctly based on choices
- Intel from NPCs adds addresses and price data to journal
- Fame-gated NPCs correctly refuse low-fame players
- NPC relationship state persists across visits

---

## Phase 9.6 — Bulletin Board Content

**Goal:** Populate bulletin boards with dynamic content that makes hubs feel alive and provides trading intelligence.

### Tasks
- [ ] Create `scenes/ui/bulletin_board.tscn`:
  - Display at hub Bulletin Board discoveries
  - Tabbed interface: News, Trade Intel, Warnings, Community
- [ ] **News tab**:
  - Active world events affecting nearby regions
  - Recent pirate system clearances
  - Faction relationship changes
  - Generated from current GameState
- [ ] **Trade Intel tab**:
  - Price reports from nearby systems (may be slightly outdated)
  - "System X has high demand for [good]" hints
  - Trade route suggestions from NPC traders
  - Costs a small fee for detailed reports (optional)
- [ ] **Warnings tab**:
  - Pirate activity reports for nearby regions
  - Known dangers in specific systems
  - Bounty postings for pirate targets
  - Active pirate surges or faction conflicts
- [ ] **Community tab**:
  - Flavor text posts from fictional NPCs
  - Lore snippets about the galaxy
  - Rumors that hint at rare discoveries or hidden content
  - Story-relevant posts that appear based on story progress
- [ ] Implement content generation:
  - Regenerate bulletin board content on each visit
  - Content reflects current game state (not static)
  - Mix of procedural and hand-crafted entries

### Verification
- Bulletin board shows relevant, current content
- Trade intel provides useful (but not perfect) trading information
- Warnings reflect actual pirate activity in nearby regions
- Content changes between visits as game state evolves
- Flavor text adds atmosphere without gameplay impact

---

## Stage 09 Completion Checklist

- [ ] Tutorial teaches: visit, search, repair, trade, missions, navigation
- [ ] Tutorial flows smoothly from stranded start to Earth arrival
- [ ] Tutorial can be skipped
- [ ] Tutorial state saves/loads correctly mid-sequence
- [ ] Earth hub is fully set up with hand-crafted content
- [ ] Story arc framework supports triggers, flags, branching, and consequences
- [ ] Story beats trigger at correct conditions
- [ ] NPC dialog system works with branching conversations
- [ ] NPCs share intel, offer missions, and gate by fame
- [ ] Bulletin boards display dynamic, contextual content
- [ ] Placeholder story content plays through Acts 1 beginning

**Next Stage:** [10 — Polish & Release](10_polish_and_release.md)
