# Technical Design

## Overview
This document defines the technical architecture, tools, and frameworks used to build the game.

---

## Game Engine
- **Engine:** Godot 4.3+
- **Scripting Language:** GDScript (primary), with C# available for complex systems
- **Rationale:** Open-source, lightweight, strong 2D support, built-in UI system, cross-platform export, active community

---

## Project Structure
```
SpaceGame/
├── docs/                      # Design documents
├── project.godot              # Godot project file
├── assets/                    # Art, audio, fonts
│   ├── sprites/               # 2D sprites and textures
│   │   ├── ships/             # Ship sprites
│   │   ├── planets/           # Planet sprites and backgrounds
│   │   ├── ui/                # UI icons and elements
│   │   └── effects/           # Particle textures, VFX
│   ├── audio/                 # Sound and music
│   │   ├── music/             # Background music tracks
│   │   ├── sfx/               # Sound effects
│   │   └── ambient/           # Ambient loops
│   ├── fonts/                 # Custom fonts
│   └── shaders/               # Custom shader files
├── scenes/                    # Godot scene files (.tscn)
│   ├── main.tscn              # Entry scene
│   ├── game/                  # Core game scenes
│   │   ├── game_manager.tscn  # Top-level game controller
│   │   ├── star_map.tscn      # Galaxy/star map with 6-digit address input
│   │   ├── travel.tscn        # Jump travel sequence (multi-hop)
│   │   └── search.tscn        # System search / discovery screen
│   ├── system/                # System-related scenes
│   │   ├── system_view.tscn   # System overview (type, discovery progress)
│   │   ├── marketplace.tscn   # Trading interface
│   │   ├── services.tscn      # Repair, refuel, upgrades
│   │   ├── shipyard.tscn      # Ship purchase (hub systems only)
│   │   ├── bulletin_board.tscn # Hub info board (addresses, routes, warnings)
│   │   └── black_market.tscn  # Contraband trading
│   ├── combat/                # Combat scenes
│   │   └── combat.tscn        # Combat encounter
│   ├── ui/                    # Reusable UI components
│   │   ├── hud.tscn           # Main HUD overlay
│   │   ├── inventory.tscn     # Cargo/inventory panel
│   │   ├── dialogue.tscn      # Dialogue box
│   │   ├── main_menu.tscn     # Title/main menu
│   │   └── pause_menu.tscn    # Pause menu
│   └── encounters/            # Random encounter scenes
│       ├── pirate.tscn        # Pirate ambush
│       └── derelict.tscn      # Derelict ship discovery
├── scripts/                   # GDScript files (.gd)
│   ├── autoload/              # Singleton/autoload scripts
│   │   ├── game_state.gd      # Global game state manager
│   │   ├── save_manager.gd    # Save/load system
│   │   ├── event_bus.gd       # Global signal bus
│   │   ├── audio_manager.gd   # Audio controller
│   │   ├── discovery_db.gd    # Persistent discovery database
│   │   ├── economy_manager.gd # Stock market price simulation
│   │   └── navigation_manager.gd # 6-digit address system and jump routing
│   ├── models/                # Data model scripts
│   │   ├── pilot.gd           # Pilot resource class
│   │   ├── ship.gd            # Ship resource class
│   │   ├── system_data.gd     # System resource (address, type, locations)
│   │   ├── cargo.gd           # Cargo and trade goods
│   │   ├── mission.gd         # Mission resource class
│   │   └── discovery.gd       # Discoverable location resource
│   ├── systems/               # Game system scripts
│   │   ├── trading.gd         # Economy and trading logic
│   │   ├── combat_manager.gd  # Combat resolution
│   │   ├── navigation.gd      # Travel and jump path calculation
│   │   ├── encounter_manager.gd # Random encounter and travel event logic
│   │   ├── search_manager.gd  # Progressive discovery / search mechanic
│   │   ├── mission_manager.gd # Mission tracking
│   │   └── procedural_gen.gd  # System and cluster generation
│   └── ui/                    # UI controller scripts
│       ├── hud_controller.gd
│       ├── market_ui.gd
│       └── dialogue_controller.gd
├── data/                      # Game data (Godot Resources)
│   ├── planets/               # Planet resource files (.tres)
│   ├── goods/                 # Trade goods definitions (.tres)
│   ├── ships/                 # Ship definitions (.tres)
│   ├── events/                # Event definitions (.tres)
│   └── dialogue/              # Dialogue trees (.tres or .json)
├── addons/                    # Godot plugins (if any)
└── export_presets.cfg         # Export configurations
```

---

## Godot-Specific Architecture

### Scene Tree Structure
```
Main (Node)
├── GameState (Autoload)
├── SaveManager (Autoload)
├── EventBus (Autoload)
├── AudioManager (Autoload)
└── CurrentScene (varies)
    ├── StarMap / PlanetView / Combat / etc.
    ├── HUD (CanvasLayer)
    └── DialogueBox (CanvasLayer)
```

### Autoload Singletons
| Singleton | Purpose |
|-----------|---------|
| `GameState` | Tracks pilot, ship, current 6-digit address, turn count, game clock |
| `SaveManager` | Handles save/load to `user://saves/` |
| `EventBus` | Global signal bus for decoupled communication |
| `AudioManager` | Manages music and SFX playback |
| `DiscoveryDB` | Persistent discovery database — tracks all discovered locations per system |
| `EconomyManager` | Stock market price simulation, supply/demand tracking per system |
| `NavigationManager` | 6-digit address lookup, jump path calculation, route planning |

