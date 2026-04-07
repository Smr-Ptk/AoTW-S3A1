# The Fractured Crown — Product Requirements Document

## 1. Concept

"The Fractured Crown" is a browser-based, RuneScape-styled interactive quest set in the fantasy kingdom of **Valdrun**. The user plays as a travelling chronicler arriving at Crownharbour during a period of political upheaval. Through NPC dialogue, branching choices, and visible dice rolls, the user navigates court intrigue across 5 turns. Each playthrough produces different outcomes — alliances shift, characters live or die, and the Fractured Crown changes hands.

This is a solo student project for an "Art of the Web" course. The site itself functions simultaneously as:
- **The script** (pseudo-code logic governs the experience)
- **The performance** (the user enacts it by playing)
- **The documentation** (an "About" section and playthrough log record what happened)

**Tone:** RuneScape — deadpan, slightly absurd medieval fantasy. NPCs speak in short, punchy lines. Humor comes from game-logic rigidity applied to serious political drama.

---

## 2. Rubric Requirements (MUST HIT ALL)

### Plan (5 pts)
- A written pseudo-code "score" is visible inside the site via a toggleable "Source Code" overlay panel
- This PRD serves as the planning document
- The pseudo-code is NOT syntactically valid JS — it's a creative interpretation of programming logic as a performance score

### Script (10 pts)
- Every NPC interaction is structured as a function: `talkTo(npc)`, `claimThrone(faction)`, `formAlliance(a, b)`, etc.
- Game state is passed between turns like function arguments
- Conditionals (`if aldric.alive`) change downstream dialogue and outcomes
- The script creatively interprets markup/programming logic but is NOT syntactically correct code

### Execute (10 pts)
- 5 turns with dialogue, choices, and dice = 3–5 minutes minimum per playthrough
- Element of **chance**: visible dice rolls determine battle outcomes, NPC honesty, survival
- Replayable — different outcomes each run
- Critical engagement: the thesis is that power structures are unstable systems where small chance events cascade into totally different histories

### Document (5 pts)
- The site includes an "About" page/section explaining the project, its references, and the concept
- A "Chronicle Log" records what happened in the current playthrough
- The pseudo-code overlay shows the "source" being executed

---

## 3. World and Character Reference

### Places

| Role in Story              | Name            |
|---------------------------|-----------------|
| The kingdom               | Valdrun         |
| The throne                | The Fractured Crown|
| The capital city          | Crownharbour    |
| Northern stronghold       | Greyholt        |
| The great wall            | The Barrier     |
| Western seat of power     | Aurum Keep      |
| Southeastern coast        | Storrund Bay    |
| Land across the sea       | The Ember Isles |

### Houses / Factions

| Role in Story                    | House Name   | Color        |
|---------------------------------|-------------|-------------|
| Honourable northern lords        | Ashward     | Grey #808080 |
| Wealthy, scheming western house  | Golmere     | Gold #ffd700 |
| Warrior kings, current dynasty   | Storrund    | Dark gold #daa520 |
| Exiled dragonlords               | Vaelthorne  | Deep red #cc0000 |
| Defenders of the Barrier         | Barrier Watch| Near black #1a1a1a|
| The undead threat                | Pale Walkers | Ice blue #a8d8ea |

### Characters

| Role in Story                          | Name              |
|---------------------------------------|-------------------|
| The honourable lord, asked to serve    | Aldric Ashward    |
| The king, old friend of Aldric        | Beren Storrund    |
| The queen, ruthless schemer           | Elara Golmere     |
| The queen's clever brother, sharp wit | Halric Golmere    |
| The cruel prince, claims the throne   | Maegor Storrund   |
| The exiled heir with dragons          | Lysara Vaelthorne |
| The bastard of the north, guards the Barrier | Corwin Frost |
| The whisperer, broker of secrets      | Aldren Thorne     |

---

## 4. Game Structure — 5 Turns

### Turn 1: "The King's Summons"

**Setting:** Greyholt. Beren Storrund arrives with his retinue.

**NPCs present:** Beren Storrund, Aldric Ashward

