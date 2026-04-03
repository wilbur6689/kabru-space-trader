# UI and Presentation

## Overview
This document defines the user interface, visual presentation, and player experience using Godot's UI system. The game is built around a **cockpit-first design** — the player's ship cockpit is always visible as the primary screen, with all UI elements overlaid on cockpit zones and monitors.

---

## Visual Style

### Art Direction
- **Style**: Still images as primary backgrounds with UI overlays
- **Aesthetic**: Gritty industrial with hints of technology advancement
- **Tone**: Worn, lived-in spaceships and stations — not pristine sci-fi
- **Reference**: Supplied cockpit artwork (paceShip.png, spaceship_female.png, draftSpaceship.png)

### Resolution and Display
- **Base resolution**: 2560x1440 (1440p)
- **Display modes**: Fullscreen and windowed with toggle in settings
- **Stretch mode**: Configured to scale properly across monitor sizes
- **Godot stretch settings**: TBD based on testing

---

## Cockpit-First Design

### Core Principle
The cockpit interior is the **primary game screen** that the player always sees. All gameplay happens through the cockpit's monitors, windows, and console areas. The player never leaves the cockpit view during normal gameplay — only transitional overlays temporarily layer on top.

### Cockpit Zones (Reference — Final Mapping Deferred)
The cockpit artwork contains natural interactive anchor points:

| Zone | Potential Function | Notes |
|------|-------------------|-------|
| Left monitors (3 screens) | Main info displays | Marketplace, journal, missions, bulletin board, inventory |
| Console screens (front) | Ship systems | Navigation/address input, scanner, system overview, ship status |
| Cockpit window (top right) | World view | Current system, planets, space, travel animations, enemy ships |
| Cargo area (left side, crates) | Cargo visual | Crates appear/disappear based on cargo hold contents |
| Dashboard | Quick stats | Shards, hull/shields, discovery progress, turn counter |

**Note:** Exact zone-to-function mapping is deferred to visual design phase. See deferred design doc.

### Ship Class Cockpit Variants
- Different ship classes may have different cockpit artwork
- Same zone layout principles apply across all variants
- Details deferred to art production phase

---

## Transitional Overlays

The cockpit remains visible during all transitions. Overlays layer on top of specific cockpit areas.

### Jump Travel
- **Tunnel animation** overlays on the cockpit windows
- Stars begin flying by through the glass
- Cockpit interior stays visible underneath
- Multi-hop jumps may show brief pauses between hops

### Discovery (Search Result)
- After a search action, a **"You've Discovered" card** pops up over the cockpit
- Card reveals the location name, type, and brief description
- Player dismisses the card to return to normal cockpit view
- Dangerous discoveries may have a different card style (warning colors)

### Arriving at a System
- **Planet or station graphic** appears outside the cockpit windows
- System information is **overlaid on the cockpit monitor screens**
- System name, type, danger level, discovery progress displayed
- Known locations and action menu appear on monitors

### Combat
- **Enemy ship overlay** appears outside the cockpit windows
- Combat UI overlays on cockpit screens
- Cockpit interior remains visible throughout
- Detailed combat visual design deferred (see deferred design doc)

---

## Screen Layouts

All screens are displayed **within the cockpit view** on monitors and overlay panels.

### Main HUD (Always Visible on Dashboard)
- Current system name and Q-R address
- System alignment indicator (Friendly/Neutral/Hostile/Pirate-Controlled/Dead) and population tier
- Discovery progress: "X / Y locations discovered"
- Shards balance
- Ship status bars (hull, shields)
- Cargo capacity indicator (used / max)
- Jump drive tier/range indicator
- Turn counter
- Message inbox notification indicator (unread count)

### Main Menu Screen (Outside Cockpit)
- Game title and artwork
- New Game / Continue / Load / Settings / Quit
- Animated background (star field)
- Only screen that is NOT the cockpit view

### System Overview (On Cockpit Monitors)
- System artwork visible through cockpit window
- System name, Q-R address, type, and danger level on monitors
- Discovery progress display: "X / Y locations discovered"
- List of discovered locations with interaction buttons
- Core action menu:
  - **Search** (costs 1 turn — discover next location)
  - **Visit** (free — interact with discovered location)
  - **Travel** (open navigation / address input)
  - **Save** (manual save — only available when docked)
  - **Journal** (open pilot log)
  - **Messages** (open message inbox)
