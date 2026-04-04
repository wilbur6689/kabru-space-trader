# Stage 10: Polish & Release

## Goal
Transform the functional game into a polished, release-ready product. This stage covers audio implementation, UI art replacement, visual effects, game balancing, performance optimization, accessibility, platform export, and final QA. Nothing new is built — everything is refined.

**Design Docs:** [11_audio_and_atmosphere.md](../docs/11_audio_and_atmosphere.md), [10_ui_and_presentation.md](../docs/10_ui_and_presentation.md), [12_balancing.md](../docs/12_balancing.md)
**Prerequisites:** Stages 01-09 complete (all gameplay systems and content)
**Outputs:** Release-ready build with full audio, polished art, balanced gameplay, platform exports

---

## Phase 10.1 — Audio Implementation

**Goal:** Replace all audio stubs with real music and sound effects. The game should sound atmospheric and immersive.

### Tasks
- [ ] Source or create music tracks for the dual-style soundtrack:
  - **Dark Ambient tracks** (5-8 tracks):
    - Docked at friendly system
    - Exploring/searching
    - Trading at marketplace
    - Dialog/NPC conversations
    - Journal/menus
    - Dead system atmosphere
    - Drifting in empty space
  - **Synthwave tracks** (4-6 tracks):
    - Standard combat
    - Boss combat (Warlord encounters)
    - Pirate-controlled system exploration
    - High-stakes travel events
    - Black market dealings
    - System cleared victory
- [ ] Implement music transitions in AudioManager:
  - Crossfade (2-3 sec) for calm transitions (arriving, opening marketplace)
  - Sharp cut for urgent moments (combat triggered, danger discovered)
  - Ensure smooth looping for all tracks
- [ ] Source or create sound effects:
  - **UI sounds** (~10): Button click, panel open/close, tab switch, notification chime, error beep, confirm, cancel, scroll, address input keypress, save complete
  - **Ship sounds** (~14): Engine idle hum, jump drive charge, jump execute, jump arrive, weapon fire, shield hit, hull damage, shield down, hull critical, repair sounds, cargo load/unload, scanner sweep, docking
  - **Combat sounds** (~6): Attack impact, defend block, flee attempt, negotiate, intimidate success, enemy defeat
  - **Event sounds** (~5): Black market delivery, boss warning siren, undercover bust alarm, system cleared fanfare, death sequence
  - **Environment ambience** (~6): Space ambient (per system type), mining system, dead system wind, pirate territory tension, station interior, asteroid field
- [ ] Wire all SFX into existing systems:
  - Replace all `AudioManager.play_sfx("stub")` calls with real sound names
  - Add SFX triggers to any actions currently missing them
- [ ] Implement volume settings:
  - Music volume slider
  - SFX volume slider
  - Master volume
  - Mute toggle
  - Save preferences

### Verification
- Every action produces appropriate audio feedback
- Music transitions feel smooth and contextually appropriate
- No audio pops, clicks, or jarring cuts
- Volume controls work correctly
- Audio doesn't loop awkwardly

---

## Phase 10.2 — Cockpit Art and UI Polish

**Goal:** Replace placeholder rectangles and debug text with polished cockpit artwork and styled UI elements.

### Tasks
- [ ] Commission or create cockpit interior artwork:
  - Main cockpit view with clear interactive zones
  - Must accommodate: left monitors, console screens, window, cargo area, dashboard
  - Variations per ship class (stretch goal — start with one universal cockpit)
- [ ] Implement cockpit zones as interactive overlays on the artwork:
  - Clickable regions mapped to art features (monitors, screens, buttons)
  - Hover highlights to indicate interactability
  - Smooth panel transitions when switching zone content
- [ ] Polish all UI panels:
  - Apply consistent theme (dark panels, cyan headers, monospace body text)
  - Add borders, shadows, and subtle glow effects
  - Ensure text readability at all zoom levels
  - Add icons for: discovery types, good categories, mission types, faction logos, ship classes
- [ ] Implement visual feedback effects:
  - **Screen shake**: On hull damage (intensity scales with damage)
  - **Cockpit flicker**: On critical damage (hull below 25%)
  - **Vignette darkening**: Progressive as hull decreases
  - **Shard popups**: Green +X or red -X floating numbers on transactions
  - **Damage numbers**: Floating damage values during combat
  - **Shield shimmer**: On shield hit
  - **Discovery glow**: Brief highlight when new location found
- [ ] Polish notification system:
  - Slide-in notifications from cockpit edge
  - Color-coded by type (info, success, warning, danger)
  - Stack multiple notifications
  - Auto-dismiss with manual close option
- [ ] Create loading/transition screens:
  - Jump travel animation (stars stretching, tunnel effect)
  - System arrival transition
  - Combat entry transition

### Verification
- Cockpit art looks professional and zones are clearly identifiable
- All UI panels are styled consistently
- Visual feedback effects fire at correct moments
- No UI elements overlap or clip
- Transitions feel smooth and add to immersion

---

