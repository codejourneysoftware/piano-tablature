# 🎹 The Piano Tablature Standard Project

Traditional sheet music is rich and expressive, but it comes with a steep learning curve and requires specialized software. Conversely, text-based guitar tablature revolutionized how millions learn music online, yet piano players have lacked a widely accepted, clean, and elegant text-based notation system.

**This project changes that.** We are establishing a beautiful, unified, and human-readable piano tablature standard that bridges the gap between raw text flexibility and structured musical intent.

---

## 🚀 Live Preview
* See a live, interactive preview here: [Piano Tablature Sandbox](https://codejourneysoftware.github.io/piano-tablature/)

---

## 🎯 Our Mission

Currently, there is no universally established standard for text-based piano tabs. Most tabs found online are inconsistent, fragment across different screen sizes, and easily lose alignment.

With this open-source standard, we are establishing a rigorous layout system that accomplishes three core goals:

1. **Universal Readability:** Easily understood by beginners tracking basic hand positions, finger numbers, and chords.
2. **Deterministic Layouts:** Standardizing structural rules so a text tab looks identical whether rendered in a terminal, a standard text editor, or compiled into a rich web canvas layout.
3. **Structured Interoperability:** Grounding the layout on a highly predictable grid system that can easily map to an extensible JSON data model for native UI rendering and playback.

---

## 📏 The Architectural Rule: The 0-75 Grid System

To ensure total layout stability across physical screens, markdown viewers, and digital web canvases, our standard introduces a strict **fixed-width horizontal layout system**.

> 💡 **The Core Rule:** Every musical line or structural row in a standard tab block is constrained to a fixed window of **exactly 76 positions, indexed from position 0 to position 75.**

* **Predictable Boundaries:** By enforcing a hard ceiling at position 75, we prevent unexpected text-wrapping behavior on standard viewing windows, creating a clean, dependable canvas boundary.
* **Tokenized Alignment:** Every character space functions as an absolute coordinate. If a chord change or a finger strike happens at position 32, the associated lyric token, hand shift marker, or secondary note array aligns precisely at position 32 across all layers of the tab block.

---

## 🎨 Layout Anatomy of a Standard Tab Segment

A complete piece of tablature is composed of repeating structural blocks. Each block maps specific layers of performance data onto the 76-column grid layout:

```text
Position: 0--------10--------20--------30--------40--------50--------60--------75
                                                                                
[Hand Pos] C3 Pos                              G3 Pos                            
[Chords]   C                                   G                                
[Melody]   5---------5---------1---------1-----5---------5---------4----------- 
[Lyrics]   Twinkle,  twinkle,  lit-  tle       star,     how   I   won-  der   

```

### Layer Breakdown

#### 1. Hand Position Row (`[Hand Pos]`)

Because a piano spans 88 keys, specifying a generic letter note is not enough—the standard must dictate exactly *where* on the keyboard the player should anchor their hand. We do this by appending the standard **Scientific Pitch Notation (SPN)** octave number directly to the anchor note.

| Octave Number | Common Descriptive Name | Musical Character / Register |
| :--- | :--- | :--- |
| **Octaves 0 & 1** | Sub-Contra / Contra | The absolute lowest, rumbling bass notes. |
| **Octave 2** | Great Octave | Deep bass notes (ideal for left-hand bass lines). |
| **Octave 3** | Small Octave | Tenor range (just below Middle C). |
| **Octave 4** | **Middle Octave** | **The center of the piano.** Contains **Middle C (C4)**. |
| **Octave 5** | Two-Line Octave | Treble range (where standard vocal melodies sit). |
| **Octave 6** | Three-Line Octave | High treble range (bright, piercing melodies). |
| **Octaves 7 & 8** | High / Top Octave | The highest, tinny "crystal" notes on the far right. |

* **Octave Anchoring Rules:**
  * `C4 Pos` anchors the player's home base around Middle C.
  * `G4 Pos` anchors the player around the G just above Middle C.
  * Left-hand configurations can cleanly specify lower bass anchors like `C3 Pos` or `G2 Pos`.
* **The "Default to 4" Rule:** To keep beginner tabs clean, a position written without a trailing integer (e.g., `C Pos`) implicitly defaults to Octave 4. Alternate octaves *must* explicitly append their number.
* **Hand Shifts:** When a pianist needs to shift smoothly up or down the keyboard, a **Cross** (thumb under or fingers over) is digitally represented by placing a new Hand Position marker (e.g., shifting from `C4 Pos` to `G4 Pos`) exactly at the grid coordinate where the physical hand shift occurs.

#### 2. Chord Tone Row (`[Chords]`)

Displays harmonic context (e.g., `C`, `G`, `Am`, `F`) mapped precisely above the notes and lyrics to give the player an instant visual cue of the song's underlying progression.

#### 3. The Musical Finger & Stretch Row (`[Melody]`)

Instead of reading traditional absolute pitches on a treble staff, the standard maps relative finger mechanics natively to the active home base:

* **The 5-Finger Home Base:** Enforces standard fingering coordinates (`5` = Root/Lowest, `4` = +2 semitones, `3` = +4 semitones, `2` = +5 semitones, `1` = +7 semitones).
* **Chromatic Accidentals:** Explicitly handles sharps and flats by appending `#` or `b` inline within the grid sequence.
* **The Physical Stretch:** Minor extensions outside the immediate home base are elegantly capped at a maximum of $\pm2$ semitones, designated by appending `+`/`++` (stretch up) or `-`/`--` (stretch down). If a passage moves further, the standard dictates a formal Hand Position change in the row above.

#### 4. Highly Extensible Lyric Mapping (`[Lyrics]`)

Lyrics are bound directly to the matching visual coordinates of the musical grid. To accommodate rich arrangements, our open-source standard natively supports a multi-dimensional array structure for lyrics—allowing **multiple verses** to cleanly stack and share the exact same chord map and finger tracking without duplicating the entire musical block.

---

## 🖐️ Hand Mechanics & Fingering Standard

To keep text-based tabs intuitive, the standard adopts the universal piano fingering system for both hands. Below is the primary mapping standard for the **Right Hand (RH)** home base:

| Finger Number | Anatomical Finger | Home Base Default Mapping (e.g., `C4 Pos`) |
| :---: | :--- | :--- |
| **1** | Thumb | **Root Position Anchor** (e.g., The `C4` key) |
| **2** | Index Finger | +2 Semitones / Whole Step up (e.g., The `D4` key) |
| **3** | Middle Finger | +4 Semitones / Major Third up (e.g., The `E4` key) |
| **4** | Ring Finger | +5 Semitones / Perfect Fourth up (e.g., The `F4` key) |
| **5** | Pinky | +7 Semitones / Perfect Fifth up (e.g., The `G4` key) |

### 🧭 Interpretation Rules

* **The 5-Finger Shell:** When a position marker like `C4 Pos` appears, the player instinctively rests their right hand over these 5 consecutive white keys using fingers 1 through 5.
* **Relative vs. Absolute:** By prioritizing relative finger numbers (`1`-`5`) over absolute pitch letters (`C`-`D`-`E`), beginners can read melodies instantly without needing to think about notes on a page, and developers can program trivial transposition engines across the grid layout.

### Sample JSON Song Layout

```javascript
{
  "metadata": {
    "title": "The Sample Standard Arrangement",
    "composed_by": "Code Journey Software",
    "key_instructions": "Play with a steady, explicit pop ballad rhythm in C Major. Note the 0-75 absolute indexing boundaries.",
    "grid_columns_per_line": 76
  },
  "sections": [
    {
      "type": "Intro",
      "lines": [
        {
          "type": "grid_line",
          "hand_positions": [
            { "position": "C4 Pos", "col": 0 }
          ],
          "chords": [
            { "chord": "C", "col": 0 },
            { "chord": "G", "col": 32 }
          ],
          "fingering": [
            { "finger": "1", "col": 0 },
            { "finger": "3", "col": 8 },
            { "finger": "5", "col": 16 },
            { "finger": "2", "col": 32 },
            { "finger": "4", "col": 40 },
            { "finger": "5", "col": 48 }
          ],
          "lyrics": [
            [
              { "text": "[Piano", "col": 0 },
              { "text": "Intro", "col": 8 },
              { "text": "Arpeggio]", "col": 16 }
            ]
          ]
        }
      ]
    },
    {
      "type": "Verse 1",
      "lines": [
        {
          "type": "grid_line",
          "chords": [
            { "chord": "C", "col": 0 },
            { "chord": "Am", "col": 32 },
            { "chord": "F", "col": 56 }
          ],
          "fingering": [
            { "finger": "1", "col": 0 },
            { "finger": "2", "col": 8 },
            { "finger": "3", "col": 16 },
            { "finger": "1", "col": 32 },
            { "finger": "3", "col": 40 },
            { "finger": "4", "col": 56 },
            { "finger": "5", "col": 64 }
          ],
          "lyrics": [
            [
              { "text": "This", "col": 0 },
              { "text": "is", "col": 8 },
              { "text": "a", "col": 16 },
              { "text": "Verse,", "col": 32 },
              { "text": "first", "col": 40 },
              { "text": "verse", "col": 56 },
              { "text": "here.", "col": 64 }
            ]
          ]
        }
      ]
    },
    {
      "type": "Chorus",
      "lines": [
        {
          "type": "grid_line",
          "hand_positions": [
            { "position": "G4 Pos", "col": 0 }
          ],
          "chords": [
            { "chord": "G", "col": 0 },
            { "chord": "C", "col": 24 },
            { "chord": "G", "col": 48 },
            { "chord": "F", "col": 64 }
          ],
          "fingering": [
            { "finger": "1", "col": 0 },
            { "finger": "1", "col": 8 },
            { "finger": "1", "col": 16 },
            { "finger": "4", "col": 24 },
            { "finger": "4", "col": 32 },
            { "finger": "4", "col": 40 },
            { "finger": "1", "col": 48 },
            { "finger": "1", "col": 56 },
            { "finger": "2", "col": 64 },
            { "finger": "1", "col": 70 }
          ],
          "lyrics": [
            [
              { "text": "Cho-", "col": 0 },
              { "text": "rus,", "col": 8 },
              { "text": "Cho-", "col": 16 },
              { "text": "rus,", "col": 24 },
              { "text": "Cho-", "col": 32 },
              { "text": "rus,", "col": 40 },
              { "text": "this", "col": 48 },
              { "text": "is", "col": 56 },
              { "text": "Cho-", "col": 64 },
              { "text": "rus.", "col": 70 }
            ]
          ]
        }
      ]
    },
    {
      "type": "Bridge",
      "lines": [
        {
          "type": "grid_line",
          "hand_positions": [
            { "position": "C4 Pos", "col": 0 }
          ],
          "chords": [
            { "chord": "Am", "col": 0 },
            { "chord": "Em", "col": 24 },
            { "chord": "F", "col": 48 },
            { "chord": "G", "col": 64 }
          ],
          "fingering": [
            { "finger": "1", "col": 0 },
            { "finger": "2", "col": 8 },
            { "finger": "3", "col": 16 },
            { "finger": "1", "col": 24 },
            { "finger": "3", "col": 32 },
            { "finger": "4", "col": 48 },
            { "finger": "5", "col": 56 },
            { "finger": "5+", "col": 64 },
            { "finger": "5", "col": 72 }
          ],
          "lyrics": [
            [
              { "text": "Bridge,", "col": 0 },
              { "text": "this", "col": 8 },
              { "text": "is", "col": 16 },
              { "text": "a", "col": 24 },
              { "text": "bridge,", "col": 32 },
              { "text": "bridge,", "col": 48 },
              { "text": "bridge,", "col": 56 },
              { "text": "high", "col": 64 },
              { "text": "note.", "col": 72 }
            ]
          ]
        }
      ]
    },
    {
      "type": "Tag",
      "lines": [
        {
          "type": "grid_line",
          "chords": [
            { "chord": "F", "col": 0 },
            { "chord": "G", "col": 32 }
          ],
          "fingering": [
            { "finger": "4", "col": 0 },
            { "finger": "4", "col": 12 },
            { "finger": "5", "col": 32 },
            { "finger": "5", "col": 44 }
          ],
          "lyrics": [
            [
              { "text": "Tag", "col": 0 },
              { "text": "it,", "col": 12 },
              { "text": "tag", "col": 32 },
              { "text": "it.", "col": 44 }
            ]
          ]
        }
      ]
    },
    {
      "type": "Outro",
      "lines": [
        {
          "type": "grid_line",
          "chords": [
            { "chord": "C", "col": 0 }
          ],
          "fingering": [
            { "finger": "5", "col": 0 },
            { "finger": "3", "col": 12 },
            { "finger": "1", "col": 24 }
          ],
          "lyrics": [
            [
              { "text": "Out-", "col": 0 },
              { "text": "ro", "col": 12 },
              { "text": "ends.", "col": 24 }
            ]
          ]
        }
      ]
    }
  ]
}
```

---

## 🚀 Live Demo & Reference Implementation

We are building a beautiful product ecosystem around this open-source standard, enabling features like instant interactive transpositions, fluid canvas printing options (`Manuscript` and `Studio Dark`), and local-first data processing.

* See a live, interactive preview here: [Piano Tablature Sandbox](https://codejourneysoftware.github.io/piano-tablature/)
* 
---

## 🛠️ Contributing

We welcome contributions of all kinds! Whether you want to help refine the parsing rules, build visual layout engines, write translation layers for MIDI/JSON, or just submit tab files following the standard.

1. Fork this repository.
2. Create your feature branch (`git checkout -b feature/AmazingStandardImprovement`).
3. Commit your changes (`git commit -m 'Add support for sustain pedal markers'`).
4. Push to the branch (`git push origin feature/AmazingStandardImprovement`).
5. Open a Pull Request.

---

---

## 🚧 RFC: Outstanding Standards & Technical Proposals

As an evolving open-source specification, we are actively looking for input on how to formally codify advanced physiological mechanics. We want to finalize an elegant, non-breaking text notation for **hand expansions/contractions** and **finger crossings**. 

If you have expertise in music theory or pedagogical design, please jump into the issues to help define the syntax for these core movements:

### 1. Lateral Hand Expansions & Contractions (Stretches)

While the default standard maps a static 5-finger shell (a perfect fifth span), real-world pieces frequently require players to expand their web space or contract their hand frame to reach alternate intervals.

| Movement Type | Classical Musical Context | Proposed Syntactical Notation |
| :--- | :--- | :--- |
| **Extension (Upward Stretch)** | Stretching the pinky or index to play an interval larger than a major third or fifth without shifting the wrist anchor. | Append `+` or `++` to the finger token (e.g., `5+` to extend the pinky out a whole step up). |
| **Contraction (Compression)** | Narrowing the hand frame to play minor intervals or chromatic passing notes inside a tight layout window. | Append `-` or `--` to the finger token (e.g., `2-` to play a minor second inflected scale degree). |

*   **The Design Dilemma:** How far can a finger extend before it forces a structural shift of the entire home base (position)? We are currently debating capping static extensions at a **Major 6th** or **Minor 7th** span before requiring a formal change in the `[Hand Pos]` row.

### 2. Finger Crossings & Substitutions (Passages & Arpeggios)

To execute fluid scalar runs (like a basic C Major scale), pianists rely on intricate finger crossings to reset their hand frame. However, typing descriptive text like `(thumb under)` inside the grid stretches the row out, breaking the absolute visual alignment with the chords and lyrics beneath it.

Every space in the standard must represent a strict visual time coordinate. We are currently evaluating two compact, non-breaking layout proposals to handle crossings:

#### Possible Proposal: The Inline Pivot Token (Single Character)
This approach embeds a single-character geometric modifier immediately following the finger number, keeping the strict column width intact.

* `3<` : **Thumb-Under Cross** (The thumb ducks under finger 3).
* `1>` : **Finger-Over Cross** (A finger crosses back over the thumb).

```text
Position: 0--------10--------20--------30--------40--------50--------60--------75

[Hand Pos] C4 Pos                     F4 Pos
[Melody]   1---------2---------3<--------1---------2---------3---------4---------
[Lyrics]   Joy       to        the       world,    the       Lord      is
```


---

## 📄 License

This project and its specification are licensed under the MIT License - see the [LICENSE](https://www.google.com/search?q=LICENSE) file for details.

## 🎁 A Gift From Code Journey Software
This open-source standard and its reference sandbox implementation were lovingly crafted and contributed to the global developer and music community as a free gift by Code Journey Software (http://www.codejourneysoftware.com).
We love building beautiful, intuitive products that simplify workflows and add joy to creative journeys. If this project helps you learn, teach, or build something great, consider sharing it with a fellow musician!
