# Stage 04: Economy & Trading

## Goal
Implement the full trading economy: stock market pricing, dynamic supply/demand, marketplace UI, cargo management, and the haggle system. By the end of this stage the player can buy goods cheap, travel, sell expensive, and see their shard balance grow. Trading should feel like the primary way to make money and progress.

**Design Docs:** [07_economy_and_trading.md](../docs/07_economy_and_trading.md), [03_ship.md](../docs/03_ship.md) (cargo section), [12_balancing.md](../docs/12_balancing.md)
**Prerequisites:** Stage 03 complete (core loop, travel, discovery, UI shell)
**Outputs:** EconomyManager, marketplace UI, cargo system, haggle mechanic, dynamic pricing

---

## Phase 4.1 — EconomyManager Singleton

**Goal:** Create the central economy system that manages prices, stock levels, and supply/demand for every system.

### Tasks
- [ ] Create `scripts/autoload/economy_manager.gd`
- [ ] Load trade good definitions (22 common + 22 rare goods with base prices, weights, categories)
- [ ] Implement per-system economy state:
  - Which goods are stocked (30-50% of available goods, weighted by system type)
  - Current stock quantity per good (based on weight 1-15 defining range)
  - Current price per good
  - Supply/demand modifier per good
- [ ] Implement the price formula:
  ```
  final_price = base_price 
    × system_type_modifier    # hub type bonus/penalty
    × economy_level_modifier  # population tier factor
    × supply_demand_modifier  # driven by stock level
    × time_fluctuation        # periodic randomness
    × event_modifier          # world events (placeholder for now)
    × fame_modifier           # pilot reputation discount
  ```
- [ ] Implement `generate_economy(system_data: SystemData, region_specialties: Dictionary, seed: int)`:
  - Select which goods this system stocks
  - Set initial stock quantities
  - Calculate initial prices based on region specialties (dominant = cheap, scarce = expensive)
- [ ] Implement `get_buy_price(address, good_name) -> int`
- [ ] Implement `get_sell_price(address, good_name) -> int` (sell price = buy price × sell ratio, typically 85-95%)
- [ ] Implement economy tick on `turn_ended`:
  - Prices drift slightly toward baseline
  - Stock slowly regenerates toward default
  - Random minor fluctuations
- [ ] Register as Autoload

