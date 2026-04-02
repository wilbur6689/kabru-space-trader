# Audio and Atmosphere

## Overview
This document covers audio design, visual atmosphere, writing tone, and immersion elements powered by Godot's audio and rendering systems. Audio follows a **dual-style approach**: dark ambient for exploration and non-active scenes, synthwave for action and risky moments.

---

## Audio Architecture

### Godot Audio System
- Audio buses for separate volume control
- `AudioStreamPlayer` for non-positional audio (music, UI sounds)
- `AudioManager` autoload singleton for global playback control
- **Cinematic audio perspective** — full rich sound regardless of source (big explosions, dramatic weapon fire, clear alerts)

### Audio Bus Layout
```
Master
├── Music        # Background music tracks (dark ambient + synthwave)
├── SFX          # Sound effects (combat, trade, events, ship)
├── Ambient      # Background ambience loops (space, system type)
└── UI           # Menu clicks, notifications, inbox
```

### Volume Controls
- **Simple for now**: Master volume slider + Music volume slider
- Per-bus sliders (SFX, Ambient, UI) deferred to post-MVP

---

## Music

### Dual-Style Soundtrack
- **Dark ambient**: Used for exploration, docked scenes, traveling, trading, dialogue — atmospheric, immersive, keeps the mood grounded
- **Synthwave (retro-futuristic)**: Used for combat, risky encounters, boss fights, black market deals — high energy, adrenaline, signals danger/action

### Music Transitions
- **Context-dependent transitions**:
  - **Crossfade** (2-3 seconds) for calm transitions: arriving at a system, opening marketplace, returning from menus
  - **Sharp cut** for urgent moments: combat triggered, dangerous discovery, boss warning, intimidation failure
- The shift from ambient to synthwave is itself a signal to the player that something is happening

### Music Track List

#### Dark Ambient Tracks
| Context | Mood | Transition In |
|---------|------|--------------|
| Main Menu | Atmospheric, grand | N/A — plays on launch |
| Friendly System | Calm, safe, warm | Crossfade on arrival |
| Dead System | Eerie, haunting, hollow | Crossfade on arrival |
| Hostile System | Brooding, threatening, tense | Crossfade on arrival |
| Hub System | Warmer ambient, busier undertones | Crossfade on arrival |
| Traveling (Known Route) | Relaxed, steady | Crossfade on departure |
| Traveling (Unknown Space) | Building tension, mysterious | Crossfade on departure |
| Search / Discovery | Curious, anticipation building | Crossfade when searching |
| Marketplace | Warm, trading atmosphere | Crossfade when opening market |
| Story / Dialogue | Subtle, doesn't overpower text | Crossfade on dialogue start |
| Death / Recovery | Somber, then slowly hopeful | Crossfade from combat |

#### Synthwave Tracks
| Context | Mood | Transition In |
|---------|------|--------------|
| Combat | Intense, urgent, adrenaline | **Sharp cut** on encounter |
| Black Market Deal | Slower synthwave, shady tension | **Sharp cut** on deal initiation |
| Boss Warning / Boss Fight | Heaviest track, maximum intensity | **Sharp cut** on boss arrival |
| Victory / System Cleared | Triumphant, achievement rush | Crossfade from combat |

**Note:** Detailed music composition and track specifics deferred for later revisit. See deferred design doc.

---

## Sound Effects

### UI Sounds
| Action | Sound | Notes |
|--------|-------|-------|
| Button click | Soft click/beep | Consistent across all menus |
| Button hover | Subtle tone | Light feedback |
| Purchase/sell | Cash register / coin | Satisfying trade feedback |
| Menu open/close | Whoosh / slide | Panel transitions on cockpit monitors |
| Notification | Chime / ping | Alerts and messages |
| Error / invalid | Buzz / negative tone | Denied actions |
| Haggle success | Positive chime / deal struck | Bartering win |
| Haggle failure | Negative buzz / price hike | Bartering loss |
| Message received | Inbox ping | New message in inbox |
| Confirmation dialog | Soft alert tone | "Are you sure?" prompts |

### Ship Sounds
| Action | Sound | Notes |
|--------|-------|-------|
| Engine idle | Low hum | Ambient while in system |
| Jump drive charge | Building energy whine | Before each jump |
| Jump initiation | Whoosh / warp boom | Each hop in multi-jump route |
| Jump arrival | Deceleration boom | Arriving at new system |
| Weapon fire | Laser / projectile | Combat attacks |
| Shield hit | Energy crackle | Taking damage (shields absorb) |
| Hull damage | Metal crunch | Direct hull hits |
| Explosion | Boom | Ship destruction |
| Repair | Welding / mechanical | Repair sequence |
| Search ping | Scanner sweep | When searching a system |
| Discovery chime | Positive reveal tone | New location discovered |
| Danger alert | Warning klaxon | Dangerous discovery found |
| Station deployed | Heavy mechanical clunk | Personal station placed |
| Ship transfer complete | Docking confirmation tone | Stored ship arrived at destination |

### Combat Sounds
| Action | Sound | Notes |
|--------|-------|-------|
| Intimidation success | Enemy comms cutting out | They're fleeing |
| Intimidation failure | Angry comms burst | They're attacking harder |
| Flee attempt | Engine surge | Boosting away |
| Negotiate offer | Comms open tone | Attempting to talk |
| Enemy defeated | Explosion + victory sting | Combat win |
| Player defeated | Hull breach + alarm | Leads to death screen |

