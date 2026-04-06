# Turns, Time, and the Galactic Standard Year

## Overview
All game actions consume **cycles** — the base unit of time. The game calendar uses the **Galactic Standard Year (GSY)** format. Actions are **non-blocking** — the player can perform other activities while waiting for longer actions to complete.

---

## Galactic Standard Year (GSY)

### Format
```
GSY [Era].[Year].[Cycle]
```
- **Era** — major galactic epoch
- **Year** — year within the era (resets each era)
- **Cycle** — single day equivalent (1 cycle = 1 Earth day)

**Example:** `GSY 3.042.187` = Era 3, Year 42, Cycle 187

---

## Non-Blocking Action System
- Actions that cost cycles **run in the background** — the player is not locked out
- The player can perform other actions while waiting for longer tasks to complete
- **Scheduled actions list** — player can view all pending/in-progress actions and their completion times
- **Skip ahead** — player can fast-forward to the next scheduled completion if desired
- **Warnings** — if skipping would pass a mission deadline or required appointment, the game warns the player before allowing the skip

---

## Action Cycle Costs

### Navigation & Travel
| Action | Cycles | Notes |
|--------|--------|-------|
| Intra-region travel (system to system) | 1 | Short hop within same region |
| Inter-region jump (per region crossed) | 2 per region | Longer jumps scale with distance |
| Plot route / set destination | 0 | Instant — nav computer |

### Exploration & Discovery
| Action | Cycles | Notes |
|--------|--------|-------|
| Search system (discover next location) | 1 | Active scanning |
| Explore a discovered location | 2 | Landing, investigating, looting |
| Collect salvage from dead system | 3 | EVA or drone retrieval, slow process |
| Explore ancient ruins | 2 | Complex site |
| Scan contaminated zone | 1 | Quick with proper equipment |

### Combat
| Action | Cycles | Notes |
|--------|--------|-------|
| Combat encounter (single fight) | 1 | Fast space engagement |
| Pirate Fleet (sequential waves) | 3 | Multiple engagements back to back |
| Boss fight | 2 | Extended battle |
| Flee attempt | 0 | Part of combat turn |

### Trading & Economy
| Action | Cycles | Notes |
|--------|--------|-------|
| Buy/sell goods (per transaction batch) | 0 | Instant — digital commerce |
| Haggle with merchant | 0 | Part of transaction |
| Access black market | 1 | Finding contact, vetting, dealing |
| Bribe authority | 0 | Instant — during patrol encounter |

### Ship Management
| Action | Cycles | Notes |
|--------|--------|-------|
| Buy/swap ship at hub | 1 | Paperwork, transfer, familiarization |
| Request ship transfer | 0 | Order placed; delivery delayed by distance |
| Manage/jettison cargo | 0 | Instant — onboard action |

### Upgrades & Repairs
| Action | Cycles | Notes |
|--------|--------|-------|
| Install upgrade (any component) | 1 | Docking, installation, calibration |
| Repair ship at hub/station | 3 | Docking and repair crew work |
| Self-repair (engineering skill) | 5 | Manual work, slower without a crew |

### Missions
| Action | Cycles | Notes |
|--------|--------|-------|
| Accept/abandon mission | 0 | Instant — just a decision |
| Complete mission (turn in) | 0 | Instant — reward on delivery |

### NPC Interactions
| Action | Cycles | Notes |
|--------|--------|-------|
| Talk to NPC / get tips | 0 | Conversation, no time cost |
| Rescue imprisoned NPC | 1 | Part of combat/exploration encounter |
| Access mission/bulletin board | 0 | Instant — browse and pick |

### Station Building
| Action | Cycles | Notes |
|--------|--------|-------|
| Build Tier 1 station | 5 | Construction takes time |
| Upgrade to Tier 2 | 10 | More complex build |
| Upgrade to Tier 3 | 20 | Major construction project |
| Craft blueprint at station | 3 | Assembly and testing |
| Drop off / pick up cargo | 0 | Instant — your station |
| Defend station from raid | 1 | Combat encounter |

### Information & Scanning
| Action | Cycles | Notes |
|--------|--------|-------|
| Pre-jump scan | 0 | Automatic — part of navigation |
| View prices / bulletin boards | 0 | Instant lookup |
| Purchase star chart | 0 | Instant — data transfer |

### Death & Recovery
| Action | Cycles | Notes |
|--------|--------|-------|
| Respawn after death | 3 | Time passes while recovering |
| Recover destroyed ship | 0 | Just travel there (travel cost applies) |

---

## Skip-Ahead System
- Player can view a list of all **scheduled/in-progress actions** with estimated completion cycles
- Player can choose to **skip ahead** to the next completion event
- Before skipping, the game checks for:
  - **Mission deadlines** that would be passed
  - **Scheduled appointments or events** that would be missed
  - **Incoming threats** (e.g., boss arrival warnings)
- If any conflicts exist, the game displays a **warning** and asks for confirmation before skipping

---

## Open Design Questions
- Starting GSY date for the game (what era/year/cycle does the player begin?)
- How many cycles in a year?
- Does the era number have narrative significance?
- Ship transfer delivery time formula (cycles per region distance?)
- Do some actions block others? (e.g., can you trade while your ship is being repaired?)
- Price fluctuation timing — do prices change on a cycle-based schedule?
- Mission board refresh — minimum cycles before new missions appear