## Phase 10.3 — Star Map and Navigation Polish

**Goal:** Create a visual star map for navigation that shows known systems, routes, and galaxy structure.

### Tasks
- [ ] Create `scenes/ui/star_map.tscn`:
  - Full galaxy view showing quadrant grid
  - Zoom levels: galaxy → quadrant → region → system cluster
  - Color-coded by: danger zone, faction territory, alignment
- [ ] Show known systems as interactive points:
  - Visited systems: solid dots with names
  - Known but unvisited: hollow dots
  - Current location: highlighted marker
  - Player stations: station icon
- [ ] Show route visualization:
  - Draw lines between systems when plotting a course
  - Color by: within range (green), out of range (red)
  - Show hop count on route
- [ ] Show region info on hover:
  - Region address, danger zone, dominant faction
  - Trade specialties (if visited)
  - System count (discovered/total)
- [ ] Implement click-to-navigate:
  - Click a known system to auto-fill navigation address
  - Confirm travel from star map
- [ ] Show jump drive range overlay:
  - Circle/diamond showing reachable regions based on current jump drive tier
  - Clearly indicates what's accessible and what needs upgrades

### Verification
- Star map accurately reflects known galaxy state
- Zoom levels are smooth and readable
- Route visualization shows correct hop counts
- Jump range overlay matches actual jump drive capabilities
- Click-to-navigate works correctly

---

## Phase 10.4 — Game Balancing Pass

**Goal:** Tune all numerical values so the game feels fair, challenging, and rewarding across the full progression curve.

### Tasks
- [ ] **Economy balance**:
  - Verify starting 2,000 shards allows meaningful early trades
  - Confirm trade route profits scale appropriately with distance and risk
  - Test: player should afford first ship upgrade around turn 30-50
  - Test: premium ships reachable around turn 100-200
  - Verify market event frequency doesn't frustrate or overwhelm
  - Balance haggle success rates and discount amounts
- [ ] **Combat balance**:
  - Verify early enemies (fame 1-3) are manageable for new players
  - Verify mid-game enemies (fame 4-7) require ship upgrades to handle
  - Verify Warlords are challenging but fair
  - Test flee success rates — should feel risky but possible
  - Balance intimidation — should feel rewarding without trivializing combat
  - Verify loot rewards match risk level
- [ ] **Progression balance**:
  - Test XP curve: skills should level at a satisfying pace
  - Verify fame points feel impactful (+3% per point should be noticeable)
  - Check mission slot gating doesn't bottleneck early game
  - Verify reputation changes at appropriate rate
- [ ] **Ship and component balance**:
  - Verify each ship tier feels like a meaningful upgrade
  - Check component tier pricing against expected player wealth at that stage
  - Verify jump drive tiers gate content appropriately (not too fast, not too slow)
  - Balance repair costs vs. income
- [ ] **Station balance**:
  - Verify station construction costs are achievable but significant
  - Check raid frequency is annoying but not devastating
  - Verify Tier 3 stations provide sufficient benefit to justify investment
- [ ] **Difficulty curve**:
  - Safe zones (distance 1-3) should be comfortable for new players
  - Low-Med zones (4-6) should feel noticeably harder
  - Med-High zones (7-9) should require upgraded ships and skills
  - Extreme zones (10+) should challenge max-level players
- [ ] Create balancing spreadsheet or tool:
  - Simulate progression through expected play patterns
  - Track shard income vs. expenses over time
  - Verify no dead spots where player can't afford anything useful

### Verification
- Playtest from new game through 100+ turns
- Progression feels rewarding at every stage
- No point where player feels stuck with no path forward
- Death setback feels consequential but not rage-inducing
- Economy has meaningful decisions (not just "always buy the most expensive good")

---

## Phase 10.5 — Save System Hardening

**Goal:** Ensure the save system is bulletproof — handles corruption, version migration, edge cases, and player expectations.

### Tasks
- [ ] Implement save file version migration:
  - When loading a save from an older version, upgrade data format
  - Add missing fields with sensible defaults
  - Log migration actions
