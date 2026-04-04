# Stage 08: Advanced Systems

## Goal
Implement the mid-to-late game systems: pirate-controlled system clearing, player station construction and management, the black market economy, the bribe system, ship purchasing and ownership, and bounty hunters. These systems add depth and long-term goals for experienced players.

**Design Docs:** [05c_hostile_systems.md](../docs/05c_hostile_systems.md), [02d_player_stations.md](../docs/02d_player_stations.md), [07_economy_and_trading.md](../docs/07_economy_and_trading.md) (black market), [03_ship.md](../docs/03_ship.md) (ship ownership)
**Prerequisites:** Stage 07 complete (missions, combat encounters, events)
**Outputs:** Pirate clearing mechanic, station building, black market, ship dealership, bribe system, bounty hunter spawning

---

## Phase 8.1 — Pirate-Controlled System Clearing

**Goal:** Implement the full system liberation mechanic where players discover and destroy pirate outposts, then defeat the faction Warlord to convert a pirate system to friendly.

### Tasks
- [ ] Implement outpost tracking per pirate-controlled system:
  - Required outposts based on Manhattan distance:
    - Distance 1-5: 2 outposts
    - Distance 6-10: 3 outposts
    - Distance 11-15: 4 outposts
    - Distance 16+: 5 outposts
  - Outposts are specific hostile discovery types in pirate-controlled systems
  - Track cleared count per system in DiscoveryDB
- [ ] Implement outpost combat:
  - Outpost discoveries trigger combat when visited
  - Enemy type: Gang Enforcer or Pirate Captain (faction-specific)
  - Multiple enemies per outpost at higher distances
  - On victory: outpost marked as cleared
- [ ] Implement boss arrival trigger:
  - When all required outposts cleared, Warlord arrival triggers
  - Warning message: "The [Faction] Warlord has been alerted. They will arrive in 2-3 turns."
  - Player can leave to prepare (repair, restock) or stay
  - Warlord timer tracked in GameState
- [ ] Implement Warlord encounter:
  - When player is in the system and timer reaches 0, Warlord arrives
  - Boss combat encounter:
    - Warlord fame = 8 + floor(manhattan_distance / 2.5)
    - Higher stats than standard enemies
    - Faction-specific boss behavior:
      - Silk Hand Warlord: offers bribe mid-combat, high negotiation resistance
      - Phantom Circuit Warlord: changes tactics mid-fight, unpredictable
      - Rust Collective Warlord: massive hull, never flees, steady damage
      - Void Reavers Warlord: extreme damage, berserks at low health
  - Cannot intimidate Warlords (regardless of fame)
- [ ] Implement system conversion on victory:
  - System alignment changes from Pirate-Controlled to Friendly
  - New discoveries generated (merchants, services replace outposts)
  - Trade routes open to this system
  - Major reputation gain with legitimate factions
  - Pirate fame decreases with that faction
  - Region destabilization: nearby pirate systems may increase patrols temporarily
  - Emit `EventBus.system_cleared(address, faction)`
- [ ] Handle player leaving during boss timer:
  - If player leaves, boss timer pauses
  - On return, timer resumes
  - If player takes too long (50+ turns), faction rebuilds 1 outpost

### Verification
- Outpost count matches Manhattan distance formula
- Clearing all outposts triggers boss warning
- Boss arrives after timer expires
- Warlord is significantly harder than standard enemies
- System converts to Friendly on Warlord defeat
- Reputation effects apply correctly
- Faction rebuild triggers if player delays too long

---

## Phase 8.2 — Ship Dealership and Ownership

**Goal:** Implement the full ship purchasing, selling, and multi-ship ownership system.

### Tasks
- [ ] Create `scenes/ui/shipyard.tscn` (displays in left monitor zone)
- [ ] Implement ship catalog:
  - Available at Shipyard Hub discoveries
  - Show all 19 standard ship classes (Shuttle + 6 classes × 3 sizes)
  - Display per ship: name, class, size, base stats, price, cargo capacity
  - Prices from balancing doc (500 Shuttle → 500,000 Large Battleship)
- [ ] Implement black market ship availability:
  - 6 black market variants only at Outlaw Hub discoveries
  - Premium stats, unique names
  - Higher prices (up to 1,000,000 for BM Battleship)
- [ ] Implement ship purchase:
  - Deduct shards
  - New ship becomes active, old ship stored at current hub
  - Components transfer option: move upgrades from old ship to new (or keep on old)
  - Emit `EventBus.ship_purchased`
