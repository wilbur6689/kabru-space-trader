# Game Balancing

## Overview
This document defines the numerical balance, progression curves, and tuning parameters for the game. Balance is critical because the game's economy operates on a **dynamic stock market model** and player progression is gated by **jump drive upgrades**.

---

## Economy Balance

### Starting Values
| Parameter | Value | Notes |
|-----------|-------|-------|
| Starting Credits | | Enough to repair tutorial ship + first trade |
| Cheapest Trade Good (Food) | | Common, low margin |
| Most Expensive Trade Good (Artifacts) | | Rare, high margin |
| Average Trade Profit Margin | | Per single trade run |
| Ship Repair (Minor) | | Post-easy-combat repair |
| Ship Repair (Major) | | Post-hard-combat repair |

### Progression Targets
| Milestone | Approx. Turn Range | Credits | Ship | Jump Drive |
|-----------|-------------------|---------|------|------------|
| Tutorial complete | Turn 5-10 | Minimal | Starter (repaired) | Tier 1 (local) |
| First profitable trade | Turn 10-15 | Growing | Starter | Tier 1 |
| First ship upgrade | Turn 20-40 | | Starter + upgrades | Tier 1-2 |
| First new ship purchase | Turn 50-80 | | Freighter/Corvette | Tier 2 |
| First hostile system cleared | Turn 40-70 | | Upgraded ship | Tier 2 |
| New region accessible | Turn 60-100 | | | Tier 3 |
| Mid-game wealth | | | | Tier 3-4 |
| End-game wealth | | | Best ships | Max tier |

---

## Jump Drive Progression

### Jump Drive Tiers
The jump drive is the **primary exploration progression gate**. Each tier opens new systems.

| Tier | Range | Systems Accessible | Cost | Notes |
|------|-------|-------------------|------|-------|
| 1 | Short | Starting cluster only | Starter | Tutorial ship |
| 2 | Medium | Adjacent clusters | | First major upgrade |
| 3 | Long | Most of explored galaxy | | Mid-game |
| 4 | Very Long | Distant clusters | | Late-game |
| Max | Galaxy-wide | Everything | | End-game |

### Pacing
- Tier 1 → 2 should feel like a major achievement (opens the galaxy up)
- Each tier should provide a noticeable increase in accessible content
- The player should always have **more to explore** than they can reach — motivation to upgrade

---

## Combat Balance

### Damage Formula
```
damage = base_weapon_damage * (1 + combat_skill * modifier) - target_defense
```

### Combat Pacing
- Easy combat: 2-3 turns
- Medium combat: 4-6 turns
- Hard combat: 6-10 turns
- Boss (warlord/faction leader): 8-15 turns with phases

### Risk vs Reward
| Encounter Difficulty | Win Rate Target | Average Loot Value | Found In |
|---------------------|----------------|-------------------|----------|
| Easy (Petty Pirate) | 90%+ | Low | Travel, friendly fringe |
| Medium (Pirate Captain) | 60-80% | Medium | Hostile systems |
| Hard (Fleet/Bounty Hunter) | 30-50% | High | Deep hostile territory |
| Boss (Warlord) | 50% with prep | Very High + system control | Hostile system boss |

### Death Costs
- 0-50% cargo looted from destroyed ship (random)
- Time cost to recover ship (travel with loaner)
- Potential price shifts while recovering (market moves on)
- No permanent stat/skill loss

---

## Discovery Balance

### Locations Per System Type
| System Type | Min Locations | Max Locations | Avg | Notes |
|-------------|---------------|---------------|-----|-------|
| Hub System | | | | Most locations, all services |
| Friendly Sub-System | | | | Moderate locations |
| Dead System | | | | Fewer but high-value |
| Hostile System | | | | Moderate, high danger |

### Discovery Danger Rates
| System Type | Chance of Dangerous Discovery | Notes |
|-------------|------------------------------|-------|
| Friendly | ~10-15% | Mostly safe |
| Dead | ~25-35% | Environmental hazards |
| Hostile | ~40-60% | Combat encounters likely |

