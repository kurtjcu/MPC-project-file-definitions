# A1 — MPC Feature Reference

> **XPJ Format** | [Index](README.md)

This appendix documents what MPC features exist and how they behave, complementing the JSON field documentation in sections 01-14. Extracted from the Akai MPC 3.7 User Guide.

---

## 1. Track Types Overview

The MPC uses a track-based architecture where each sequence contains up to 128 tracks. There are **six user-facing track types** and **four infrastructure types**. Each track type determines how the track produces or routes sound.

### 1.1 User Track Types

| Type | Program Type | Description |
|------|-------------|-------------|
| **Drum** | 0 | Sample-based percussion/instrument with 128 pads, 8 velocity layers per pad. The most common MPC track type. |
| **Keygroup** | 1 | Chromatic sampler with zone-based key mapping. Maps samples across the keyboard with configurable key ranges, velocity zones, and pitch tracking. |
| **Plugin** | 2 | Hosts an internal software instrument (Hype, TubeSynth, Bassline, Electric, DrumSynth, Mellotron, Solina, Odyssey, OPx-4). |
| **MIDI** | 3 | Sends MIDI data to external hardware or other tracks. No internal sound engine -- produces no audio internally. |
| **CV** | 4 | Sends control voltage and gate signals to modular/analog synthesizers via the MPC's CV outputs. Two modes: Melodic and Drum. |
| **Audio** | 5 | Records and plays back audio from external inputs. Supports warp editing, time-stretching, bouncing, and flattening. |

### 1.2 Infrastructure Types

| Type | Program Type | Description |
|------|-------------|-------------|
| **Return** | 7 | Receives audio from send buses (4 returns available). Used for shared effects processing. |
| **Submix** | 8 | Intermediate mixing stage between tracks and outputs (8 submixes available). |
| **Output Pair** | 9 | Physical audio output routing (Out 1,2 through Out 31,32 depending on hardware model). |
| **Input** | 10 | Audio input monitoring channel. |

### 1.3 Common Track Properties

All user tracks share these properties:

| Property | Range/Values | Notes |
|----------|-------------|-------|
| Level | -Inf to +6 dB | Channel volume |
| Pan | -64 to +64 | Stereo position (0 = center) |
| Mute | On/Off | Channel mute state |
| Solo | On/Off | Solo or Cue mode (configurable) |
| Velocity Scaling | 50% to 200% | Scales incoming MIDI velocity |
| Record Arm | On/Off | Arms track for recording |
| Length | Bars:Beats:Ticks | Independent or follows sequence length |
| Insert Effects | 4 slots | Per-track insert effect chain |
| Sends | 4 levels | Send levels to Returns 1-4 |
| Audio Out | Out 1,2 / Out 3,4 / Sub 1-8 / mono outs | Output routing destination |

### 1.4 MIDI Monitoring Modes

| Mode | Behavior |
|------|----------|
| **Off** | No MIDI input monitoring |
| **In** | Always passes MIDI input to the sound engine (no playback of recorded events) |
| **Auto** | Passes MIDI input when track is record-armed; plays back recorded events otherwise |
| **Merge** | Always passes MIDI input AND plays back recorded events simultaneously |

Audio tracks have a simplified set: Off, In (always monitor), Auto (monitor when record-armed).

**Cross-ref:** [02-track-types.md](02-track-types.md)

---

## 2. Drum Program Features

A Drum program maps samples to pads for percussive or melodic one-shot playback. It is the foundational MPC workflow.

### 2.1 Pad and Layer Architecture

- **128 pads** per program (organized as banks A-H, pads 1-16)
- **8 layers per pad** for velocity switching and sample stacking
- Pads can be triggered via the physical pad grid, MIDI notes, or sequenced events

### 2.2 Velocity Switching

Each pad's 8 layers can be configured for velocity-based sample selection:

| Property | Range/Values | Description |
|----------|-------------|-------------|
| Velocity Start | 0-127 | Minimum velocity to trigger this layer |
| Velocity End | 0-127 | Maximum velocity to trigger this layer |