**What happens:**
- Beren asks Aldric to serve as King's Hand
- User (as chronicler) witnesses the exchange
- **CHOICE:** Advise Aldric to "Accept the summons" or "Refuse and stay north"
- **DICE ROLL:** Determines whether Aldric senses danger (roll 1-3: no warning, roll 4-6: a raven arrives with an ominous message that modifies Aldric's dialogue — he goes south wary instead of trusting)

**Pseudo-code displayed:**
```
function kingSummons(aldric, beren) {
    beren.speak("I need someone I can trust. Come south.")
    let omen = roll(1, 6)
    if (omen >= 4) {
        aldric.suspicion = true
        aldric.speak("Something feels wrong... but duty calls.")
    }
    let choice = chronicler.choose("Accept", "Refuse")
    if (choice == "Refuse") {
        aldric.alive = true
        aldric.location = "Greyholt"
        return skip_to_turn_4
    }
    aldric.location = "Crownharbour"
    return next_turn
}
```

**NPC Dialogue:**

Beren:
- "The realm needs a steady hand. Mine shakes from wine. Come south, old friend."
- "I didn't ride all this way for the scenery, Aldric."

Aldric (no suspicion):
- "You honour me, Your Grace. I'll ride at dawn."

Aldric (suspicion = true):
- "A raven came in the night. Unsigned. Three words: 'Trust no one.' ...I'll ride at dawn anyway."

Aldric (if user picks Refuse):
- "The North is where I belong. Send someone who wants power. I never did."

---

### Turn 2: "The Court of Whispers"

**Setting:** Crownharbour. The throne room and its corridors.

**NPCs present:** Elara Golmere, Halric Golmere, Aldren Thorne

**What happens:**
- Aldric has arrived at court (if he accepted). He needs information about the previous Hand's death.
- **CHOICE:** The user picks **2 of 3** NPCs to consult (time is limited before the feast)
- **DICE ROLL (per NPC consulted):** Roll 1-3 = NPC lies or deflects. Roll 4-6 = NPC reveals something true.
- Based on results, Aldric may or may not discover the secret: Maegor is not Beren's true heir.

**If Aldric refused in Turn 1:** This turn is replaced with a short interlude — the chronicler hears rumors from a travelling merchant NPC about chaos in Crownharbour. No choices, just flavor text. Proceed to Turn 4.

**Pseudo-code displayed:**
```
function courtOfWhispers(aldric, npcs) {
    let chosen = chronicler.choose_two(npcs)
    let clues = 0

    for each npc in chosen {
        let honesty = roll(1, 6)
        if (honesty >= 4) {
            npc.speak(npc.truth)
            clues += 1
        } else {
            npc.speak(npc.lie)
        }
    }

    if (clues >= 2) {
        aldric.knows_secret = true
    } else if (clues == 1) {
        aldric.suspects = true
    }
    return next_turn
}
```

**NPC Dialogue:**

**Elara Golmere:**
- Truth: "The previous Hand asked too many questions about my children. Hair colour, of all things. How tedious."
- Lie: "The previous Hand drank himself to death. Tragic, but not uncommon in this city."
- Default greeting: "An Ashward in Crownharbour. How... quaint."

**Halric Golmere:**
- Truth: "My sister's children look nothing like the king. I'm not one for riddles, but that one solves itself."
- Lie: "I know nothing. I'm the family disappointment — they don't tell me things."
- Default greeting: "Ah, the chronicler. Writing history, or making it? Both are forms of lying."

**Aldren Thorne:**
- Truth: "Follow the gold. The previous Hand traced payments from the crown to certain... private arrangements. I have the ledger."
- Lie: "The previous Hand was old and tired. Sometimes the simplest explanation is the true one."
- Default greeting: "Information is currency. What can you afford?"

---

### Turn 3: "The Betrayal"

**Setting:** The throne room. Beren is dead (hunting accident). Maegor claims the Fractured Crown.

**NPCs present:** Maegor Storrund, Aldren Thorne, City Guard Captain

**What happens:**
- Beren has died. Maegor sits the Fractured Crown.
- If `aldric.knows_secret`: Aldric can challenge the claim
- **CHOICE (3 options):**
  - "Confront the throne" — Aldric publicly challenges Maegor's legitimacy
  - "Flee Crownharbour" — Aldric tries to escape
  - "Seek allies" — Aldric turns to Aldren Thorne for support
- **DICE ROLL:** Determines outcome
  - Confront: Roll 4-6 = Aldric is imprisoned (alive). Roll 1-3 = Aldric is executed.
  - Flee: Roll 3-6 = Escape succeeds. Roll 1-2 = Captured and executed.
  - Seek allies: Roll 5-6 = Aldren helps. Roll 1-4 = Aldren betrays, Aldric imprisoned.

**If Aldric is dead or in Greyholt:** Turn 3 shows the throne room from the chronicler's perspective. Maegor takes the throne unopposed. No choice — just witness. Brief NPC exchange between Maegor and Elara.

**Pseudo-code displayed:**
```
function theBetrayal(aldric, maegor) {
    announce("The king is dead. Long live the king.")
    maegor.sit(fracturedCrown)

    if (!aldric.alive || aldric.location != "Crownharbour") {
        return next_turn
    }

    let action = chronicler.choose("Confront", "Flee", "Seek allies")

    if (action == "Confront") {
        let fate = roll(1, 6)
        if (fate >= 4) {
            aldric.status = "imprisoned"
        } else {
            aldric.alive = false
            announce("The sword falls. The North remembers.")
        }
    }
    if (action == "Flee") {
        let fate = roll(1, 6)
        if (fate >= 3) {
            aldric.location = "Fled"
        } else {
            aldric.alive = false
        }
    }
    if (action == "Seek allies") {
        let fate = roll(1, 6)
        if (fate >= 5) {
            aldren.help(aldric)
            aldric.location = "Fled"
        } else {
            aldren.betray(aldric)
            aldric.status = "imprisoned"
        }
    }
    return next_turn
}
```

**NPC Dialogue:**

**Maegor:**
- "The throne is mine by right. Kneel, or learn what iron feels like."
- (If Aldric confronts): "You dare question ME? Guards!"
- (Unopposed): "Finally. Bring me wine. And someone to punish."

**Aldren Thorne (if sought as ally):**
- (Betrayal): "I did warn you — information is currency. And Elara pays better."
- (Helps): "The tunnels beneath the keep. Go now. You owe me a kingdom."

---

### Turn 4: "The War of Five Claimants"

**Setting:** The map view — Valdrun from above. Territories in faction colors.

**NPCs present:** None directly — this is a strategic overview. A narrator NPC ("The Chronicler") describes events.

**What happens:**
- The map shows 5 factions mobilizing: Ashward (grey), Golmere (gold), Storrund-loyalists (dark gold), Vaelthorne (red), Barrier Watch (black)
- **THREE simultaneous dice rolls** resolve conflicts:
  - Roll 1: North vs. Crown (Ashward vs. Golmere) — determines who controls the midlands
  - Roll 2: Eastern rebellion — determines if Storrund loyalists take the coast
  - Roll 3: Lysara's dragons — `hatchDragons()` roll determines how many survive (1-2: one dragon, 3-4: two dragons, 5-6: three dragons)
- **NO user choice this turn** — the user watches the map shift. This is the "chance" showcase turn.
- Territory colors animate/change based on results.

**Pseudo-code displayed:**
```
function warOfFiveClaimants(factions, map) {
    let northResult = campaign(ashward, golmere, roll(1,6))
    let eastResult = campaign(storrundLoyalists, crown, roll(1,6))
    let dragons = hatchDragons(lysara, roll(1,6))

    if (northResult.winner == "ashward") {
        map.control("Midlands", "grey")
    }
    if (eastResult.winner == "loyalists") {
        map.control("Storrund Bay", "dark gold")
    }
    lysara.dragons = dragons
    announce("Across the sea, " + dragons + " dragon(s) take flight.")

    return next_turn
}
```

**Narrator Dialogue (varies by outcome):**
- "The North marches. Steel meets steel at the River Ashrun. The dice of war are cast."
- (Ashward wins): "Grey banners fly over the Midlands. The wolves advance."
- (Ashward loses): "The Golmere line holds. Gold buys sharper swords."
- (1 dragon): "Across the Pale Sea, a single dragon screams into the sky. It will have to be enough."
- (3 dragons): "Three. The sky burns. Valdrun is not ready."

---

### Turn 5: "Beyond the Barrier"

**Setting:** Split view — the map (top) and the Barrier (a dark, icy wall) in the chat area.

**NPCs present:** Corwin Frost, faction leaders (whoever survived)

**What happens:**
- Corwin Frost sends a raven: the Pale Walkers advance.
- **DICE ROLL 1:** Does the raven arrive? Roll 1-2 = lost, factions don't know. Roll 3-6 = arrives.
- **CHOICE (if raven arrives):** "Unite the factions" or "Ignore the warning"
  - If unite: **DICE ROLL 2** = Roll 4-6 success (alliance forms), Roll 1-3 fail (factions refuse)
- **Final state** is determined by: who holds Crownharbour + raven + alliance + number of dragons

**Pseudo-code displayed:**
```
function beyondTheBarrier(factions, barrier) {
    let ravenArrives = roll(1, 6) >= 3
    let paleWalkersAdvance = true

    if (!ravenArrives) {
        announce("The raven never came.")
        return ending("fall")
    }

    let action = chronicler.choose("Unite", "Ignore")
    if (action == "Ignore") {
        return ending("fall")
    }

    let alliance = formAlliance(factions, roll(1,6))
    if (alliance && lysara.dragons >= 2) {
        return ending("victory")
    } else if (alliance) {
        return ending("pyrrhic")
    } else {
        return ending("fall")
    }
}
```

**Corwin Frost:**
- "The dead don't care who sits your throne. Send everyone. Send them now."
- (Raven lost): [silence — this NPC never appears]

**Endings (shown as chronicle entry):**

**Victory:** "The factions united beneath dragonfire. The Barrier held. Valdrun endured — scarred, but whole. The Fractured Crown sat empty for a season. No one rushed to claim it."

**Pyrrhic:** "They fought together, barely. The Barrier cracked but did not fall. Half the armies perished. The survivors argued over the ashes. History does not record who sat the Fractured Crown. It no longer mattered."

**Fall:** "The Pale Walkers crossed the Barrier. The factions were still fighting each other when the cold came. The chronicler's pen froze mid-sentence. This is where the record ends."

---

## 5. Epilogue — The Chronicle Log

After the final turn, display a generated summary:

```
--- CHRONICLE OF VALDRUN — PLAYTHROUGH #[random 4-digit number] ---

Aldric Ashward: [ALIVE / DEAD / IMPRISONED / STAYED NORTH]
Maegor Storrund: [SITS THE ASHEN THRONE / DEPOSED / IRRELEVANT]
The Secret: [DISCOVERED / SUSPECTED / UNKNOWN]
Aldren Thorne: [BETRAYED / HELPED / NEVER CONSULTED]
War Outcome: Midlands held by [FACTION]. Coast held by [FACTION].
Dragons Hatched: [1 / 2 / 3]
The Raven: [ARRIVED / LOST]
Alliance: [FORMED / REFUSED / NEVER ATTEMPTED]
Ending: [VICTORY / PYRRHIC / FALL]

"History is what the dice decide. Play again to write a different truth."
```

This log satisfies the "documentation" requirement — it's a record unique to each playthrough, echoing Douglas Huebler's Secrets (1969).

---

## 6. UI Layout and Visual Spec

### Screen Layout (single page, no scrolling during gameplay)

```
+----------------------------------------------------------+
|  [THE ASHEN THRONE]           [Turn 2/5]   [About]       |  <- Top bar
+----------------------------------------------------------+
|                                                          |
|                                                          |
|              MAIN VIEWPORT                               |
|              (Map SVG or scene illustration)              |
|              Territory colors shift per faction           |
|              Simple pixel-art style                       |
|                                                          |
|                                                          |
+----------------------------------------------------------+
|  [NPC PORTRAIT]  NPC NAME                                |
|  (64x64 pixel    "Dialogue text appears here, one        |
|   colored         line at a time, click to advance."     |
|   silhouette)                                            |
|                  [Choice A]  [Choice B]  [Choice C]      |  <- RuneScape-style options
+----------------------------------------------------------+
|  [Source Code]  [Chronicle Log]                           |  <- Toggle panels
+----------------------------------------------------------+
```

### Color Palette

```
Background:        #1a1a2e (dark navy)
Panel backgrounds: #16213e (slightly lighter navy)
Panel borders:     #0f3460 (blue-grey)
Default text:      #e0e0e0 (light grey)
NPC dialogue:      #ffff00 (RuneScape yellow)
System messages:   #00ffff (RuneScape cyan)
Choice buttons:    #e94560 (red accent) with #0f3460 border

Faction colors:
  Ashward:         #808080 (grey)
  Golmere:         #ffd700 (gold)
  Storrund:        #daa520 (dark gold)
  Vaelthorne:      #cc0000 (deep red)
  Barrier Watch:   #1a1a1a (near black)
  Pale Walkers:    #a8d8ea (ice blue)
  Neutral:         #2d4059 (muted blue-grey)
```

### Typography

```
Headings / Title:  monospace (or 'Courier New'), uppercase, letter-spacing 2px
NPC Dialogue:      monospace, 14px-16px
System text:       monospace, 12px, cyan
Pseudo-code:       monospace, 12px, green (#00ff00) on dark background
Choice buttons:    monospace, 14px, bold
```

Use Tailwind's `font-mono` throughout. The entire aesthetic is monospace — this is a terminal/game hybrid. No serif or sans-serif fonts.

### Dice Animation

When a roll happens:
1. Show a dice icon (simple SVG cube) that spins/shakes for 1 second via CSS keyframes
2. Display the result as a RuneScape-style "hitsplat" — a colored circle with the number:
   - Red circle (bad roll: 1-3)
   - Green circle (good roll: 4-6)
3. The number fades after 2 seconds, and dialogue continues based on result

### NPC Portraits

Simple 64x64 SVG silhouettes. Each is a head-and-shoulders shape filled with the faction color, with one distinguishing feature:
- **Beren:** Crown shape on head, dark gold fill
- **Aldric:** Flat top / fur collar, grey fill
- **Elara:** Long hair silhouette, gold fill
- **Halric:** Shorter silhouette (half height), gold fill
- **Aldren:** Pointed goatee shape, dark green fill (#2d5a3d)
- **Maegor:** Small crown, gold fill
- **Corwin:** Hood/cloak shape, black fill
- **Lysara:** Flowing hair, red fill
- **Narrator/Chronicler:** Scroll/quill shape, white fill

These do NOT need to be detailed. Colored blobs with one distinguishing silhouette feature is enough.

### Map of Valdrun (SVG)

A simplified fantasy map with ~7 labeled regions as colored polygons:
- **The North** (top) — Ashward grey
- **The Midlands** (center) — contested, changes color
- **Crownharbour** (center-south dot) — seat of power indicator
- **The Westerlands** (west) — Golmere gold
- **Storrund Bay** (southeast) — Storrund dark gold
- **The Barrier** (very top, horizontal line) — black/ice blue
- **The Ember Isles** (far right or bottom, separated) — Vaelthorne red

The map is NOT geographically accurate to any real-world location. It's a simple fantasy landmass. Shapes can be abstract polygons.

---

## 7. State Schema

```javascript
// Core game state — this is the single source of truth
const initialState = {
    // Turn tracking
    currentTurn: 1,        // 1-5
    gameOver: false,

    // Characters
    aldric: {
        alive: true,
        location: "Greyholt",   // "Greyholt" | "Crownharbour" | "Imprisoned" | "Fled"
        suspicion: false,       // true if omen roll >= 4 in Turn 1
        knows_secret: false,    // true if 2 clues found in Turn 2
        suspects: false,        // true if 1 clue found in Turn 2
    },
    beren: {
        alive: true,            // dies before Turn 3 (always)
    },
    maegor: {
        sitsThrone: false,      // true after Turn 3
    },
    aldren: {
        consulted: false,       // was he one of the 2 chosen in Turn 2?
        betrayed: null,         // true/false, set in Turn 3 if "Seek allies" chosen
    },
    lysara: {
        dragons: 0,             // 1-3, set in Turn 4
    },

    // Factions (Turn 4 war results)
    map: {
        north: "ashward",       // always ashward
        midlands: "contested",  // set in Turn 4: "ashward" or "golmere"
        westerlands: "golmere", // always
        storrundBay: "storrund",// set in Turn 4: "loyalists" or "crown"
        crownharbour: "golmere",// default after Turn 3, may change
        barrier: "barrier_watch",
        emberIsles: "vaelthorne",
    },

    // Turn 5
    ravenArrived: false,
    allianceFormed: false,

    // Ending
    ending: null,           // "victory" | "pyrrhic" | "fall"

    // Logs
    diceLog: [],            // Array of { turn, purpose, result }
    chronicleLog: [],       // Array of narrative strings for the epilogue

    // UI state
    dialogueQueue: [],      // Array of { speaker, text, color } to display sequentially
    currentDialogue: 0,     // Index into dialogueQueue
    waitingForChoice: false,
    choices: [],            // Current available choices: [{ label, action }]
    showSourceCode: false,  // Toggle for pseudo-code overlay
    showChronicle: false,   // Toggle for chronicle log overlay
    showAbout: false,       // Toggle for about page
};
```

---

## 8. Component Breakdown

### `<App />`
- Root component. Holds all state via `useState`.
- Renders: `<TopBar />`, `<MainViewport />`, `<DialoguePanel />`, `<BottomBar />`
- Conditionally renders: `<SourceCodeOverlay />`, `<ChronicleOverlay />`, `<AboutOverlay />`, `<DiceAnimation />`

### `<TopBar />`
- Props: `title`, `currentTurn`, `onAboutClick`
- Displays game title, turn counter, settings/about button
- Simple flex row, dark background, border-bottom

### `<MainViewport />`
- Props: `currentTurn`, `mapControl`, `diceResult`, `showDice`
- Turns 1-3: Shows a scene-appropriate colored background with location text (e.g., "GREYHOLT" in large monospace)
- Turn 4: Shows the SVG map with faction-colored regions
- Turn 5: Shows the map with the Barrier glowing

### `<DialoguePanel />`
- Props: `dialogueQueue`, `currentDialogue`, `choices`, `waitingForChoice`, `onChoiceSelect`, `onAdvance`
- The RuneScape chat box. Dark panel, bottom of screen.
- Shows NPC portrait (left), name + dialogue text (right)
- Text appears character-by-character (typewriter effect, ~30ms per char)
- Click anywhere on panel to advance to next dialogue line
- When `waitingForChoice` is true, shows choice buttons instead of "click to continue"

### `<DiceAnimation />`
- Props: `rolling`, `result`, `isGood`
- Appears center-screen when a roll happens
- CSS spin animation for 1s, then shows result in colored circle
- Green circle for good (4-6), red for bad (1-3)
- Auto-dismisses after 2s

### `<SourceCodeOverlay />`
- Props: `visible`, `currentTurn`, `onClose`
- Slide-in panel from the right
- Shows the pseudo-code for the CURRENT turn
- Green monospace text on near-black background
- Current executing line could be highlighted (optional polish)

### `<ChronicleOverlay />`
- Props: `visible`, `chronicleLog`, `diceLog`, `ending`, `gameState`, `onClose`
- Full chronicle entry (see Section 5 format)
- Only fully populated after Turn 5; during gameplay shows partial log

### `<AboutOverlay />`
- Props: `visible`, `onClose`
- Static content explaining the project (see Section 9)
- References, rubric mapping, concept statement

### `<NpcPortrait />`
- Props: `character` (string key)
- Returns the appropriate simple SVG silhouette
- 64x64px, faction-colored

---

## 9. About Page Content

```
ABOUT THIS PROJECT

"The Fractured Crown" is an interactive performance script created for
Art of the Web, Section 3, Assignment 1.

CONCEPT

A browser-based quest that uses the logic of programming
functions to drive a political drama. Each playthrough is a
unique "performance" — the dice ensure no two runs are the same.

The script is not syntactically valid code. It is a creative
interpretation of programming logic as a performance score,
in the tradition of Fluxus event scores and algorithmic
choreography.

HOW IT SATISFIES THE BRIEF

FUNCTIONS: Every NPC interaction, battle, and plot event is
governed by a named function with inputs, outputs, and
conditionals. The "Source Code" panel shows these in real time.

CHANCE: Visible dice rolls determine NPC honesty, battle
outcomes, character survival, and the final state of the world.
The same script produces different performances each time.

DURATION: A single playthrough takes 3-5 minutes. The work is
designed to be replayed.

DOCUMENTATION: This site is the documentation. The Chronicle
Log records each playthrough's unique history. The Source Code
panel reveals the logic driving the performance.

REFERENCES

- VNS Matrix, CorpusFantasticaMoo (1995) — interactive text
  world as performance art
- Taeyoon Choi, Movement Scores (2018-19) — instructions as
  performance scores
- Josh Darnit, Exact Instructions Challenge (2017) — literal
  interpretation of programmatic instructions
- Chloe Bass, Interactions and Play (2012-14) — structured
  audience interaction as art
- Douglas Huebler, Secrets (1969) — documentation of unique,
  unrepeatable events

CREATED BY

[Your Name]
[Date]
Art of the Web — Section 3, Assignment 1
```

---

## 10. Game Flow Logic (simplified)

```
START
  -> Show title screen with "Begin Chronicle" button
  -> On click: set currentTurn = 1, load Turn 1 dialogue

TURN 1 (kingSummons):
  -> Queue Beren's greeting dialogue
  -> Trigger dice roll (omen check)
  -> Queue Aldric's response (varies by roll)
  -> Present choice: "Accept" / "Refuse"
  -> If Refuse: set aldric.location = "Greyholt", log it, skip to Turn 4
  -> If Accept: set aldric.location = "Crownharbour", proceed to Turn 2

TURN 2 (courtOfWhispers):
  -> If aldric.location == "Greyholt": show merchant interlude, skip to Turn 4
  -> Present NPC selection: pick 2 of 3 (Elara, Halric, Aldren)
  -> For each chosen NPC: dice roll -> truth or lie dialogue
  -> Calculate clues -> set aldric.knows_secret or aldric.suspects
  -> Proceed to Turn 3

TURN 3 (theBetrayal):
  -> Beren dies (narrated, not shown)
  -> Maegor claims throne
  -> If aldric not in Crownharbour: show Maegor taking throne, no choice
  -> If aldric present: choice of Confront / Flee / Seek Allies
  -> Dice determines outcome for chosen action
  -> Update aldric status (alive/dead/imprisoned/fled)
  -> Proceed to Turn 4

TURN 4 (warOfFiveClaimants):
  -> Switch to map view
  -> Three simultaneous dice rolls (north war, east rebellion, dragons)
  -> Animate map territory changes
  -> Narrator describes outcomes
  -> Proceed to Turn 5

TURN 5 (beyondTheBarrier):
  -> Dice roll: does raven arrive?
  -> If no: ending = "fall"
  -> If yes: choice "Unite" or "Ignore"
  -> If Ignore: ending = "fall"
  -> If Unite: dice roll for alliance success
  -> Determine final ending based on alliance + dragons
  -> Show Chronicle Log

END
  -> Display full chronicle
  -> "Play Again" button resets all state to initialState
```

---

## 11. Technical Notes for Implementation

### Code Style
- **Comment heavily.** Every function, every state change, every dice roll should have a comment explaining what it does and why.
- **Keep it simple.** No external state management (no Redux, no Context). Just `useState` at the App level, pass props down.
- **Single index.html file.** No build step, no bundler, no npm. Everything lives in one `index.html` that loads dependencies via CDN. CSS via Tailwind utility classes. Inline styles only where Tailwind doesn't cover it (e.g., specific hex colors).

### Output Format (IMPORTANT)
The final deliverable MUST be a single `index.html` file that runs with no build step. It loads React, ReactDOM, Babel, and Tailwind via CDN. All JSX is compiled in-browser by Babel. The file structure:

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>The Fractured Crown</title>
    <script src="https://unpkg.com/react@18/umd/react.production.min.js"></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <script src="https://cdn.tailwindcss.com"></script>
</head>
<body>
    <div id="root"></div>
    <script type="text/babel">
        // ALL React components and game logic go here
        // Single script block, heavily commented
    </script>
</body>
</html>
```

This format is required because the project is hosted on GitHub Pages with no server-side processing.

### Dice Roll Implementation
```javascript
// Simple dice roll — returns integer between min and max inclusive
// This is the "chance" element required by the rubric
const roll = (min, max) => {
    return Math.floor(Math.random() * (max - min + 1)) + min;
};
```

### Typewriter Effect
```javascript
// Typewriter for dialogue — reveals text character by character
// Use a setInterval that increments a charIndex counter
// Display dialogueText.substring(0, charIndex)
// Click to skip: set charIndex to full length
```

### NPC Selection (Turn 2)
- Show 3 NPC portraits as clickable buttons
- Track selections in state. After 2 are picked, disable the third and proceed.
- Each selection triggers its dice roll and dialogue sequence

### Map Territory Animation (Turn 4)
- SVG regions have `fill` attribute bound to state
- On dice resolution, update `map.midlands` etc. in state
- CSS transition on SVG fill for smooth color change (0.5s)

### Replay
- "Play Again" button calls a function that resets state to `initialState`
- Generate a new random playthrough number for the chronicle

---

## 12. Pseudo-Code Script (the "Plan" document)

This is the full pseudo-code that appears in the Source Code overlay. It is the "written component" required by the rubric. It should be displayed verbatim in the site.

```
-- THE ASHEN THRONE — A Performance Script --

This script governs the chronicle of Valdrun.
It is not valid code. It is a score.
Each performance produces a different history.
The dice decide. The chronicler witnesses.

function beginChronicle() {
    let state = {
        aldric: { alive: true, location: "Greyholt" },
        throne: "Beren Storrund",
        secret: "unknown",
        dragons: 0,
        ending: null
    }

    state = kingSummons(state)
    state = courtOfWhispers(state)
    state = theBetrayal(state)
    state = warOfFiveClaimants(state)
    state = beyondTheBarrier(state)

    inscribe(state)
    announce("Play again. History is never settled.")
}

function kingSummons(state) {
    beren.speak("Come south, old friend.")
    let omen = roll(1, 6)
    if (omen >= 4) {
        aldric.suspicion = true
    }
    let choice = chronicler.choose("Accept", "Refuse")
    if (choice == "Refuse") {
        state.aldric.location = "Greyholt"
    } else {
        state.aldric.location = "Crownharbour"
    }
    return state
}

function courtOfWhispers(state) {
    if (state.aldric.location != "Crownharbour") {
        announce("Rumors travel north. The court festers.")
        return state
    }
    let npcs = [elara, halric, aldren]
    let chosen = chronicler.choose_two(npcs)
    let clues = 0
    for each npc in chosen {
        if (roll(1, 6) >= 4) {
            npc.speak(npc.truth)
            clues += 1
        } else {
            npc.speak(npc.lie)
        }
    }
    if (clues >= 2) { state.secret = "known" }
    if (clues == 1) { state.secret = "suspected" }
    return state
}

function theBetrayal(state) {
    beren.die()
    maegor.sit(fracturedCrown)
    state.throne = "Maegor Storrund"

    if (state.aldric.location != "Crownharbour") {
        return state
    }

    let action = chronicler.choose("Confront", "Flee", "Seek allies")
    let fate = roll(1, 6)

    if (action == "Confront") {
        if (fate >= 4) { state.aldric.status = "imprisoned" }
        else { state.aldric.alive = false }
    }
    if (action == "Flee") {
        if (fate >= 3) { state.aldric.location = "Fled" }
        else { state.aldric.alive = false }
    }
    if (action == "Seek allies") {
        if (fate >= 5) { aldren.help(aldric) }
        else { aldren.betray(aldric); state.aldric.status = "imprisoned" }
    }
    return state
}

function warOfFiveClaimants(state) {
    let north = campaign(ashward, golmere, roll(1, 6))
    let east = campaign(loyalists, crown, roll(1, 6))
    let dragons = hatchDragons(roll(1, 6))

    map.update(north.winner, east.winner)
    state.dragons = dragons
    announce(dragons + " dragon(s) take flight.")
    return state
}

function beyondTheBarrier(state) {
    let paleWalkers = true

    if (roll(1, 6) < 3) {
        announce("The raven never came.")
        state.ending = "fall"
        return state
    }

    let action = chronicler.choose("Unite", "Ignore")
    if (action == "Ignore") {
        state.ending = "fall"
        return state
    }

    let alliance = roll(1, 6) >= 4
    if (alliance && state.dragons >= 2) {
        state.ending = "victory"
    } else if (alliance) {
        state.ending = "pyrrhic"
    } else {
        state.ending = "fall"
    }
    return state
}

function inscribe(state) {
    // the chronicle is written
    // no two are the same
    // this is the document
    // this is the performance
    // this is the script
    // play again
}

-- END SCORE --
```

---

## 13. Hosting and Deployment

Each class assignment gets its own GitHub repo with GitHub Pages enabled, matching the pattern used for previous assignments (Section 1, S2A2, etc.).

### Setup

1. Create a new repo named something like `aotws3a1` (or whatever naming convention matches your other assignments)
2. Enable GitHub Pages in repo Settings > Pages > Source: deploy from branch `main`, folder `/ (root)`

### Repo Structure

```
aotws3a1/                      (new repo)
└── index.html                 (the entire game — single file)
```

### Deployment Steps

```bash
mkdir aotws3a1
cd aotws3a1
git init
cp /path/to/the-fractured-crown/index.html index.html
git add index.html
git commit -m "the ashen throne - aotw s3 a1"
git remote add origin https://github.com/[username]/aotws3a1.git
git push -u origin main
```

Then enable Pages in the repo settings. Live at: `https://[username].github.io/aotws3a1/`

No build step. No CI/CD. No npm. Push the single HTML file, enable Pages, done.

### Why This Works
GitHub Pages serves static files. Since the game is a single `index.html` that loads React/Babel/Tailwind via CDN and has zero server-side requirements, it just works. The browser does all the work.

---

## 14. File Checklist

When complete, the project should consist of:

- [ ] `index.html` in its own repo (e.g., `aotws3a1`) — the entire interactive experience (single file, no build step, CDN dependencies)
- [ ] This PRD (submitted as the "planning document")
- [ ] The pseudo-code script (embedded in the site AND submittable separately if needed)

That's it. One file in one folder in an existing repo.