### Search Pacing
- Searching should feel **meaningful every turn** — each discovery matters
- Systems shouldn't have so many locations that searching feels tedious
- Systems shouldn't have so few that they feel empty after 1-2 searches

---

## Travel Balance

### Jump Path Length
| Route Type | Typical Jumps | Encounter Chance Per Jump |
|------------|---------------|-------------------------|
| Within cluster | 1-3 jumps | Low |
| Adjacent cluster | 3-6 jumps | Low-Medium |
| Distant cluster | 6-12 jumps | Medium |
| Cross-galaxy | 12+ jumps | Medium-High |

### Travel Risk Tuning
| Route Type | Negative Event Chance | Positive Event Chance |
|------------|----------------------|----------------------|
| Known route | 2-5% per jump | 1-3% per jump |
| Unknown territory | 8-15% per jump | 5-8% per jump |
| New region entry | 15-25% | 10-20% (story/side quest) |

---

## Skill Progression Balance

### XP Curve
| Level | Total XP Required | XP Per Level | Skill Points |
|-------|-------------------|-------------|-------------|
| 1 | 0 | - | Starting allocation |
| 2 | | | |
| 3 | | | |
| 5 | | | |
| 10 | | | |
| 15 | | | |
| 20 (Max?) | | | |

### XP Sources
| Activity | XP Reward | Notes |
|----------|-----------|-------|
| Complete a trade | Low | Scales with profit |
| Discover a location | Medium | Each new discovery |
| Win combat | Medium-High | Scales with difficulty |
| Complete a mission | High | Scales with mission tier |
| Clear hostile system | Very High | Major achievement |
| Story milestone | Very High | Key progression moments |

### Skill Impact
- [ ] How much does each skill level improve outcomes?
- [ ] Diminishing returns or linear scaling?
- [ ] Max skill level per skill

---

## Stock Market Tuning

### Price Fluctuation Parameters
| Parameter | Value | Notes |
|-----------|-------|-------|
| Daily random fluctuation range | | e.g., +/- 5% |
| Player trade impact (buy) | | Price increase per unit bought |
| Player trade impact (sell) | | Price decrease per unit sold |
| Price recovery rate | | How fast prices return to baseline |
| Event spike magnitude | | e.g., +50% to +200% for war |
| Event duration | | How many turns before event fades |

### Market Health Indicators
- Average profit per trade run should be **positive but not trivial**
- Best routes should yield 30-80% profit margins
- Contraband routes should yield 100-300% but with real risk
- Random fluctuation should create occasional **windfall** and **loss** scenarios
- Player should feel rewarded for tracking trends and timing trades

---

## Difficulty Tuning

### Knobs to Adjust
- Starting credits
- Price spread (profit margins)
- Discovery danger rates per system type
- Travel encounter frequency
- Enemy strength scaling per region
- Ship repair costs
- Jump drive upgrade costs
- Mission reward multiplier
- Cargo loot percentage on death (0-50% range)

### Playtesting Metrics to Track
- Average credits earned per turn
- Player death frequency and primary causes
- Most/least traded commodities
- Average turns to reach each progression milestone
- Most common trade routes
- Percentage of systems explored vs. available
- Hostile systems cleared per playthrough
- Average game length (turns to story completion)

---

## Regional Difficulty Scaling

### Cluster Difficulty
- Starting cluster: Easy enemies, low danger, forgiving economy
- Adjacent clusters: Medium difficulty, moderate hostile systems
- Distant clusters: Hard enemies, more hostile systems, better loot
- Far clusters: Very hard, faction bosses, endgame content, rare artifacts

### Natural Progression
- Jump drive tier gates the player into **appropriate difficulty content**
- Players can't accidentally reach endgame content with a Tier 1 drive
- Each new tier of content should feel challenging but achievable with the gear available

---

## Notes
_Add design notes, open questions, and decisions here._

### Open Design Questions
- All numerical values need playtesting to finalize
- Stock market simulation complexity (simple vs. detailed)
- How aggressively should difficulty scale between clusters?
- Should there be difficulty settings (Easy/Normal/Hard) or a single tuned experience?
- Exact loot tables per enemy/discovery type