- [ ] Implement ship selling:
  - Sell price = 60% of base price + component value
  - Ship removed from ownership
  - Shards credited
- [ ] Implement multi-ship ownership (max 5 ships):
  - Store ships at hub systems (1 per hub)
  - Display stored ships panel with location
  - Swap active ship when visiting a hub with a stored ship
- [ ] Implement ship transfer service:
  - Request a stored ship be delivered to current hub
  - Fee based on distance between hubs
  - Delivery delay: 5-15 turns based on distance
  - Track in-transit ships in GameState
- [ ] Implement component upgrade purchasing:
  - Available at repair and shipyard discoveries
  - Show all 8 component types with tiers 1-5
  - Display stat changes per tier
  - Prices from balancing doc (200-100,000 per component per tier)
  - Install immediately on purchase

### Verification
- Can browse and purchase ships at shipyards
- Can sell ships for correct price
- Can own up to 5 ships, stored at different hubs
- Can swap ships when visiting a hub with a stored ship
- Ship transfer works with correct delay and fee
- Components upgrade correctly with stat changes

---

## Phase 8.3 — Player Station Construction

**Goal:** Implement the station building system where players invest in personal infrastructure across the galaxy.

### Tasks
- [ ] Implement Station Construction Service discovery:
  - Found via searching at certain system types (more common at Trade/Tech hubs)
  - Interaction opens station building UI
- [ ] Create `scenes/ui/station_builder.tscn`:
  - Address input for station location (must be empty, valid address)
  - Show cost: 25,000 shards base × distance modifier
  - Show estimated delivery time (turns, scaled by distance)
  - Confirm button
- [ ] Implement station construction:
  - Deduct shards
  - Create pending station in GameState
  - Station "under construction" for delivery period
  - On completion: station appears at address, notification sent
  - Station starts at Tier 1
- [ ] Implement station limits:
  - Max 2 stations per quadrant, 12 total galaxy-wide
  - Show current station count in builder UI
- [ ] Implement station interaction:
  - When player visits station address, show station management UI
  - **Tier 1**: Storage only
    - Deposit/withdraw cargo from unlimited storage
    - No pirate raid risk
  - **Tier 2**: Storage + repair
    - Upgrade cost: 50,000 shards
    - Full repair available (free)
    - Passive pirate raids begin (notification of small cargo losses)
  - **Tier 3**: Storage + repair + blueprint crafting + respawn
    - Upgrade cost: 100,000 shards
    - Blueprint crafting (placeholder — craft system is deferred design)
    - Becomes a valid respawn point on death
    - Active pirate raids (warning + timer, must defend or lose tier)
- [ ] Implement pirate raid system:
  - **Passive raids (Tier 2)**:
    - Random chance per turn (scaled by danger zone)
    - Notification: "[Station] raided — X shards worth of goods lost"
    - Small cargo loss from storage
  - **Active raids (Tier 3)**:
    - Less frequent but more severe
    - Warning: "Pirate fleet approaching [Station]. Arrive within X turns to defend."
    - If player arrives: combat encounter with pirate fleet
    - If player doesn't arrive: station downgrades to Tier 2, significant cargo loss
    - Stations never destroyed, only downgraded
- [ ] Create station management UI:
  - Station name, address, tier
  - Storage inventory with deposit/withdraw
  - Repair button (Tier 2+)
  - Upgrade button with cost
  - Raid status and warning

### Verification
- Can build stations at empty addresses
- Construction completes after correct delay
- Station limits enforced (2 per quadrant, 12 total)
- Tier upgrades work with correct costs
- Storage deposit/withdraw works
- Passive raids cause small losses at Tier 2
- Active raids require defense at Tier 3
- Failed defense downgrades station (never destroys)
- Tier 3 stations work as respawn points

---

## Phase 8.4 — Black Market System

**Goal:** Implement the full underground economy with black market contacts, contraband deliveries, and the risk/reward balance.

### Tasks
- [ ] Implement black market contact discovery:
  - Rare discovery type (more common in Pirate-Controlled and Neutral systems)
  - Also discoverable via NPC referral (high pirate fame) or pirate mission reward
  - Persistent — once found, always available at that system
- [ ] Create `scenes/ui/black_market.tscn`:
  - Show available contraband deliveries
  - Each delivery: goods description, destination address, reward, risk level
  - Accept button — loads contraband into special cargo slot
- [ ] Implement contraband delivery:
  - Contraband occupies a special slot (not regular cargo)
  - During travel, patrol checkpoints scan for contraband
  - Detection chance based on patrol thoroughness and player's pirate fame
  - If detected: confiscation + fine (or bribe attempt)
