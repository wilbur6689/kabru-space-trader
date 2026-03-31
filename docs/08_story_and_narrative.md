# Story and Narrative

## Overview
This document defines the game's story, lore, world-building, and narrative structure. The main story arc is **player-triggered** — it only advances when the player chooses to engage. The world waits for the player.

---

## Setting

### Universe
- [ ] Time period (far future, near future, alternate history)
- [ ] Scale — galaxy organized into **regional clusters** of systems connected by jump drive travel
- [ ] History of the universe (how did humanity get here?)
- [ ] Major factions and powers controlling different clusters/regions
- [ ] State of the galaxy — mix of friendly, dead, and hostile systems reflecting a complex political landscape

### Tone
- [ ] Gritty and realistic vs. lighthearted and adventurous
- [ ] Hard sci-fi vs. space opera
- [ ] Dark themes or family-friendly
- [ ] Humor level

---

## Main Story Arc

### Prologue / Tutorial
The player begins **stranded** with a broken ship orbiting a lesser-known planet in a sub-system. Their dilapidated spaceship broke down while on a routine delivery pickup. The player must:
1. Repair the ship (teaches searching, discovery, and repair mechanics)
2. Purchase new parts (teaches trading and NPC interaction)
3. Figure out how to navigate to the destination (teaches 6-digit address system and jump travel)
4. Deliver the cargo to a **city space station** at the regional hub system
5. Upon delivery, the tutorial ends and the **main story hook** is revealed

### Act 1 - The Beginning
- [ ] What does the player discover at the hub station after the delivery?
- [ ] What is the inciting incident that launches the main story?
- [ ] What is the player's initial story goal beyond trading?
- [ ] How are the major factions introduced?

### Act 2 - The Rising Action
- [ ] What larger conflict is revealed?
- [ ] Key story missions (triggered by player at hub systems)
- [ ] Faction involvement and allegiance choices
- [ ] Player choices that affect the story
- [ ] How does clearing hostile systems tie into the main narrative?

### Act 3 - The Climax and Resolution
- [ ] What is the final challenge?
- [ ] Multiple endings based on faction loyalty and player choices?
- [ ] Post-story free play? (continue exploring and trading after credits)

### Story Pacing
- Story advances **only when the player triggers it** — no timers or deadlines
- Main story missions are available at hub systems
- Player can spend as much time trading, exploring, and clearing systems as they want between story beats
- Story events can trigger when entering **new regions** for the first time

---

## Factions

### Major Factions
Factions control different regional clusters and systems. The player's reputation with each affects gameplay.

| Faction | Description | Territory | Attitude | Specialization |
|---------|-------------|-----------|----------|----------------|
| | | | | |
| | | | | |
| | | | | |
| | | | | |

### Hostile Factions
- Gangs, warlords, and rogue armies that control **hostile systems**
- Each hostile system cluster has a controlling faction with a leader
- Defeating the leader converts the system to friendly
- Hostile factions may be connected to the main story arc

### Faction Relationships
- [ ] Faction alliance/rivalry matrix
- [ ] How does reputation with one faction affect others?
- [ ] Can the player be allied with opposing factions?
- [ ] Faction wars that change system hostility across regions
- [ ] Do cleared hostile systems come under a friendly faction's control?

---

## Characters / NPCs

### Key NPCs
| Name | Role | Location | Purpose |
|------|------|----------|---------|
| | Mentor/Guide | Starting sub-system | Helps during tutorial, gives first delivery mission |
| | Hub Contact | Regional hub station | Introduces main story, provides key missions |
| | Rival Trader | Roaming | Recurring competitor, may become ally or enemy |
| | Informant | Various hubs | Sells system addresses, tips, and intel |
| | Faction Leader | Hub systems | Faction quest lines and story involvement |
| | Warlord/Boss | Hostile systems | Antagonist controlling hostile territory |

### NPC Information Roles
NPCs serve as a key source of galaxy knowledge:
- **Merchants** share trade tips and price information
- **Locals** warn about hostile neighbors and regional dangers
- **Information brokers** sell system addresses and star charts
- **Quest givers** provide destination addresses as part of mission briefings
- **Rescued NPCs** (from hostile systems) may offer loyalty discounts or rare intel

### NPC Interactions
- [ ] Dialogue system (branching, linear, keyword-based?)
- [ ] Relationship tracking with key NPCs
- [ ] Do NPCs move between systems or stay fixed?
- [ ] NPC loyalty from rescue missions

---

## Lore and World-Building

### History
- [ ] Timeline of major events leading to current galaxy state
- [ ] How did interstellar travel (jump drives) develop?
- [ ] What happened to Earth?
- [ ] Origin of the hostile factions — why do gangs/armies control some systems?
- [ ] What destroyed the dead systems? Ancient war? Natural disaster? Unknown force?

### Cultures and Societies
- [ ] Distinct cultures on different planet types
- [ ] Primitive race societies (how do they interact with traders?)
- [ ] Hub system culture vs. frontier sub-system culture
- [ ] How do different factions view traders/outsiders?

### Technology
- **Jump Drives** — How FTL travel works via the 6-digit coordinate system
- [ ] Communication systems between systems
- [ ] AI and automation in this universe
- [ ] Weapons technology
- [ ] Why are some systems dead? What technology was lost?

---

## Side Stories and Quests

### Side Quest Sources
- Hub system mission boards and bulletin boards
- NPC conversations at various locations
- Discoveries during system searches
- Events triggered when entering new regions
- Rescue missions in hostile systems

### Quest Types
- **Delivery chains** — Multi-hop cargo runs with escalating stakes
- **Exploration quests** — Chart specific unknown addresses for a patron
- **Bounty hunts** — Track and eliminate targets across multiple systems
- **Rescue operations** — Free imprisoned NPCs from hostile systems
- **Investigation** — Gather intel from data caches in hostile/dead systems
- **Clearance contracts** — Liberate a hostile system for a faction
- **Mystery/Lore** — Uncover what happened to dead systems and ancient civilizations

### Emergent Narrative
- Random events during travel create unplanned stories
- Discovering what happened to dead systems through search results
- Player's trading choices shape their relationship with the galaxy
- Clearing hostile systems changes the political landscape
- Consequences ripple — freeing one system may provoke a neighboring hostile faction

---

## Dialogue and Text

### Writing Style
- [ ] Voice and tone guidelines
- [ ] Text length targets (brief for trading, detailed for story moments)
- [ ] Flavor text for items, systems, discoveries
- [ ] How much lore is embedded in item/location descriptions vs. explicit dialogue?

### Text Delivery
- Story text presented via Godot `RichTextLabel` with styled BBCode formatting
- Dialogue at NPC interactions — key story moments
- Discovery descriptions when searching systems
- Event narration during travel encounters
- Bulletin board postings at hub systems
- Skippable for repeat content, mandatory for first-time story beats
- All text logged in the **player's journal** for review

---

## Notes
_Add design notes, open questions, and decisions here._

### Open Design Questions
- Main story arc details (Acts 1-3 content)
- Faction names, descriptions, and territories
- Key NPC names and personalities
- Number of side quest chains
- Lore behind dead systems and ancient civilizations
- Connection between hostile faction leaders and main story villain
