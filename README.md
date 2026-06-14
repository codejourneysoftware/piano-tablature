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
                                                                                
[Hand Pos] C Pos                               G Pos                            
[Chords]   C                                   G                                
[Melody]   5---------5---------1---------1-----5---------5---------4----------- 
[Lyrics]   Twinkle,  twinkle,  lit-  tle       star,     how   I   won-  der   

```

### Layer Breakdown

#### 1. Hand Position Row (`[Hand Pos]`)

Because the piano utilizes the entire keyboard, the standard organizes rows around a **Home Base** or anchoring zone (e.g., `C Pos`, `G Pos`).

* When a pianist needs to shift smoothly up or down the keyboard, a **Cross** (thumb under or fingers over) is digitally represented by placing a new Hand Position marker exactly at the grid coordinate where the physical hand shift occurs.

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

## 🚀 Live Demo & Reference Implementation

We are building a beautiful product ecosystem around this open-source standard, enabling features like instant interactive transpositions, fluid canvas printing options (`Manuscript` and `Studio Dark`), and local-first data processing.

* 🌍 **Interactive Sandbox Web App:** [Code Journey Labs - Piano Tabs](https://www.google.com/search?q=https://www.codejourneysoftware.com/labs/pianotabs/pianotabs.html)

---

## 🛠️ Contributing

We welcome contributions of all kinds! Whether you want to help refine the parsing rules, build visual layout engines, write translation layers for MIDI/JSON, or just submit tab files following the standard.

1. Fork this repository.
2. Create your feature branch (`git checkout -b feature/AmazingStandardImprovement`).
3. Commit your changes (`git commit -m 'Add support for sustain pedal markers'`).
4. Push to the branch (`git push origin feature/AmazingStandardImprovement`).
5. Open a Pull Request.

---

## 📄 License

This project and its specification are licensed under the MIT License - see the [LICENSE](https://www.google.com/search?q=LICENSE) file for details.
