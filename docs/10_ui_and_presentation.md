# UI and Presentation

## Overview
This document defines the user interface, visual presentation, and player experience using Godot's UI system.

---

## Visual Style
- [ ] Art direction (pixel art, stylized, minimalist, hand-drawn)
- [ ] Resolution and aspect ratio (1920x1080, 1280x720, etc.)
- [ ] Camera perspective (top-down 2D, side-view, isometric)
- [ ] Overall aesthetic (retro sci-fi, sleek modern, gritty industrial)

---

## Godot UI Framework

### Approach
- Godot's built-in Control nodes for all menus and HUD elements
- `Theme` resource for consistent fonts, colors, and styling
- `CanvasLayer` for HUD and overlays that persist across scenes
- `RichTextLabel` with BBCode for narrative text styling

### Key Control Nodes
| Node | Usage |
|------|-------|
| `VBoxContainer` / `HBoxContainer` | Layout and alignment |
| `GridContainer` | Marketplace tables, inventory grids |
| `ProgressBar` / `TextureProgressBar` | HP, fuel, cargo bars |
| `Button` / `TextureButton` | Menu options, actions |
| `RichTextLabel` | Story text, descriptions, dialogue |
| `TabContainer` | Multi-tab panels (ship, pilot, missions) |
| `PopupMenu` / `ConfirmationDialog` | Confirmations, alerts |

---

## Screen Layouts

### Main HUD (CanvasLayer - Always Visible)
- Current system name, 6-digit address, and system type indicator (friendly/dead/hostile)
- Discovery progress: "X / Y locations discovered"
- Credits display
- Ship status bars (hull, shields)
- Cargo capacity indicator (used / max)
- Jump drive range indicator
- Turn counter
- Mini notification area (events, price alerts)
- [ ] HUD mockup / wireframe

### Main Menu Screen
- Game title and artwork
- New Game / Continue / Load / Settings / Quit
- [ ] Animated background (star field, ship flying)

### System Overview Screen
- System artwork / sprite based on type (friendly, dead, hostile)
- System name, 6-digit address, type, and danger level
- Discovery progress display: "X / Y locations discovered"
- List of discovered locations with interaction buttons
- Core action buttons:
  - **Search** (costs 1 turn — discover next location)
  - **Visit** (free — interact with discovered location)
  - **Travel** (open star map / address input)
- NPC interaction points (at hub systems)
- [ ] Layout mockup

### Discovered Location Panel
- Location name and type (merchant, shipyard, resource, black market, danger, etc.)
- Location description and available interactions
- Quick actions per location type (buy/sell, repair, accept mission, etc.)
- [ ] Layout mockup

### Marketplace Screen
- Scrollable goods list with icons
- Buy/sell price columns
- Quantity selectors (slider or +/- buttons)
- Player cargo summary sidebar
- Available credits
- Profit/loss indicators per item
- [ ] Layout mockup

### Star Map / Navigation Screen
- **6-digit address input field** — player types destination address
- Known systems displayed as nodes (from bulletin boards, NPCs, star charts, personal exploration)
- System type indicators (friendly = green, dead = gray, hostile = red, unknown = dim)
- Jump drive range overlay — reachable vs. unreachable systems highlighted
- Jump path preview — shows intermediate systems and number of jumps required
- Travel risk indicator (known route vs. unknown territory)
- Current location indicator
- Travel confirmation panel with jump count and risk summary
- [ ] Layout mockup

### Bulletin Board Screen (Hub Systems Only)
- List of known neighboring system addresses with names and types
- Friendly/hostile status indicators per system
- Known trade routes with commodity hints
- Available mission postings with destination addresses
- Warnings about hostile regions
- Star charts for purchase
- [ ] Layout mockup

### Combat Screen
- Player ship and enemy ship visuals
- Health/shield bars for both sides
- Action buttons (Attack, Defend, Flee, Negotiate)
- Combat log / narration text
- Turn indicator
- [ ] Layout mockup