Layers are evaluated in order; the first layer whose velocity range contains the incoming velocity is triggered (or multiple if ranges overlap and Simultaneous Play is enabled).

### 2.3 Pad Trigger Modes and Behavior

| Property | Values | Description |
|----------|--------|-------------|
| Pad Play | Poly / Mono | Whether multiple instances of the same pad can overlap |
| Mute Group | 0-35 (0 = none) | Pads in the same mute group cut each other off (e.g., open/closed hi-hat) |
| Simultaneous Play | Off / On | When on, all layers matching the velocity window play simultaneously |

### 2.4 Per-Layer Sample Properties

| Property | Range/Values | Description |
|----------|-------------|-------------|
| Sample | Any loaded sample | The audio file assigned to this layer |
| Slice | Slice index | If sample has chop slices, which slice to use |
| Sample Start | 0 to sample length | Playback start point (in samples) |
| Sample End | 0 to sample length | Playback end point |
| Loop Position | 0 to sample length | Loop start point |
| Loop | Off / On / Forward / Reverse / Alternating | Loop mode |
| Pitch | -36.00 to +36.00 semitones | Pitch offset from root |
| Direction | Forward / Reverse | Playback direction |
| Tune (fine) | -100 to +100 cents | Fine pitch adjustment |

### 2.5 Per-Pad Mixer Properties

| Property | Range/Values | Description |
|----------|-------------|-------------|
| Level | -Inf to 0 dB | Pad volume |
| Pan | L64 to R64 | Pad stereo position |
| Insert Effects | 4 slots | Per-pad effect chain |
| Sends 1-4 | 0-100% | Per-pad send levels to Returns |
| Audio Out | Track / direct to output | Override track routing |
| MIDI Out Enabled | Never / Always / When Empty | When to send MIDI note for this pad |
| MIDI Out Note | 0-127 | Specific MIDI note to transmit |

### 2.6 Drum Track-Specific Features

- **14 Drum FX types** (envelope, pitch bend, randomizer, etc.) available per pad through the articulation system
- **Pad color assignment** for visual identification
- **Pad copy/paste/swap** operations
- **Sample chop** (manual, auto, BPM-based) with slices assignable to pads

**Cross-ref:** [03-drum-program.md](03-drum-program.md)

---

## 3. Keygroup Features

A Keygroup program maps samples across the keyboard chromatically, enabling melodic sample-based instruments.

### 3.1 Zone-Based Key Mapping

- Keygroups define zones with configurable **key range** (low note to high note)
- Each zone has its own sample assignment, pitch tracking, and filter settings
- Zones can overlap for velocity crossfading or layered sounds
- Up to 128 keygroups (one per pad), each with 8 layers

### 3.2 Key Properties per Zone

| Property | Range/Values | Description |
|----------|-------------|-------------|
| Root Note | 0-127 | The note at which the sample plays at original pitch |
| Key Low / Key High | 0-127 | Range of MIDI notes this zone responds to |
| Velocity Low / High | 0-127 | Velocity range for zone triggering |
| Pitch Tracking | On / Off | Whether pitch shifts with played note |
| Filter | Type, Cutoff, Resonance | Per-zone filter with keytracking |

### 3.3 Legacy vs Advanced Modes

| Mode | Description |
|------|-------------|
| **Legacy** | Simpler interface with per-zone insert effects (4 slots per zone). Traditional MPC keygroup workflow. |
| **Advanced** | Extended modulation and filter routing. No per-zone inserts in this mode. |

### 3.4 Shared Infrastructure with Drum

Keygroup programs share the same underlying 128-pad / 8-layer data structure as Drum programs. The difference is behavioral: Keygroup pads respond to MIDI notes based on zone key ranges rather than fixed pad-to-note mapping.

**Cross-ref:** [04-keygroup-program.md](04-keygroup-program.md)

---

## 4. Plugin, MIDI, CV, and Audio Track Features

### 4.1 Plugin Tracks

Plugin tracks host one of the MPC's built-in software instruments:

| Instrument | Type | Description |
|------------|------|-------------|
| Hype | Hybrid synth | Modern dual-oscillator synthesizer with macro controls, built-in effects chain |
| TubeSynth | Analog polysynth | Vintage analog emulation based on AIR Vacuum Pro, 2 oscillators + sub |
| Bassline | Monosynth | 303-style acid bass with single oscillator, resonant filter, glide |
| Electric | Electric piano | Pickup-modeled EP with bell harmonics, noise, tremolo, tube drive |
| DrumSynth | Drum machine | Synthesis-based drums (Kick, Snare, HiHat, Clap, Tom, Perc, Crash, Ride). Multi mode combines all 8 types. |
| Mellotron | Tape replay | Classic Mellotron recreation with tape aging, mechanical noise simulation |
| Solina | String ensemble | Classic Solina string machine with 6 voice types (Contra Bass through Horn) |
| Odyssey | Analog synth | Circuit-modeled ARP Odyssey with 2 VCOs, S&H, multimode filter |
| OPx-4 | FM synth | 4-operator FM synthesizer with modulation matrix (separate purchase) |
| Fabric Select | Multi-layer sampler | Curated preset instrument with 2 sample layers + percussion (Pro Pack purchase) |

All plugin instruments include built-in effects (chorus, delay, reverb, compressor, etc.) that are separate from the track insert effects.

### 4.2 MIDI Tracks

- Send MIDI note, CC, program change, and pitch bend data to external hardware or other tracks
- No internal audio engine -- no insert effects, sends, or audio routing
- Configurable MIDI output port and channel
- **Send To** field can route MIDI to another internal track (e.g., MIDI track controlling a Plugin track)
- Velocity, aftertouch, and CC data all recordable

### 4.3 CV Tracks

CV tracks control analog/modular synthesizers via the MPC's CV/Gate outputs.

| Mode | Description |
|------|-------------|
| **Melodic** | Pitch CV + Gate output. Tracks note pitch chromatically. V/Oct standard. One note at a time (monophonic). |
| **Drum** | Each pad sends a gate trigger on a dedicated CV output. Used for triggering analog drum modules. |

Key CV properties:

| Property | Range/Values | Description |
|----------|-------------|-------------|
| CV Output | Output 1-4 (hardware-dependent) | Physical CV output jack |
| Gate Output | Output 1-4 | Physical gate output jack |
| Voltage Standard | V/Oct | 1 volt per octave (standard for most modular gear) |
| Glide | On/Off, Time | Portamento between notes |

CV tracks produce no internal audio -- they only generate control voltage signals.

### 4.4 Audio Tracks

Audio tracks record and play back audio from the MPC's physical inputs.

| Feature | Description |
|---------|-------------|
| **Recording** | Captures audio from configurable input source (Input 1,2 stereo pair or mono) |
| **Monitoring** | Off, In (always), Auto (when record-armed) |
| **Warp Editing** | Time-stretch audio clips to match project tempo using Elastique algorithm |
| **Bounce** | Render a track's audio (with effects) to a new audio clip |
| **Flatten** | Destructively apply warp/stretch settings, replacing the original audio |
| **Audio Events** | Type-4 events in the sequence data, referencing sample pool entries with offset/length |

Audio tracks support full mixer features: 4 insert effects, 4 sends, pan, level, mute, solo, automation.

**Cross-ref:** [05-plugin-program.md](05-plugin-program.md), [06-audio-program.md](06-audio-program.md), [02-track-types.md](02-track-types.md)

---

## 5. Signal Flow & Routing

### 5.1 Signal Path

```
Pad/Keygroup (4 per-pad insert slots)
    |
    v
Track (4 per-track insert slots)
    |
    +---> Send 1-4 (adjustable level) ---> Return 1-4 (4 inserts each) ---> Output pair
    |
    v
Destination routing (one of):
    - Main Output (Out 1,2 default)
    - Submix 1-8 (4 inserts each) ---> Output pair
    - Output pair directly (Out 1,2 or Out 3,4)
    - Mono output (Out 1, Out 2, Out 3, Out 4)
    |
    v
Output Pair (4 insert slots)
    |
    v
Master Output (4 insert slots)
```