### Verification
- Systems near dominant regions have low prices for those goods
- Systems far from source regions have high prices for rare goods
- Prices change slightly each turn
- Buy/sell spread exists (can't instantly profit at same system)
- Stock quantities are within expected ranges

---

## Phase 4.2 — Cargo System

**Goal:** Implement the ship's cargo hold so goods can be bought, stored, transported, and sold.

### Tasks
- [ ] Flesh out cargo management in `Ship` resource:
  - `cargo_capacity: int` — determined by cargo bay component tier
  - `get_used_cargo() -> int` — sum of all held goods
  - `get_free_cargo() -> int` — capacity minus used
  - `special_slot_count: int` — determined by cargo bay tier (Tier 1 = 1 slot, higher tiers add more)
- [ ] Implement `add_cargo(good_name, quantity, buy_price) -> bool`:
  - Check capacity
  - Add to cargo_hold array
  - Track buy price per unit (for profit calculation display)
  - Return false if insufficient space
- [ ] Implement `remove_cargo(good_name, quantity) -> Dictionary`:
  - Remove from cargo_hold
  - Return the removed goods with their buy prices
- [ ] Implement `get_cargo_manifest() -> Array[Dictionary]`:
  - Return all held goods with quantities and buy prices
- [ ] Update dashboard cargo visual:
  - Show fill percentage on cockpit cargo area
  - Cargo area visually changes with fill level

### Verification
- Can add goods up to capacity, adding more fails gracefully
- Removing goods frees up space correctly
- Buy prices are tracked per cargo entry
- Dashboard cargo indicator updates in real time

---

## Phase 4.3 — Marketplace UI

**Goal:** Build the marketplace screen where the player buys and sells goods at discovered merchant locations.

### Tasks
- [ ] Create `scenes/ui/marketplace.tscn` (displays in left monitor zone)
- [ ] Show two tabs: **Buy** and **Sell**
- [ ] **Buy tab**:
  - List all goods this merchant stocks with: name, category (common/rare), current price, stock available
  - Color-code prices relative to base price (green = below base = good deal, red = above base)
  - Quantity selector (1, 5, 10, max)
  - Total cost display
  - "Buy" button — disabled if insufficient shards or cargo space
  - Show remaining cargo space
- [ ] **Sell tab**:
  - List all goods in player's cargo with: name, quantity held, buy price (what player paid), sell price here, profit/loss per unit
  - Color-code profit (green = profit, red = loss)
  - Quantity selector
  - Total revenue display
  - "Sell" button
- [ ] On transaction:
  - Update player shards
  - Update cargo hold
  - Update system stock and supply/demand modifier (buying drives price up, selling drives price down)
  - Emit `EventBus.trade_completed`
  - Play transaction SFX (stub)
  - Show shard change popup (+/- green/red)
- [ ] Show sub-system wholesale info:
  - Sub-systems of a hub may offer discounted prices
  - Display discount percentage if applicable

### Verification
- Can buy goods — shards decrease, cargo increases, stock decreases
- Can sell goods — shards increase, cargo decreases, stock increases
- Prices visibly shift after large transactions
- Can't buy more than cargo space allows
- Can't buy more than shards allow
- Profit/loss display is accurate per unit

---

## Phase 4.4 — Haggle System

**Goal:** Implement the one-attempt-per-visit haggle mechanic at merchants.

### Tasks
- [ ] Add haggle button to marketplace UI (visible only if not yet attempted this visit)
- [ ] Implement haggle resolution:
  - Base success chance: depends on Negotiation skill level
  - Success: discount on all buy prices / premium on all sell prices for this visit
  - Failure: price increase for this visit
  - Discount/premium amount scales with Negotiation level
- [ ] Show haggle result:
  - Success: green flash, "Merchant offers better prices!" — prices update in list
  - Failure: red flash, "Merchant raises prices." — prices update in list
- [ ] Track haggle state per merchant per visit (resets when player leaves and returns)
- [ ] Haggle only available at standard merchants, not black market

### Verification
- Haggle button appears once per merchant visit
- Success changes prices favorably
- Failure changes prices unfavorably
- Button disappears after one attempt
- Returning to the same merchant on a later visit allows haggling again

---

## Phase 4.5 — Price History and Journal Trading Data

**Goal:** Track prices the player has seen so they can plan trade routes.

### Tasks
- [ ] Extend pilot journal to track:
  - Last known price for each good at each visited merchant
  - Timestamp (turn number) of last observation
  - Mark stale prices (older than X turns) with a visual indicator
- [ ] Create `scenes/ui/journal_trading.tscn` (displays in left monitor zone):
  - Sortable table of known prices across visited systems
  - Columns: good name, system, price, turns since observed
  - Filter by good or by system
  - Highlight best known buy/sell opportunities
- [ ] Implement profit route suggestions:
  - Compare known prices across systems
  - Suggest "Buy X at System A, sell at System B" for top profit margins
  - Only suggest routes within current jump drive range

### Verification
- Visiting a merchant records its prices in the journal
- Journal table shows all observed prices with staleness
- Route suggestions identify genuinely profitable trades
- Filters and sorting work correctly

---

## Phase 4.6 — Dynamic Market Events (Foundation)

**Goal:** Implement the foundation for market events that cause price spikes, crashes, and shortages. Full event content comes in Stage 07, but the economy system needs to support them now.

### Tasks
- [ ] Implement event modifier system in EconomyManager:
  - `apply_event(address, good_name, modifier, duration_turns)`
  - Events temporarily multiply price by modifier
  - Events expire after duration, prices return toward normal
- [ ] Implement price spike: demand surge causes prices to rise 150-300%
- [ ] Implement price crash: oversupply causes prices to drop 30-60%
- [ ] Implement shortage: stock drops to 0, prices rise until restocked
- [ ] Implement event recovery:
  - After event expires, prices gradually return to baseline over 5-10 turns
  - Stock regenerates slowly
- [ ] Stub random event generation (1-3% chance per turn per system):
  - For now, just randomly trigger events on nearby systems for testing
  - Full event system with narrative comes in Stage 07

### Verification
- Price spike visibly raises prices at affected system
- Price crash visibly lowers prices
- Shortage removes stock entirely
- Prices recover toward baseline after event expires
- Events visible in marketplace (indicator showing active event)

---

## Stage 04 Completion Checklist

- [ ] EconomyManager generates per-system economy with region specialty influence
- [ ] Prices follow the multi-factor formula and feel dynamic
- [ ] Cargo hold tracks goods with capacity limits and buy-price history
- [ ] Marketplace UI supports buying and selling with clear profit/loss display
- [ ] Transactions update stock, prices, shards, and cargo correctly
- [ ] Haggle system works once per visit with skill-based success
- [ ] Journal tracks price history and suggests profitable routes
- [ ] Market events can spike, crash, or shortage prices
- [ ] Economy ticks each turn with drift and regeneration
- [ ] Sub-system wholesale discounts are applied

**You can now trade:** buy cheap at source regions, sell expensive at scarce regions, watch your shards grow

**Next Stage:** [05 — Pilot & Progression](05_pilot_and_progression.md)