### Custom Resources
- Use Godot's `Resource` class for data models (Pilot, Ship, Planet, Cargo, Mission)
- Resources are serializable, inspector-editable, and saveable as `.tres` files
- Enables data-driven design with editor tooling
- **Discovery database** stored as a dictionary of system addresses → discovered locations
- **Economy state** stored per system with price arrays, supply/demand values, and trend data

---

## Key Godot Features to Leverage

### UI System
- Godot's built-in Control nodes for all UI (no external UI framework needed)
- `Theme` resources for consistent styling across all screens
- `RichTextLabel` for styled narrative text with BBCode
- `AnimationPlayer` for UI transitions and effects

### Scene Management
- Scene switching via `SceneTree.change_scene_to_packed()`
- Additive scenes for overlays (HUD, dialogue, pause menu)
- Scene instancing for reusable components

### Signals
- Godot's signal system for event-driven architecture
- Custom signals on the `EventBus` autoload for cross-scene communication
- Examples: `trade_completed`, `system_arrived`, `combat_started`, `location_discovered`, `system_cleared`, `player_died`, `price_updated`

### Tween and Animation
- `Tween` nodes for smooth UI animations
- `AnimationPlayer` for complex sequences
- `Shader` support for visual effects (hyperspace, planet atmospheres)

---

## Data Storage

### Game Data
- [ ] Godot Resource files (.tres) for static game content
- [ ] JSON files for bulk data (dialogue trees, large datasets)
- [ ] How is data loaded at startup? (preload vs. load on demand)

### Save System
- [ ] Save format: Godot Resource (.tres), JSON, or ConfigFile
- [ ] Save location: `user://` directory (OS-appropriate)
- [ ] Multiple save slots?
- [ ] Auto-save mechanics

---

## Architecture Patterns

### Master Entity System
All game objects that exist in the world inherit from a **MasterEntity** base class. This provides a unified interface for stats, fame, location, and interactions.

```
MasterEntity (fame_level, name, location_address, base_stats...)
├── PlayerEntity (pilot skills, journal, reputation, backgrounds, pirate fame)
│   └── Ship class determines stat caps and component tiers
│       └── Player + Ship treated as one entity (player never leaves ship)
├── NPCEntity (dialogue, trade inventory, quest flags, faction affiliation)
├── EnemyEntity (loot table, behavior AI, aggression, fame level)
│   ├── Pirates (faction: Void Reavers / Silk Hand / Rust Collective / Phantom Circuit)
│   ├── Bounty Hunters
│   └── Faction Enforcers
├── ResourceNode (resource type, depletion rate, yield, discovery state)
└── EventEntity (trigger conditions, rewards, flags, one-time vs repeatable)
```

**Key Principles:**
- `MasterEntity` contains all **shared properties**: fame level, name, location (XX-XXXX address), base stats
- Subclasses inherit from `MasterEntity` and add **unique properties** specific to their type
- The **PlayerEntity** and their current ship are one combined entity — ship class determines stat caps
- **EnemyEntity** has its own fame level, enabling the intimidation mechanic (player fame vs. enemy fame)
- **ResourceNode** and **EventEntity** are also entities, allowing the discovery system to treat all discoverable things uniformly
- In Godot: `MasterEntity` extends `Resource`, subclasses extend `MasterEntity`

### State Machine
- Game states managed via a state machine pattern
- Each state is a scene or node handling its own input, update, and render
- Consider using Godot's `State` pattern or a state machine addon

### Event System
- [ ] Global `EventBus` autoload with custom signals
- [ ] Signal types (trade completed, planet arrived, combat started, etc.)
- [ ] Connected listeners per scene

### Data-Driven Design
- Game content defined in Resource files, not hardcoded in scripts
- Easy to add new planets, goods, ships, events via the Godot editor
- Custom `@export` properties for designer-friendly editing

---

## Configuration
- [ ] Game settings (difficulty, audio volume, display preferences)
- [ ] Configurable constants via exported Resource files
- [ ] Debug/dev mode toggles (Godot's `OS.is_debug_build()`)

---

## Performance Considerations
- [ ] Target platforms (Windows, Mac, Linux — potentially web via HTML5 export)
- [ ] Minimum specs and resolution targets
- [ ] Memory management for large game worlds (lazy loading scenes)
- [ ] Efficient save/load for complex game state

---

## Export and Distribution
- [ ] Target platforms (Windows, macOS, Linux, Web)
- [ ] Export presets configuration
- [ ] Installer / distribution method (itch.io, Steam, standalone)
- [ ] Version numbering scheme

---

## Testing Strategy
- [ ] Godot's built-in debugging tools (remote debugger, profiler)
- [ ] GDScript unit testing (GUT addon or similar)
- [ ] Integration tests for game flow
- [ ] Manual playtesting plan

---

## Plugins and Addons (Under Consideration)
| Addon | Purpose | Notes |
|-------|---------|-------|
| GUT | Unit testing | GDScript unit test framework |
| Dialogic | Dialogue system | Visual dialogue editor |
| Phantom Camera | Camera effects | Smooth transitions |
| Sound Manager | Audio management | Advanced audio bus control |

---

## Notes
_Add design notes, open questions, and decisions here._
