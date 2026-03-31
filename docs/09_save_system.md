# Save and Load System

## Overview
This document defines how game state is persisted, loaded, and managed. The save system must capture the **persistent discovery database**, dynamic economy state, and all player progress.

---

## Save Data

### What Gets Saved
- **Pilot state** — Name, background, stats, skills, level, XP, credits, reputation per faction
- **Ship state** — Class, components, upgrade tiers, hull integrity, cargo contents
- **Loaner ship status** — Whether player is in a loaner (death recovery state)
- **Abandoned ship location** — 6-digit address where ship was lost, remaining cargo, loot percentage
- **Current location** — 6-digit address of current system
- **Last merchant location** — 6-digit address of last visited merchant (respawn point)
- **Discovery database** — All discovered locations per system, persistent across visits
- **System states** — Hostility status (friendly/dead/hostile), controlling faction, cleared status
- **Economy state** — Current prices, supply/demand levels, price trend data per system
- **Active missions** — Current quests, objectives, destination addresses
- **Completed missions** — Quest history
- **Story progress** — Main story flags, tutorial completion, act progression
- **Known addresses** — All system addresses the player has learned (from exploration, NPCs, charts, missions)
- **Player journal** — Visited systems, price history, statistics, discoveries
- **Turn count** — Background game clock
- **World event state** — Active events, system hostility changes, faction war status

### What Does NOT Get Saved
- Static game data (loaded from Godot Resource files at startup)
- Procedural generation seeds (if deterministic, the seed is saved; generated content is not)
- UI state (menu positions, scroll positions)
- Temporary calculations
- Audio playback state

---

## Save Format

### File Format
- **Primary**: Godot Resource files (`.tres`) or JSON via `user://` directory
- JSON preferred for human-readability and debugging during development
- Consider binary/compressed format for release builds

### File Structure
```json
{
  "version": "1.0",
  "timestamp": "2026-03-31T12:00:00",
  "turn_count": 142,
  "pilot": {
    "name": "...",
    "background": "mechanic",
    "level": 5,
    "xp": 1200,
    "credits": 15000,
    "skills": { "trading": 3, "piloting": 2, "engineering": 4, ... },
    "reputation": { "faction_a": 50, "faction_b": -10, ... }
  },
  "ship": {
    "class": "corvette",
    "name": "...",
    "hull": 75,
    "components": { "jump_drive": 3, "shields": 2, ... },
    "cargo": [ { "good": "ore", "quantity": 20 }, ... ]
  },
  "recovery_state": {
    "is_in_loaner": false,
    "abandoned_ship_address": null,
    "abandoned_cargo": [],
    "loot_percentage": 0
  },
  "location": {
    "current_address": "384721",
    "last_merchant_address": "291034"
  },
  "discovery_database": {
    "384721": {
      "system_type": "friendly",
      "total_locations": 7,
      "discovered": [ "merchant_1", "shipyard", "resource_deposit_1" ],
      "hostility": "friendly",
      "cleared": false
    }
  },
  "economy": {
    "384721": {
      "prices": { "ore": 45, "food": 12, ... },
      "supply": { "ore": 80, "food": 120, ... },
      "trend": { "ore": "rising", "food": "stable", ... }
    }
  },
  "known_addresses": ["384721", "291034", "482103", ...],
  "missions": {
    "active": [ ... ],
    "completed": [ ... ]
  },
  "story_flags": {
    "tutorial_complete": true,
    "act_1_started": true,
    ...
  },
  "journal": {
    "visited_systems": [...],
    "price_history": {...},
    "statistics": {
      "total_profit": 42000,
      "systems_visited": 15,
      "discoveries_made": 34,
      "enemies_defeated": 8,
      "hostile_systems_cleared": 2
    }
  }
}
```

---

## Save Slots
- [ ] Number of save slots (recommend 3-5 + 1 auto-save)
- [ ] Save slot naming (player name + timestamp)
- [ ] Save slot preview info (pilot name, credits, location, turn count, play time)

---

## Save/Load Triggers

### Manual Save
- Save available from the pause menu at any time
- Save available when docked at any system
- [ ] Quick save hotkey

### Auto-Save
- Auto-save when arriving at a new system
- Auto-save when completing a mission
- Auto-save after major events (combat victory, clearing hostile system, story progression)
- Auto-save uses a **dedicated slot** that doesn't overwrite manual saves
- Auto-save is the fallback for crash recovery

---

## Save File Location
- Godot's `user://saves/` directory
- Cross-platform: Godot handles OS-appropriate paths automatically
  - Windows: `%APPDATA%/Godot/app_userdata/SpaceGame/saves/`
  - macOS: `~/Library/Application Support/Godot/app_userdata/SpaceGame/saves/`
  - Linux: `~/.local/share/godot/app_userdata/SpaceGame/saves/`

---

## Version Compatibility
- Save files include a **version field**
- Migration functions handle saves from older game versions
- Add new fields with defaults rather than breaking old save format
- Major version changes may require save migration scripts

---

## Error Handling
- Validate save file integrity on load (checksum or structural validation)
- Corrupted save detection with user-friendly error message
- Keep **backup of previous save** when overwriting (`.bak` file)
- Recovery from failed saves — don't delete old save until new save confirmed written
- Auto-save provides a fallback if manual save is corrupted

---

## Death and Respawn Save Behavior
- On player death, the game **auto-saves** the recovery state
- Recovery state includes: loaner ship flag, abandoned ship location, cargo remaining
- This prevents save-scumming deaths (the death is recorded)
- [ ] Should the player be able to reload a pre-death save? (Design decision)

---

## Notes
_Add design notes, open questions, and decisions here._

### Open Design Questions
- Exact number of save slots
- Save file size optimization for large discovery databases
- Should procedural generation use deterministic seeds (save seed vs. save all generated content)?
- Save-scumming policy — allow pre-death reloads or enforce death consequences?
- Cloud save support (future consideration)