### 5.2 Channel Mixer

The Channel Mixer provides mixing controls across all channel types, displayed 8 channels at a time. Swipe left to access infrastructure channels.

**Channel order (left to right):** User Tracks --> Submix 1-8 --> Return 1-4 --> Main Outputs

**Mixer tabs:** Volume | Pan & Volume | Sends | Effects | I/O

### 5.3 Pad Mixer

Per-pad mixing controls, available for **Drum and Keygroup tracks only**. Provides per-pad level, pan, mute, solo, sends, insert effects, and I/O routing. Up to 8 pads displayed at once.

Filter options: by events (show only pads with events), by samples (show only pads with assigned samples).

### 5.4 Send/Return System

- **4 send buses** route audio from tracks and submixes to Returns 1-4
- Send levels are adjustable at two levels: per-pad/keygroup AND per-track/submix
- Each Return has 4 insert effect slots (the primary purpose of returns is shared effects)
- Returns route their output to an output pair

### 5.5 Submix Buses

- **8 submix buses** (Sub 1-8) act as intermediate mixing stages
- Tracks and pads can route their audio output to any submix
- Each submix has 4 insert effect slots and can send to Returns 1-4
- Submixes route to a configurable output pair

### 5.6 Insert Effect Slot Summary

Every mixable channel has exactly **4 insert effect slots**:

| Channel Type | Insert Count | Max Total for a Drum Track |
|-------------|-------------|---------------------------|
| Pad / Keygroup | 4 per pad | 128 pads x 4 = 512 |
| Track | 4 | 4 |
| Submix | 4 | -- |
| Return | 4 | -- |
| Output Pair | 4 | -- |
| Master | 4 | -- |

Maximum theoretical inserts on a single Drum track: 4 (track) + 128 x 4 (pads) = **516 insert effects**.

**Cross-ref:** [07-mixer-routing.md](07-mixer-routing.md)

---

## 6. Effects

### 6.1 Overview

The MPC includes approximately **103 confirmed insert effects** organized into 6 categories plus special types. Effects are loaded into the 4 insert slots available on every mixable channel.

### 6.2 Effect Categories

#### Delay/Reverb (24 effects)

Spatial and time-based effects for ambience and echo.

| Sub-category | Effects | Description |
|-------------|---------|-------------|
| AIR Delays | AIR Delay, AIR Diff Delay, AIR Multitap Delay | Modern delays with filtering, diffusion, multi-tap |
| AIR Reverbs | AIR Reverb, AIR Non-Lin Reverb, AIR Spring Reverb | Full-featured reverbs with room modeling, spring emulation |
| Legacy Delays | Delay Analog, Delay Analog Sync, Delay HP, Delay LP, Delay Mono, Delay Mono Sync, Delay Multi-Tap, Delay Ping Pong, Delay Stereo, Delay Sync (Stereo), Delay Tape Sync | Classic MPC delays with various filtering and sync options |
| Legacy Reverbs | Reverb Small, Reverb Medium, Reverb Large, Reverb Large 2, Reverb In Gate, Reverb Out Gate | Room simulations from small to large hall, with gated variants |
| Utility | Sample Delay | Small timing offsets (0-11025 samples or 0-250ms) for phase alignment or stereo width |

#### Dynamics (14 effects)

Compressors, limiters, gates, and transient shapers.

| Sub-category | Effects | Description |
|-------------|---------|-------------|
| AIR Dynamics | AIR Channel Strip, AIR Compressor, AIR Limiter, AIR Maximizer, AIR Noise Gate, AIR Pumper, AIR Transient | Modern dynamics processing including channel strip (EQ + gate + compressor combo), lookahead limiting, rhythmic pumping |
| Legacy Compressors | Bus Compressor, Compressor Opto, Compressor VCA, Compressor Vintage | Various compressor models: bus/glue, optical, VCA, vintage tube |
| Transient | Transient Shaper | Attack and release phase shaping |
| Sidechain | Mother Ducker Input, Mother Ducker | Two-component sidechain system (see Section 6.4) |