- NPC interaction options (at systems with NPC discoveries)

### Discovered Location Panel (Overlay)
- Location name and type (merchant, shipyard, resource, black market, danger, etc.)
- Location description and available interactions
- Quick actions per location type (buy/sell, repair, accept mission, etc.)

### Marketplace Screen (On Cockpit Monitors)
- Scrollable goods list with icons
- Buy/sell price columns
- Quantity selectors
- Player cargo summary sidebar
- Available shards
- Profit/loss indicators per item
- Haggle button (1 attempt per merchant)

### Star Map / Navigation Screen (On Cockpit Monitors)
- **Q-R address input field** — player types destination address
- Known systems displayed as nodes (from bulletin boards, NPCs, star charts, exploration)
- Alignment indicators (Friendly = green, Neutral = yellow, Hostile = orange, Pirate-Controlled = red, Dead = gray, unknown = dim)
- Jump drive range overlay — reachable vs. unreachable systems highlighted
- Jump path preview — shows intermediate systems and number of jumps required
- Travel risk indicator (known route vs. unknown territory)
- Current location indicator
- Travel confirmation panel with jump count and risk summary

### Bulletin Board Screen (Hub Systems — On Cockpit Monitors)
- List of known neighboring system addresses with names and types
- Friendly/hostile status indicators per system
- Known trade routes with commodity hints
- Available mission postings with destination addresses
- Warnings about hostile regions
- Star charts for purchase

### Combat Screen (Cockpit Window + Monitor Overlay)
- Enemy ship visible through cockpit window
- Player ship status on monitors (hull, shields)
- Enemy status display
- Action buttons: Attack, Defend, Flee, Negotiate, Intimidate (when available)
- Combat log / narration text
- Turn indicator
- Detailed visual design deferred

### Inventory / Ship Screen (On Cockpit Monitors)
- Cargo grid with item quantities
- Ship component slots with upgrade tier indicators
- Ship class, size, and stats overview
- Pilot skills and fame level
- Active mission list

### Player Journal Screen (On Cockpit Monitors)
- Visited systems list with Q-R addresses and discovery progress
- Known addresses (all sources — exploration, NPCs, charts, missions)
- Last known prices per visited system
- Regional goods overview (supply/demand hints per region)
- Faction reputation standings
- Pirate fame per faction
- Fame level and progress toward next point
- Owned ships and storage locations
- Personal station locations and tiers
- Statistics dashboard

### Message Inbox (On Cockpit Monitors)
- List of received messages (newest first)
- Message types: delivery notifications, mission updates, boss warnings, NPC tips, event alerts, price shifts
- Read/unread status indicators
- Player reads messages when they choose — not forced

### Death / Recovery Screen (Overlay)
- "Ship Destroyed" notification overlay on cockpit
- Cargo looted percentage displayed
- Respawn location (last visited hub) shown
- Options: Travel to recover ship, Buy new ship, View star map

### Save/Load Screen (Overlay)
- 3 manual save slots + 1 auto-save slot
- Each slot shows: Pilot name, play time, current system address and name
- Save button (writes to selected slot)
- Load button (loads selected slot)
- Available only when docked (save) or from main menu/pause (load)

---

## Godot UI Framework

### Approach
- Godot's built-in Control nodes for all menus and HUD elements
- `Theme` resource for consistent styling across all screens
- `CanvasLayer` for HUD and overlays that persist across scenes
- `RichTextLabel` with BBCode for narrative text styling
- Cockpit background as a `TextureRect` with UI elements positioned over zones

### Key Control Nodes
| Node | Usage |
|------|-------|
| `VBoxContainer` / `HBoxContainer` | Layout and alignment |
| `GridContainer` | Marketplace tables, inventory grids |
| `ProgressBar` / `TextureProgressBar` | Hull, shields, cargo bars |
| `Button` / `TextureButton` | Menu options, actions |
| `RichTextLabel` | Story text, descriptions, dialogue, message inbox |
| `TabContainer` | Multi-tab panels (ship, pilot, missions) |
| `ConfirmationDialog` | Confirmation prompts for major actions |
| `LineEdit` | Address input field for navigation |
| `TextureRect` | Cockpit background, planet/station artwork, enemy ship overlays |

---

## Theme and Styling

