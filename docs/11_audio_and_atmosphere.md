# Audio and Atmosphere

## Overview
This document covers audio design, visual atmosphere, and immersion elements powered by Godot's audio and rendering systems.

---

## Audio Architecture

### Godot Audio System
- Audio buses for separate volume control (Master, Music, SFX, Ambient, UI)
- `AudioStreamPlayer` for non-positional audio (music, UI sounds)
- `AudioStreamPlayer2D` for positional audio (if using spatial scenes)
- `AudioManager` autoload singleton for global playback control

### Audio Bus Layout
```
Master
├── Music        # Background music tracks
├── SFX          # Sound effects (combat, trade, events)
├── Ambient      # Background ambience loops
└── UI           # Menu clicks, notifications
```

---

## Music

### Soundtrack Style
- [ ] Genre (synthwave, orchestral, ambient electronic, lo-fi space)
- [ ] Tone (epic, chill, tense, mysterious)
- [ ] Dynamic music (layers that change based on game state)

### Music Tracks Needed
| Context | Mood | Notes |
|---------|------|-------|
| Main Menu | Grand, inviting | Title theme |
| Friendly System | Calm, warm | Safe harbor feel |
| Dead System | Eerie, haunting | Desolate and mysterious |
| Hostile System | Gritty, tense | Danger imminent |
| Hub System | Bustling, lively | Center of civilization |
| Traveling (Known Route) | Adventurous, relaxed | Safe journey |
| Traveling (Unknown Space) | Mysterious, tense | Anything could happen |
| Search / Discovery | Curious, building | Anticipation of the unknown |
| Combat | Intense, urgent | Drives adrenaline |
| Marketplace | Bustling, lively | Trading energy |
| Story / Dialogue | Atmospheric, subtle | Doesn't overpower text |
| Victory / System Cleared | Triumphant | Major achievement feel |
| Death / Recovery | Somber, then hopeful | Loss but not the end |

### Music Transitions
- [ ] Crossfade between tracks on scene change
- [ ] Layered music that adds/removes instruments based on tension
- [ ] Stinger sounds for key moments (discovery, danger alert)

---

## Sound Effects

### UI Sounds
| Action | Sound | Notes |
|--------|-------|-------|
| Button click | Soft click/beep | Consistent across all menus |
| Button hover | Subtle tone | Light feedback |
| Purchase/sell | Cash register / coin | Satisfying trade feedback |
| Menu open/close | Whoosh / slide | Panel transitions |
| Notification | Chime / ping | Alerts and messages |
| Error / invalid | Buzz / negative tone | Denied actions |

### Ship Sounds
| Action | Sound | Notes |
|--------|-------|-------|
| Engine idle | Low hum | Ambient while in system |
| Jump drive charge | Building energy whine | Before each jump |
| Jump initiation | Whoosh / warp boom | Each hop in multi-jump route |
| Jump arrival | Deceleration boom | Arriving at new system |
| Weapon fire | Laser / projectile | Combat attacks |
| Shield hit | Energy crackle | Taking damage |
| Hull damage | Metal crunch | Critical hits |
| Explosion | Boom | Ship destruction |
| Repair | Welding / mechanical | Repair sequence |
| Search ping | Scanner sweep | When searching a system |
| Discovery chime | Positive reveal tone | New location discovered |
| Danger alert | Warning klaxon | Dangerous discovery found |

### Environment Sounds
| Context | Sound | Notes |
|---------|-------|-------|
| Space ambient | Deep space hum | Subtle, always present |
| Friendly system | Wind, city, nature | Varies by sub-type (mining, industrial, etc.) |
| Dead system | Silence, creaking debris | Eerie emptiness |
| Hostile system | Low threat hum, distant alarms | Tension always present |
| Hub station | Machinery, chatter, bustle | Center of activity |
| Combat ambient | Alarms, tension | During encounters |

---

## Visual Atmosphere

### Space Visuals
- [ ] Parallax scrolling star field (multiple layers)
- [ ] Nebulae and cosmic dust (shader-based or sprite)
- [ ] Planet approach animations
- [ ] Hyperspace visual effect (tunnel, streaking stars)

### System Visuals by Type
- **Friendly systems**: Vibrant, populated, warm lighting — varies by sub-type:
  - Mining colony: Rocky, industrial machinery, amber tones
  - Industrial colony: Factories, smoke stacks, metallic
  - Uninhabited: Lush, green, untouched nature
  - Primitive race: Exotic architecture, natural materials
- **Dead systems**: Dark, muted colors, debris fields, eerie glow
  - Dead worlds: Cracked surfaces, ruined cities
  - Asteroid fields: Tumbling rocks, dust clouds
  - Destroyed chunks: Massive fragments, energy residue
- **Hostile systems**: Red/orange warning tones, fortified structures, patrol ships
- **Hub systems**: Massive stations, busy traffic, bright lights, civilization
- [ ] Atmospheric glow and lighting effects per type
- [ ] Orbital view vs. surface view

### Ship Visuals
- [ ] Ship sprites per class with engine glow
- [ ] Damage states (visual hull damage progression)
- [ ] Upgrade visuals (visible component changes)
- [ ] Particle effects (engine thrust, weapon fire)

### Title Screen
- [ ] Animated title with space backdrop
- [ ] Parallax star field
- [ ] Ship flyby or planet rotation
- [ ] Atmospheric menu with music

---

## Writing and Tone

### Location Descriptions
- First arrival: Full atmospheric description in `RichTextLabel` with styled text
- Return visits: Brief status update
- [ ] Seasonal/time-based description variants

### Event Narration
- [ ] Combat narration style (dramatic, tactical, humorous)
- [ ] Trade narration (brief, factual)
- [ ] Story narration (detailed, immersive)
- [ ] Typewriter text reveal effect for key moments

### Example Tone
> You drop out of hyperspace above Kepler-7. The planet's amber atmosphere swirls beneath you, dotted with the glowing lights of mega-cities. Your comm crackles: *"Trader vessel, you are cleared for landing at Port Meridian. Docking fee: 50 credits."*

---

## Screen Shake and Juice

### Feedback Effects
- [ ] Camera shake on damage taken
- [ ] Screen flash on critical hits
- [ ] Chromatic aberration on low hull
- [ ] Vignette darkening when near death
- [ ] Number popups for damage / credits gained
- [ ] Particle bursts on explosions and discoveries

---

## Accessibility for Audio
- [ ] Subtitles for any voiced content
- [ ] Visual indicators alongside audio cues
- [ ] Independent volume sliders per audio bus
- [ ] Mute toggle per category

---

## Notes
_Add design notes, open questions, and decisions here._