#### EQ/Filter (16 effects)

Equalization and frequency filtering.

| Sub-category | Effects | Description |
|-------------|---------|-------------|
| AIR EQ/Filter | AIR Enhancer, AIR Filter Gate, AIR Filter, AIR Kill EQ, AIR Para EQ, AIR Vintage Filter | Parametric EQ (4-band), multi-mode filter (17 filter types), rhythmic gate, kill EQ, harmonic enhancer |
| Legacy Filters | HP Filter, HP Filter Sweep, HP Filter Sync, HP Shelving Filter, LP Filter, LP Filter Sweep, LP Filter Sync, LP Shelving Filter | High-pass and low-pass filters with sweep, sync, and shelving variants |
| Legacy EQ | PEQ 2-Band 2-Shelf, PEQ 4-Band | Parametric equalizers |

#### Harmonic/Distortion (19 effects)

Saturation, distortion, bit reduction, and spectral effects.

| Sub-category | Effects | Description |
|-------------|---------|-------------|
| AIR Harmonic | AIR Amp Sim, AIR Flavor, AIR Freq Shift, AIR Lo-Fi, AIR Talk Box | Amp simulation, sonic character/flavor, frequency shifting, lo-fi degradation, talk box |
| Legacy Distortion | Bit Crusher, Decimator, Distortion Custom, Distortion Fuzz, Distortion Grimey, Distortion Overdrive | Bit depth reduction, sample rate decimation, various distortion flavors |
| Legacy Harmonic | Frequency Shifter, Granulator, Resampler | Spectral manipulation, granular processing, resampling |
| Special | XYFX | XY pad-controlled effect with real-time parameter control and latch mode |

#### Modulation (22 effects)

Chorus, flanger, phaser, tremolo, pitch shifting, and rhythmic modulation.

| Sub-category | Effects | Description |
|-------------|---------|-------------|
| AIR Modulation | AIR Chorus, AIR Fuzz Wah, AIR Half Speed, AIR Pitch Shifter, AIR Stutter | Modern chorus, wah with fuzz, half-speed playback, pitch shifting, rhythmic stutter |
| Legacy Chorus | Chorus 2-Voice, Chorus 4-Voice | Classic multi-voice chorus |
| Legacy Modulation | Auto Wah, Flanger, Flanger Sync, Phaser 1, Phaser 2, Phaser Sync, Tremolo, Tremolo Sync | Envelope-following wah, flanging, phasing, and tremolo with free/sync variants |

#### Vocal (3 effects)

Pitch correction and vocal processing.

| Effect | Description |
|--------|-------------|
| AIR Vocal Doubler | 1-8 voice doubling with stereo spread, pitch/timing variation |
| AIR Vocal Harmonizer | 4-part harmony generator, scale-aware (14 scales), key-selectable |
| AIR Vocal Tuner | Pitch correction with variable detection sensitivity and retune time (1-1000ms) |

#### Pro Pack (3 effects, separate purchase)

| Effect | Description |
|--------|-------------|
| AIR Visual EQ4 | 4-band EQ with spectrum analyzer visualization |
| AIR Reverb Pro | Advanced reverb with early reflections, modulated tails, built-in EQ and compressor (limited to 4 instances on standalone) |
| AIR Utility | Gain, mono, stereo width, mid-side processing, DC offset, phase invert |

#### Special Types

| Effect | Description |
|--------|-------------|
| XYFX | XY pad effect loaded as a track insert; requires one free slot. Latch mode available. |
| TouchFX | Touch strip-controlled effect (designed for MPC Key 61, usable on all models). Wet/Dry, Attack, Release envelope. |

### 6.3 Effect Management

- **Load** via plugin browser (sorted by Type or Manufacturer)
- **Enable/Disable** individual slots or all 4 at once (All On / All Off)
- **Reorder** effects within the chain using arrow buttons
- **FX Rack presets** save and load chains of 1-4 effects as a single preset
- **Per-effect presets** save and load individual effect settings