- [ ] Implement delivery outcomes (rolled on arrival):
  - **Exact Delivery (50%)**: Full reward as promised
  - **Shortage (25%)**: Partial goods, reduced reward (60-80%)
  - **Different Goods (15%)**: Wrong product, flat consolation payment
  - **Undercover Bust (5%)**: Ambush — combat encounter with patrol, contraband confiscated, reputation hit
  - **Rare Bonus (5%)**: Extra goods, premium reward (150-200%)
- [ ] Implement pirate fame rewards:
  - Each delivery increases pirate fame with the relevant faction
  - Higher pirate fame → better delivery offers, higher rewards
  - Higher pirate fame → pirates leave you alone, but legitimate patrols target you more
- [ ] Implement risk escalation:
  - High pirate fame → bounty hunters may spawn during travel
  - Very high pirate fame → legitimate faction NPCs may refuse service
  - Reputation with legitimate factions slowly decreases

### Verification
- Black market contacts generate correct delivery offers
- Contraband occupies special slot correctly
- Patrol checkpoints detect contraband with correct probability
- Delivery outcomes match defined percentages over many deliveries
- Pirate fame increases with deliveries
- Risk escalation is noticeable at high pirate fame

---

## Phase 8.5 — Bribe System

**Goal:** Implement the bribe mechanic for when players are caught by authorities.

### Tasks
- [ ] Implement bribe opportunity:
  - Triggers when caught at patrol checkpoint with contraband
  - Also available when caught during smuggling missions
  - Not available during undercover bust (mission outcome)
- [ ] Implement bribe resolution:
  ```
  success_chance = hidden_base_percentage 
    + (10% × negotiation_level_above_npc) 
    + (1% × shards_offered / 10)
  ```
  - Player inputs bribe amount (shard slider)
  - Higher offer = higher success chance
  - Show approximate success chance (fuzzy, not exact)
- [ ] Implement outcomes:
  - **Success**: Contraband ignored, player continues, small reputation hit with that faction
  - **Failure**: Fine imposed (go to regional hub court), contraband confiscated, larger reputation hit
  - Fine amount: 2-5× the bribe offered
- [ ] Award Negotiation XP for bribe attempts (success or failure)

### Verification
- Bribe option appears when caught with contraband
- Higher shard offers increase success chance visibly
- Negotiation skill meaningfully affects outcome
- Failed bribes result in larger financial penalty than the bribe itself
- XP awarded regardless of outcome

---

## Phase 8.6 — Bounty Hunter System

**Goal:** Implement bounty hunters that pursue players with high pirate fame or active bounties.

### Tasks
- [ ] Implement bounty hunter spawning:
  - Triggered by high pirate fame (threshold: pirate fame 4+)
  - Also triggered by failing bribe attempts (active bounty placed)
  - Bounty hunters appear as travel encounters (higher priority than standard pirates)
- [ ] Implement bounty hunter behavior:
  - Fame level: 5 + pirate_fame_level
  - Relentless: won't accept negotiation
  - Can be intimidated only at very high player fame
  - Better loot than standard enemies (bounty hunter gear)
- [ ] Implement active bounty tracking:
  - Bounty placed by legitimate faction when caught
  - Bounty hunters spawn periodically until bounty cleared
  - Clear bounty by: paying fine at hub, completing mission for that faction, or time passing (50+ turns)
- [ ] Show bounty status in pilot status UI:
  - Active bounties listed with issuing faction
  - "Wanted" indicator on dashboard when bounties are active

### Verification
- Bounty hunters spawn after high pirate fame threshold
- They appear during travel as encounters
- Can't negotiate with bounty hunters
- Active bounties are trackable and clearable
- Bounty frequency scales with pirate fame level

---

## Stage 08 Completion Checklist

- [ ] Pirate system clearing works end-to-end (outposts → boss → conversion)
- [ ] Warlord bosses have faction-specific behavior
- [ ] Ship dealership allows purchasing, selling, and managing multiple ships
- [ ] Component upgrades purchasable and installable
- [ ] Player stations buildable with 3 tier upgrade path
- [ ] Station raids work (passive for Tier 2, active for Tier 3)
- [ ] Black market deliveries with 5 outcome types
- [ ] Contraband detection at patrol checkpoints
- [ ] Bribe system with Negotiation skill influence
- [ ] Bounty hunters pursue high-pirate-fame players
- [ ] All new data saves and loads correctly

**Next Stage:** [09 — Story & Tutorial](09_story_and_tutorial.md)