### Event Sounds
| Action | Sound | Notes |
|--------|-------|-------|
| Black market delivery | Mysterious package thud | Delivery notification |
| Boss warning | Deep alarm / threatening transmission | 2-3 turns until attack |
| Undercover bust | Sirens / authority alert | Black market deal gone wrong |
| Bribe attempt | Tense silence / coin slide | Offering shards |
| System cleared | Triumphant horn + ambient shift | Hostile → friendly conversion |

### Environment Sounds
| Context | Sound | Notes |
|---------|-------|-------|
| Space ambient | Deep space hum | Subtle, always present in cockpit |
| Friendly system | Wind, city, nature | Varies by sub-type (mining, industrial, etc.) |
| Dead system | Silence, creaking debris | Eerie emptiness |
| Hostile system | Low threat hum, distant alarms | Tension always present |
| Hub station | Machinery, chatter, bustle | Center of activity |
| Combat ambient | Alarms, tension | During encounters |

**Note:** Full SFX list and audio asset details deferred for later revisit. See deferred design doc.

---

## Visual Atmosphere

### Cockpit Window Visuals
All visuals are displayed through the cockpit windows — the cockpit interior is always present.

#### System Visuals by Type
- **Friendly systems**: Vibrant, populated, warm lighting — varies by sub-type:
  - Mining colony: Rocky, industrial machinery, amber tones
  - Industrial colony: Factories, smoke stacks, metallic
  - Agricultural: Green fields, biodomes, natural light
  - Uninhabited: Lush, green, untouched nature
  - Primitive race: Exotic architecture, natural materials
- **Dead systems**: Dark, muted colors, debris fields, eerie glow
  - Dead worlds: Cracked surfaces, ruined cities
  - Asteroid fields: Tumbling rocks, dust clouds
  - Destroyed chunks: Massive fragments, energy residue
- **Hostile systems**: Red/orange warning tones, fortified structures, patrol ships
- **Hub systems**: Massive stations, busy traffic, bright lights, civilization
- **Empty space**: Distant stars, void, cosmic dust

#### Travel Visuals
- Hyperspace tunnel overlay on cockpit windows during jumps
- Stars streaking by through the glass
- Brief flash/pause between multi-hop jumps

#### Title Screen
- Animated space backdrop with parallax star field
- Ship cockpit or exterior artwork
- Atmospheric with main menu dark ambient music

---

## Visual Feedback Effects

### All Included from Launch

| Effect | Trigger | Implementation |
|--------|---------|---------------|
| **Screen shake** | Hull damage taken in combat | Camera/viewport shake, intensity scales with damage |
| **Cockpit sparks/flicker** | Critical hull damage | Overlay particle effect + brief screen flicker |
| **Vignette darkening** | Hull below 25% | Screen edges darken progressively as hull drops |
| **Shard number popups** | Shard gains/losses | "+500 shards" / "-200 shards" floats up, color-coded green/red |
| **Damage number popups** | Combat hits | Damage dealt/received floats near ship status |

---

## Writing and Tone

### Overall Style
- **Atmospheric but brief** for general gameplay — a sentence or two of flavor, then straight to business
- **Detailed and immersive** for main story content — full narrative passages
- Gritty, industrial tone matching the visual aesthetic

### Arrival Text Variants

#### First Visit (Never Been Here)
Atmospheric description, 2-3 sentences setting the scene:
> You drop out of hyperspace above a scorched rock orbiting a dying star. The surface is pocked with strip mines, their amber lights cutting through clouds of dust. A comms beacon crackles: *"Voss Prime Mining Colony. Dock at platform 7."*

#### Return Visit (Previously Visited)
Quick status line:
> Returning to Voss Prime. Mining colony. 3/4 locations discovered.

#### Known But Unvisited (Address Known, Never Visited)
Brief intel-based text:
> Scans indicate a mining settlement at this location. Mineral signatures detected. Approach with caution — limited intel available.

### Event Narration
- **Combat**: Terse, tactical — "Void Reaver interceptor closing fast. Weapons hot."
- **Discovery**: Brief wonder or tension — "Scanner resolves a faint signal... an abandoned trading post, half-buried in asteroid debris."
- **Trading**: Factual, quick — "Transaction complete. 45 units of refined metals sold."
- **Black market**: Shady atmosphere — "A crate slides out of the shadows. This is it — your delivery."

### NPC Dialogue Style
- Conversational, varied by NPC type
- Hub NPCs: Professional, informative
- Sub-system NPCs: Rougher, more casual
- Pirate contacts: Guarded, cryptic
- Example: *"Oh yeah, I was just at system 11-321 and here are the prices I saw last time I was there."*

---

## Accessibility for Audio (Post-MVP)

Deferred to after MVP:
- [ ] Visual indicators alongside all audio cues
- [ ] Independent volume sliders per audio bus (currently only Master + Music)
- [ ] Mute toggle per category
- [ ] Subtitles for any future voiced content

---

## Notes
_Add design notes, open questions, and decisions here._

### Open Design Questions
- Specific music track composition and sourcing (royalty-free, commissioned, generated)
- Full SFX asset list and sourcing
- Environment sound variations per friendly sub-type
- How many unique arrival descriptions are needed per system sub-type
- Dynamic music layering complexity (simple crossfade vs. stem-based mixing)
- Audio file formats and compression for Godot (OGG vs WAV)