### 6.4 Mother Ducker Sidechain System

The Mother Ducker is a two-component sidechain compressor using **8 dedicated internal trigger buses** (separate from the 8 submix buses).

**How it works:**
1. Place **Mother Ducker Input** as an insert on the trigger source (e.g., kick drum pad)
2. Set the **To** parameter to Bus 1-8
3. Place **Mother Ducker (Output)** as an insert on the destination channel (e.g., bass submix)
4. Set the **From** parameter to the same bus number

**Mother Ducker Output parameters:**

| Parameter | Range | Default |
|-----------|-------|---------|
| Ratio | 1.00:1 - 60.00:1 | 6.00:1 |
| Knee | 0.000 - 6.000 dB | 0.000 dB |
| Attack | 1.0 - 1000.0 ms | 10.0 ms |
| Release | 1.0 - 1000.0 ms | 100.0 ms |
| Threshold | -100.000 - 0.000 dB | -6.021 dB |
| Gain | -100.000 - +12.000 dB | 0.000 dB |
| Auto Gain | On / Off | On |

8 independent buses allow 8 simultaneous sidechain configurations.

**Cross-ref:** [08-effects-catalog.md](08-effects-catalog.md)

---

## 7. Sequences & Timing

### 7.1 Sequence Structure

Each MPC project supports up to **128 sequences** (mapped to Pads A01-H16 in Next Sequence Mode). Sequences are independent patterns -- programs, tracks, and mixer settings are **global** (shared across all sequences); only **event data** varies per sequence.

### 7.2 Sequence Properties

| Property | Range/Values | Notes |
|----------|-------------|-------|
| Name | User-editable | Tap and hold to open Sequence Settings |
| BPM | Float (beats per minute) | Per-sequence or global tempo |
| Time Signature | Numerator 1-16 / Denominator 4, 8, 16, 32 | **Global** -- applies to ALL sequences |
| Bars | Variable | Adjustable; also Half Length / Double Length |
| Loop | On / Off | Can set independent loop start and end points |
| Dynamic | Off, 1-3 | Event velocity sensitivity |

### 7.3 Timing Resolution: 960 PPQ

All MPC timing uses **960 pulses per quarter note** (PPQ). This provides extremely fine timing resolution.

**Tick conversion table:**

| Note Value | Ticks | Triplet | Dotted |
|-----------|-------|---------|--------|
| Whole (1/1) | 3840 | 2560 | 5760 |
| Half (1/2) | 1920 | 1280 | 2880 |
| Quarter (1/4) | 960 | 640 | 1440 |
| 8th (1/8) | 480 | 320 | 720 |
| 16th (1/16) | 240 | 160 | 360 |
| 32nd (1/32) | 120 | 80 | 180 |

**Derived constants:**
- Ticks per bar (4/4 time): 3840
- Ticks per beat: 960
- Minimum resolution: 1 tick

### 7.4 Timing Correction (Quantize)

| Property | Range/Values | Description |
|----------|-------------|-------------|
| TC Value | 1/4 through 1/32T | Grid size for quantization |
| TC Amount | 0-100% | Strength of quantization (0% = no correction, 100% = snap to grid) |
| Swing | 50-75% | Applies swing feel by delaying even subdivisions |
| Shift Timing | Ticks | Global offset applied to all events |

### 7.5 Sequence Edit Operations

| Operation | Description |
|-----------|-------------|
| **Erase** | Remove specific event types from a region (notes, automation, or both) |
| **Clear** | Remove all events from the sequence |
| **Trim** | Cut the sequence to the loop region |
| **Transpose** | Shift note events up or down by semitones |
| **Delete Bars** | Remove bars from the sequence |
| **Insert Bars** | Add empty bars at a position |
| **Half Length** | Compress all events to half the sequence length |
| **Double Length** | Expand all events to double the sequence length |
| **Copy Sequence** | Copy events to another sequence |
| **Bounce Tracks** | Merge events from multiple tracks onto a single track |