### Inventory / Ship Screen
- Cargo grid with item icons and quantities
- Ship component slots with upgrade indicators (especially jump drive tier)
- Ship class and stats overview
- Pilot stats and skills panel
- Mission log tab (active and completed)
- [ ] Layout mockup

### Player Journal Screen
- Visited systems list with 6-digit addresses and discovery progress
- Known addresses (all sources — exploration, NPCs, charts, missions)
- Price history graphs per commodity per system
- Statistics dashboard (profit, systems visited, discoveries, enemies defeated, hostile systems cleared)
- [ ] Layout mockup

### Death / Recovery Screen
- "Ship Destroyed" notification
- Cargo looted percentage displayed
- Respawn location (last visited merchant) shown
- Options: Travel to recover ship, Buy new ship, View star map
- [ ] Layout mockup

---

## Theme and Styling

### Color Palette
| Element | Color | Hex | Notes |
|---------|-------|-----|-------|
| Background | Dark space blue | #0a0e1a | Main background |
| Panel BG | Dark gray | #1a1e2e | UI panel backgrounds |
| Primary text | Light gray | #d0d0d0 | Standard text |
| Headers | Cyan | #00ccff | Screen titles, headings |
| Credits/Money | Green | #00ff88 | Currency values |
| Damage/Danger | Red | #ff3344 | Warnings, low stats |
| Success | Bright green | #44ff44 | Positive outcomes |
| Lore/Flavor | Muted blue | #6688aa | Atmosphere text |
| Interactive | Gold/Yellow | #ffcc00 | Buttons, selectable items |
| Disabled | Dark gray | #444444 | Unavailable options |

### Typography
- [ ] Header font (bold, sci-fi style)
- [ ] Body font (readable, clean)
- [ ] Monospace font (data tables, numbers)
- [ ] Font sizes for different contexts

---

## Input Design

### Input Methods
- [ ] Mouse/click for menus, buttons, star map
- [ ] Keyboard shortcuts for common actions
- [ ] Gamepad support (controller navigation)
- [ ] Input mapping via Godot's InputMap

### Navigation
- [ ] Tab/arrow key navigation between UI elements
- [ ] Focus indicators for keyboard/gamepad users
- [ ] Context-sensitive action hints

### Input Feedback
- [ ] Button hover and press animations
- [ ] Confirmation dialogs for major actions (selling ship, dangerous travel)
- [ ] Tooltip system for item/planet details
- [ ] Sound effects on UI interactions

---

## Transitions and Animation

### Scene Transitions
- [ ] Fade to black between scenes
- [ ] Hyperspace / warp effect for travel
- [ ] Slide-in panels for sub-menus

### UI Animations
- [ ] Tween-based panel open/close
- [ ] Number roll-up for credit changes
- [ ] Shake effect for damage
- [ ] Pulsing indicators for alerts/notifications
- [ ] Typewriter effect for story text via `RichTextLabel`

---

## Visual Effects

### Space Environment
- [ ] Parallax star field background
- [ ] Planet sprites with atmospheric glow
- [ ] Ship sprites with engine particle effects
- [ ] Hyperspace tunnel / warp lines

### Combat Visuals
- [ ] Weapon fire effects (particles, sprites)
- [ ] Shield impact flashes
- [ ] Explosion effects
- [ ] Ship damage states (visual hull damage)

---

## Accessibility
- [ ] Colorblind-friendly palette (alternate themes)
- [ ] Scalable UI (font size options)
- [ ] Screen reader support (Godot accessibility features)
- [ ] High contrast mode
- [ ] Remappable controls
- [ ] Adjustable text speed for narrative

---

## Resolution and Display
- [ ] Base resolution (e.g., 1920x1080)
- [ ] Stretch mode (viewport vs. canvas_items)
- [ ] Fullscreen / windowed toggle
- [ ] Multiple resolution support
- [ ] Minimum display requirements

---

## Notes
_Add design notes, open questions, and decisions here._