- [ ] Implement save file validation:
  - Check for corrupted JSON
  - Validate required fields exist
  - Handle partial saves gracefully (don't crash, show error)
- [ ] Implement save file backup:
  - Before overwriting, create .bak copy
  - Keep last 2 backups per slot
- [ ] Test edge cases:
  - Save during combat → should not be possible (or handle gracefully)
  - Save during travel → should save at last system
  - Save with full inventory → all cargo preserved
  - Load save with missing discovery data → regenerate from seed
  - Load save referencing deleted game content → handle gracefully
- [ ] Implement save slot UI:
  - Show save metadata: pilot name, turn count, star date, current system, shards
  - Timestamp of save
  - "Load" and "Delete" buttons with confirmation
  - New game vs. continue flow on main menu

### Verification
- Saves from older versions load correctly with migration
- Corrupted saves show error message, don't crash
- Backup files created on every save
- All edge cases handled without data loss
- Save slot UI shows accurate metadata

---

## Phase 10.6 — Main Menu and Settings

**Goal:** Build the main menu and settings screens for a polished game entry experience.

### Tasks
- [ ] Create `scenes/main/main_menu.tscn`:
  - Game title and atmospheric background
  - "New Game" button → pilot creation
  - "Continue" button → load most recent save
  - "Load Game" button → save slot browser
  - "Settings" button → settings screen
  - "Quit" button
- [ ] Create settings screen:
  - **Audio**: Music volume, SFX volume, Master volume, Mute
  - **Display**: Window mode (windowed/fullscreen), resolution
  - **Gameplay**: Auto-save toggle, notification duration, text speed
  - **Controls**: Key rebinding (if applicable)
  - Save settings to `user://settings.json`
  - Apply settings on load
- [ ] Implement pause menu (in-game):
  - Resume, Save, Load, Settings, Main Menu, Quit
  - Confirm "Main Menu" and "Quit" to prevent accidental loss

### Verification
- Main menu loads cleanly with no errors
- All buttons navigate to correct screens
- Settings save and persist across sessions
- Pause menu accessible during gameplay
- Quit confirmation prevents accidental exits

---

## Phase 10.7 — Performance Optimization

**Goal:** Ensure the game runs smoothly on target hardware with no hitches, long load times, or memory leaks.

### Tasks
- [ ] Profile performance:
  - Galaxy generation time (should be < 1 second for any region)
  - Save/load time (should be < 2 seconds)
  - Scene transitions (should be < 0.5 seconds)
  - Memory usage (monitor for leaks during long play sessions)
- [ ] Optimize galaxy generation:
  - Generate regions on-demand, not all at startup
  - Cache generated regions (LRU cache with limit)
  - Free distant region data when memory is high
- [ ] Optimize UI:
  - Don't rebuild lists every frame — only on data change
  - Use object pooling for repeated UI elements (discovery list items, trade goods rows)
  - Lazy-load panels that aren't visible
- [ ] Optimize save files:
  - Compress large save files (consider binary format for release)
  - Only save changed data (delta saves for economy state)
- [ ] Test on minimum spec hardware:
  - Define minimum specs
  - Profile and optimize for target frame rate

### Verification
- No visible frame drops during gameplay
- Galaxy generation is imperceptible to the player
- Save/load feels instant
- Memory usage is stable over 1000+ turns
- Meets minimum spec requirements

---

## Phase 10.8 — Platform Export and QA

**Goal:** Export the game for target platforms and perform final quality assurance.

### Tasks
- [ ] Configure Godot export presets:
  - **Windows**: .exe with icon and metadata
  - **macOS**: .app bundle (if building on Mac)
  - **Linux**: .x86_64 binary
  - **Web/HTML5** (stretch goal): Browser-playable version
- [ ] Test each platform export:
  - Game launches correctly
  - Save/load works at correct paths
  - Audio plays correctly
  - No platform-specific crashes
- [ ] Final QA pass:
  - Play through tutorial start to finish
  - Test all 7 mission types to completion
  - Test death and respawn cycle
  - Test pirate system clearing end-to-end
  - Test station building all 3 tiers
  - Test black market delivery flow
  - Test save/load at every stage
  - Test edge cases: empty cargo trades, max ship ownership, max fame, etc.
- [ ] Create known issues list:
  - Document any remaining bugs with severity
  - Document any deferred features
  - Document any platform-specific issues
- [ ] Prepare release materials:
  - Game description text
  - Screenshots (5-10 showing key gameplay moments)
  - Version number
  - Changelog

### Verification
- Game exports and runs on all target platforms
- No game-breaking bugs in QA pass
- Tutorial is completable by someone who's never played
- All core loops function without crashes
- Save files portable between sessions

---

## Stage 10 Completion Checklist

- [ ] Full soundtrack implemented with contextual transitions
- [ ] All SFX implemented for every action and event
- [ ] Cockpit art replaces placeholder UI
- [ ] Visual feedback effects (shake, flicker, popups) all functional
- [ ] Star map provides useful visual navigation
- [ ] All numerical values balanced across full progression
- [ ] Save system handles corruption, migration, and edge cases
- [ ] Main menu and settings fully functional
- [ ] Performance meets targets on minimum spec
- [ ] Game exports for Windows, macOS, Linux
- [ ] Final QA pass completed with no game-breaking bugs

**The game is release-ready.**

---

## Post-Release Considerations

Items from the design docs that are explicitly deferred or marked as stretch goals:

- [ ] Crew system (hire crew members with roles and story arcs)
- [ ] Multiplayer trading
- [ ] Modding support
- [ ] Mobile export (Android/iOS)
- [ ] Full procedural galaxy generation (beyond seed-based)
- [ ] Faction war system with dynamic territory control
- [ ] Blueprint crafting system (partially stubbed at Tier 3 stations)
- [ ] Multiple damage types and shield variants
- [ ] Dead system connected lore
- [ ] Full main story arc (Acts 1-3) — may ship with Act 1 only