### 7.6 Clip Program (Clip Matrix / Launch Mode)

The MPC supports a clip-based workflow alongside the traditional sequence model:

- Sequences can be launched as clips in a matrix view
- **Launch modes** control playback behavior when triggering clips
- Clips can be set to loop or one-shot
- Clip quantize determines when clip launch takes effect (immediately, next bar, next beat, etc.)

**Cross-ref:** [09-sequences-events.md](09-sequences-events.md)

---

## 8. Automation

### 8.1 Automatable Parameters

The MPC supports automation across **10 parameter categories** (from the manual):

| # | Category | Description | Example Parameters |
|---|----------|-------------|--------------------|
| 1 | Track Volume | Per-track volume level | Level fader position |
| 2 | Track Pan | Per-track stereo position | Pan knob position |
| 3 | Track Mute | Per-track mute state | Mute on/off |
| 4 | Pad Volume | Per-pad volume level | Pad Mixer level |
| 5 | Pad Pan | Per-pad stereo position | Pad Mixer pan |
| 6 | Pad Filter | Per-pad filter settings | Cutoff, resonance |
| 7 | Pad Tune | Per-pad pitch/tune | Pitch offset |
| 8 | Send Levels | Per-track or per-pad send amounts | Send 1-4 levels |
| 9 | Plugin Parameters | Instrument or effect plugin parameters | Any exposed plugin knob |
| 10 | Velocity / Probability / etc. | Note modifiers | Per-note velocity, probability, timing offsets |

### 8.2 Global Automation Modes

| Mode | Symbol | Behavior |
|------|--------|----------|
| **Read** | R | Plays back recorded automation. Knob movements are not recorded. |
| **Write** | W | Records automation over playback. New movements overwrite existing data. |
| **Off** | -- | Ignores all automation. Parameters stay at their manual positions. |

### 8.3 Per-Track Automation Control

Each track has independent automation settings:

| Setting | Options | Description |
|---------|---------|-------------|
| Automation Enable | On / Off | Whether this track reads/writes automation |
| Overdub | On / Off | Layer new automation over existing without erasing |

### 8.4 Recording Modes

| Mode | Behavior |
|------|----------|
| **Touch** | Records automation only while a control is being touched/moved. When released, the parameter returns to its previously automated value. |
| **Latch** | Records automation while a control is moved. When released, the last value is held and continues writing until playback stops. |
| **Relative** | Offsets existing automation values rather than replacing them. Adds/subtracts from the existing automation curve. |

### 8.5 Automation Event Storage

Automation events are stored as type-1 (track-level) and type-2 (pad-level) events in the sequence data. Each event records a parameter ID, value, and tick position. The special note value **256** serves as a sentinel indicating an automation event rather than a note event.

**Cross-ref:** [11-automation.md](11-automation.md)

---

## 9. Song Mode

### 9.1 Overview

Song Mode arranges sequences into a linear arrangement for full song playback.

- **32 song slots** available per project
- Each song is a linear list of **steps**, where each step references a sequence
- Songs play sequences in order, providing a traditional arrangement workflow on top of the pattern-based sequence system

### 9.2 Step Properties

| Property | Description |
|----------|-------------|
| Sequence | Which sequence (1-128) plays at this step |
| Repeats | How many times to repeat this sequence before advancing |
| Tempo Override | Optional BPM override for this step (otherwise uses sequence BPM) |

### 9.3 Song Playback

- Songs play from start to end by default
- **Loop** can be enabled to repeat the entire song
- Song playback follows the arrangement strictly -- no clip launch behavior
- Songs are rendered via Audio Mixdown (Menu > Save > Audio Mixdown) for final export

### 9.4 Current Status

Song mode data structures exist in the XPJ format (32 song slots always present), but in all analyzed project files these slots have been empty. The feature is fully functional on the hardware but appears to be rarely used in practice compared to the sequence/clip workflow.

**Cross-ref:** [09-sequences-events.md](09-sequences-events.md)