### Color Palette
| Element | Color | Hex | Notes |
|---------|-------|-----|-------|
| Background | Dark space blue | #0a0e1a | Main background |
| Panel BG | Dark gray | #1a1e2e | UI panel backgrounds |
| Primary text | Light gray | #d0d0d0 | Standard text |
| Headers | Cyan | #00ccff | Screen titles, headings |
| Shards/Money | Green | #00ff88 | Currency values |
| Damage/Danger | Red | #ff3344 | Warnings, low stats |
| Success | Bright green | #44ff44 | Positive outcomes |
| Lore/Flavor | Muted blue | #6688aa | Atmosphere text |
| Interactive | Gold/Yellow | #ffcc00 | Buttons, selectable items |
| Disabled | Dark gray | #444444 | Unavailable options |

### Typography
| Usage | Font | Notes |
|-------|------|-------|
| **Headers** | Orbitron | Sci-fi tech feel, used for screen titles and section headers |
| **Everything else** | Inconsolata | Clean monospace, ship computer aesthetic, body text, data, dialogue |

Both are free Google Fonts — included in the project assets.

---

## Input Design

### Input Methods
- **Mouse**: Click on cockpit zones, menus, buttons, star map nodes
- **Keyboard shortcuts**: Hotkeys for common actions and screens
- **Gamepad**: Deferred to post-MVP

### Input Mapping
- Keyboard shortcuts mapped via Godot's `InputMap`
- Exact keybindings TBD during implementation
- Common actions should have single-key shortcuts (e.g., S for search, T for travel, J for journal)

### Input Feedback
- Button hover and press visual feedback
- Sound effects on UI interactions (see audio doc)
- Tooltips for items and system details on hover

---

## Confirmation Dialogs

The following actions require an "Are you sure?" confirmation:

- Buying a ship
- Selling a ship
- Trading in a ship
- Jettisoning cargo
- Accepting a black market deal
- Traveling to a hostile or unknown system
- Abandoning a mission

Confirmations use Godot's `ConfirmationDialog` styled to match the cockpit theme.

---

## Notification System

### Message Inbox
- All notifications go to a **message inbox** on one of the cockpit monitors
- Player chooses when to read messages — never forced
- Unread message count shown on the HUD
- Message types:
  - Black market delivery results
  - Mission deadline warnings
  - Ship transfer/tow completion
  - Boss warnings (hostile system)
  - NPC tips and intel
  - Story event triggers
  - World event updates (faction wars, hostility changes)
  - Price shift alerts

---

## Text Presentation

### Text Reveal Speed
- Text uses a **quick reveal effect** — not instant but under 1 second per few lines
- Applied to: story text, discovery cards, NPC dialogue, event narration
- Data screens (marketplace, journal, stats) display instantly — no reveal delay
- Implemented via `RichTextLabel` visible characters animation

---

## Transitions and Animation

### Scene Transitions
- Cockpit stays visible — transitions are **overlays only**
- Jump travel: Tunnel effect on windows with star streaks
- System arrival: Planet/station art fades in through window
- Discovery: Card slides in from screen edge

### UI Animations
- Tween-based panel open/close for monitor screens
- Number roll-up for shard changes
- Shake effect on cockpit when taking damage
- Pulsing indicator on message inbox when unread messages exist
- Progress bar animations for hull/shield changes

---

## Visual Effects

### Cockpit Window Effects
- Parallax star field visible through windows (idle state)
- Planet/station approach graphics
- Hyperspace tunnel overlay during jumps
- Enemy ship rendering during combat
- Asteroid field visuals during hazard encounters

### Damage Effects
- Screen shake on hull damage
- Cockpit sparks or flicker overlay on critical damage
- Warning lights/indicators when hull is low

---

## Accessibility (Post-MVP)

The following features are deferred to after MVP:
- [ ] Colorblind-friendly alternate palette
- [ ] Font size scaling options
- [ ] High contrast mode
- [ ] Remappable keyboard controls
- [ ] Screen reader support

---

## Notes
_Add design notes, open questions, and decisions here._

### Open Design Questions
- Exact cockpit zone mapping to game functions (deferred)
- Cockpit art variants per ship class
- Combat visual design within cockpit (deferred)
- Keyboard shortcut assignments
- Star map visual layout and interaction design
- How discovery cards look for different discovery types (merchant vs. danger)
- Message inbox UI layout and message format
- Exact text reveal speed tuning
