# A2 — OPx-4 FM Synth Parameters

> **XPJ Format** | [Index](README.md) | **Source:** AIR OPx-4 User Guide v1.1

The AIR OPx-4 is an FM synthesizer plugin instrument available in the MPC. Its internal state is stored as an opaque base64 blob in the `program.plugin.plugin.state` field (see [05-plugin-program.md](05-plugin-program.md)). This appendix documents the OPx-4's internal parameters for reference.

**Plugin UID:** 1330673716

---

## Architecture Overview

- 4 FM operators with 4x4 FM matrix
- Flexible wave shaping (PW, Formant, FM Filtering, FM Shaping per operator)
- Sample layer for percussive attacks
- Dual-mode filter path with 23 filter types
- 6 envelopes with tempo-synced looping (DADSR/Loop/One-Shot modes)
- 2 LFOs + 2 ramps
- 32-slot modulation matrix with 15 shaper types
- 8 assignable macro knobs
- 3 insert FX slots (13 effects) + 2 global FX slots (27 effects)
- Bus Mix: 3-bus routing through Filter 1, Filter 2, or direct

---

## Global Controls & Macros

| Parameter | Description | Value Range |
|-----------|-------------|-------------|
| Mix & FX | Toggles main view between Operators and Mix & FX | Off, On |
| Macros (8) | Preset-specific assignable controls | Varies |
| Pan | Stereo panning | L64 -- C -- R64 |
| Level | Overall volume | -inf -- 0.0 -- +6.0 dB |

## Operators and FM Matrix

| Parameter | Description | Value Range |
|-----------|-------------|-------------|
| Operators 1--4 | Enable/disable each voice | Off, On |
| Ratio | Frequency ratio (doubling = +1 octave) | 0.0000 -- 64.0000 |
| Offset | Frequency offset in Hz from original pitch | -9999.00 -- 0 -- 9999.00 |
| Level | Volume of operator in mix | 0--100% |
| PW | Pulse width modulation depth | 0--100% |
| Formant | Resonant frequency emphasis for timbre | 0.00 -- 10.00 |
| FM Matrix | 4x4 modulation levels between operators | -100 -- 0 -- +100% |
| FM Shaping | Blend between original and squared operator value | 0--100% |
| FM Filtering | High-end filtering to reduce harshness | 0--100% |
| FM Scaling | Overall FM Matrix modulation amount | 0--100% |

## Global / Sample

| Parameter | Description | Value Range |
|-----------|-------------|-------------|
| Transpose | Semitone transposition | -48 -- 0 -- +48 |
| Tune | Fine tuning in cents | -50 -- 0 -- +50 |
| Polyphony | Voice count | Legato, Retrigger, 2--7, Poly |
| Glide | Pitch gliding enable | Off, On |
| Legato | Legato-only glide | Off, On |
| Time | Glide time | 0.0 -- 2000.0 ms |
| **Sample Layer** | | |
| Layer | Sample sound selection | Varies |
| Looped | Loop enable | Off, On |
| Transpose | Sample transposition | -48 -- 0 -- +48 |
| Tune | Sample tuning offset | -50 -- 0 -- +50 cents |
| Level | Sample volume | 0--100% |
| Key Track | Pitch-to-note tracking | 0--100% |
| Dec / Rel | Decay and release length | 10.00 -- 5000.00 ms |
| Velocity | Velocity-to-level sensitivity | 0--100% |

## Filters 1/2

| Parameter | Description | Value Range |
|-----------|-------------|-------------|
| Type | Filter type (23 types) | Off, LP4, LP3, LP2, LP1, BP2, BP4, HP2+LP1, HP3+LP1, HP4, HP3, HP2, HP1, BR2, BR4, BR2+LP1, BR2+LP2, HP1+BR2, BP2+BR2, HP1+LP2, HP1+LP3, AP3, AP3+LP1, HP1+AP3 |
| Cutoff | Center cutoff frequency | 55.0 Hz -- 20.0 kHz |
| Res | Resonance amount | 0.7--20.0 |
| Drive | Overdrive on filter signal | 0--100% |

## Envelopes 1--6

Each of the 6 envelopes has identical parameters. Points are also draggable in the graph UI.

| Parameter | Description | Value Range |
|-----------|-------------|-------------|
| Time Scale | Envelope duration scale | 10.00 -- 100.00% |
| Tempo Sync | Length relative to tempo | 16 -- 8/4, Off |
| Mode | Envelope type | DADSR, Loop, One-Shot |
| Delay | Time before envelope start | 0.00 -- 15000.00 ms |
| Attack | Time to reach full level | 0.50 -- 10000.00 ms |
| Attack Curve | Attack shape | 0--100% |
| Decay | Initial decay time | 1.00 -- 10000.00 ms |
| Decay Level | Level of initial decay vs attack | 0--100% |
| Decay Curve | Decay shape | 0--100% |
| Decay 2 | Secondary decay to sustain | 1.00 -- 10000.00 ms |
| Decay 2 Curve | Decay 2 shape | 0--100% |
| Sustain | Held note level | 0--100% |
| Release | Dissipation time on release | 10.00 -- 5000.00 ms |
| Release Curve | Release shape | 0--100% |

### Envelope Scaling (per envelope)

| Parameter | Value Range |
|-----------|-------------|
| Delay, Attack, Decay 1, Decay 1 Level, Decay 2, Sustain, Release | 0--100% each |
| Time Stretch | 100.00--400.00% |

## LFOs 1/2

| Parameter | Description | Value Range |
|-----------|-------------|-------------|
| Global | Enable/disable LFO | Off, On |
| Sync | Sync to global tempo | Off, On |
| Type | Waveform type | Ramp Up, Ramp Down, Triangle, Sine, Square, Rnd1, Rnd2 |
| Speed (free) | Speed in Hz | 0.10 -- 50.00 Hz |
| Speed (sync) | Speed in note divisions | 16 -- 8/4 (17 values) |
| Level | Modulation amount | 0--100% |
| Delay | Delay before LFO start | 0.00 -- 15000.00 ms |
| Phase | Waveform start position | 0--100% |
| Fade In | Fade-in time | 0.00 -- 15000.00 ms |

### Utilities

| Parameter | Description | Value Range |
|-----------|-------------|-------------|
| Note Counter 1/2 Notes | Voice count | 2--16 |
| Note Counter 1/2 Mode | Trigger mode | Wrap, Random, Ping Pong |
| Velocity Curve | Force-to-velocity mapping | -100 -- 0 -- +100% |
| Ramp 1/2 Global | Apply ramp globally | Off, On |
| Ramp 1/2 Time | Ramp length | 10.00 -- 10000.00 ms |
| Ramp 1/2 Curve | Ramp shape | -100 -- 0 -- +100% |

## Modulation Matrix (32 slots)

| Parameter | Description | Value Range |
|-----------|-------------|-------------|
| Power | Enable/disable mod point | Off, On |
| Mod | Modulation amount | 0--100% |
| Type | Modulation type | Bipolar, Unipolar, Bipolar (scaled), Unipolar (reverse) |
| Min | Minimum modulation level | 0--100% |
| Max | Maximum modulation level | 0--100% |
| Shaper | Modulation curve shape | Off, Square, Cubic, InvSquare, InvCubic, SquareRoot, CubeRoot, Sine, DoubleSine, Quantize0025/0050/0075/2550/5075/75100 |
| Amount | Shaper amount | 0--100% |
| Source | Input modulation source | Varies |
| Target | Output modulation target | Varies |

## Bus Mix

3 bus mixes routing Operators 1--4 + Sample layer through Filter 1, Filter 2, or direct (no filter).

| Parameter | Description | Value Range |
|-----------|-------------|-------------|
| Operator 1--4 | Volume in each submix | 0--100% |
| Sample | Sample layer level in each submix | 0--100% |
| Pan | Stereo panning of each submix | L64 -- C -- R64 |

Row 1 -> Filter 1 -> FX1. Row 2 -> Filter 2 -> FX2. Row 3 -> FX3 (no filter). All -> Global FX 1--2.

---

## Insert FX 1--3

13 available effects: Chorus, Ensemble, Tremolo, Delay, Multitap Delay, Highpass, EQ (3 bands), Phaser, Phase (vintage), Flanger (vintage), Tube Drive, Compressor, Expander.

### Chorus

| Parameter | Description | Value Range |
|-----------|-------------|-------------|
| Rate | Modulation speed | 0.01 -- 10.0 Hz |
| Depth | Modulation depth | 0.00 -- 24.00 ms |
| Mix | Wet/dry | 0--100% |
| Delay | Delay between original and modulated | 0.00 -- 24.00 ms |
| Feedback | Signal feedback | 0--100% |
| Offset | LFO wave start offset | -180 -- 0 -- +180 deg |
| LFO Wave | Modulation wave type | Tri, Sine |

### Ensemble

| Parameter | Description | Value Range |
|-----------|-------------|-------------|
| Rate | Modulation speed | 0.01 -- 10.0 Hz |
| Depth | Modulation depth | 0.00 -- 24.00 ms |
| Mix | Wet/dry | 0--100% |
| Delay | Delay between original and modulated | 0.00 -- 24.00 ms |
| Shimmer | Randomizes Delay time, adding texture | 0--100% |
| Width | Stereo width | 0--100% |

### Tremolo

| Parameter | Description | Value Range |
|-----------|-------------|-------------|
| Sync | Sync Rate to tempo | Off, On |
| Rate (free) | Speed | 0.25 -- 13.00 Hz |
| Rate (sync) | Speed in note divisions | 8/4 -- 16 |
| Depth | Modulation depth | 0--100% |
| Sync Phase | Waveform start position | -180 -- 0 -- +180 |
| Shape | Modulation wave | Sine, Sqr |
| Mode | Effect type | Trem, Pan |

### Delay

| Parameter | Description | Value Range |
|-----------|-------------|-------------|
| Sync | Sync to tempo | Off, On |
| Type | Delay type | Mono, Stereo, Cross |
| Delay (free) | Delay time | 1 ms -- 4.00 s |
| Delay (sync) | Delay in note divisions | 16 -- 8/4 |
| Feedback | Signal feedback | 0--100% |
| Mix | Wet/dry | 0--100% |
| L / R Ratio | Stereo offset | L 50:100 -- R 100:50 |
| Width | Stereo width | 0--100% |
| Low Cut | Low-cut filter frequency | 20.0 Hz -- 1.0 kHz |
| High Cut | High-cut filter frequency | 1.00 kHz -- 20.0 kHz |

### Multitap Delay

| Parameter | Description | Value Range |
|-----------|-------------|-------------|
| From | Feedback source | Tap 1--5 |
| To | Feedback destination | Input, Tap 1--5 |
| Sync | Sync to tempo | Off, On |
| Low Cut | Low-cut filter | 20.0 Hz -- 1.0 kHz |
| High Cut | High-cut filter | 1.00 kHz -- 20.0 kHz |
| Delay (free) | Delay time | 1 ms -- 4.00 s |
| Delay (sync) | Delay in note divisions | 16 -- 8/4 |
| Feedback | Signal feedback | 0--100% |
| Mix | Wet/dry | 0--100% |
| Tap 1--5 | Enable/disable each tap | Off, On |
| Tap Delay 1--5 | Percent of Delay value | 0--100% |
| Tap Level 1--5 | Volume of tap | -inf -- 0.0 dB |
| Tap Pan 1--5 | Stereo panning of tap | L100 -- C -- R100 |

### Highpass

| Parameter | Description | Value Range |
|-----------|-------------|-------------|
| Cutoff | Cutoff frequency | 0--1000 Hz |
| Resonance | Resonance amount | 0--100% |

### EQ (3 bands)

| Parameter | Description | Value Range |
|-----------|-------------|-------------|
| Low Gain | Low band level | -15.0 -- 0 -- +15.0 dB |
| Low Freq | Low band frequency | 20.0 Hz -- 1.00 kHz |
| Mid Gain | Mid band level | -15.0 -- 0 -- +15.0 dB |
| Mid Q | Mid band width | 0.50--10.00 |
| Mid Freq | Mid band frequency | 40.0 Hz -- 16.0 kHz |
| High Gain | High band level | -15.0 -- 0 -- +15.0 dB |
| High Freq | High band frequency | 2.00 -- 20.0 kHz |
| Output | Post-EQ output level | -20.0 -- 0 -- +20.0 dB |

### Phaser

| Parameter | Description | Value Range |
|-----------|-------------|-------------|
| Sync | Sync Rate to tempo | Off, On |
| Rate (free) | Speed | 0.01 -- 10.0 Hz |
| Rate (sync) | Speed in note divisions | 8/4 -- 16 |
| Depth | Modulation amount | 0--100% |
| Feedback | Signal feedback | 0--100% |
| Mix | Wet/dry | 0--100% |
| Low Cut | Low-cut filter | 20.0 Hz -- 1.00 kHz |
| Center | Center frequency of poles | 100 Hz -- 10.0 kHz |
| Poles | Number of phase stages | 2, 4, 6, 8 |
| LFO Wave | Triangle to sine blend | 0--100% |
| Offset | LFO phase offset | -180 -- 0 -- +180 deg |

### Phase (vintage)

| Parameter | Description | Value Range |
|-----------|-------------|-------------|
| Rate | Modulation speed | 0.10 -- 10.00 Hz |
| Depth | Modulation amount | 0--100% |
| Feedback | Signal feedback | 0--100% |
| Mix | Wet/dry | 0--100% |
| Offset | Phase or Rate offset | Phase: -180 -- +180 deg; Rate: 25--400% |
| Phase/Rate | What Offset affects | Phase, Rate |
| Model | Vintage phaser model | Vibe, Stone, Ninety, Tron |

### Flanger (vintage)

| Parameter | Description | Value Range |
|-----------|-------------|-------------|
| Rate | Modulation speed | 0.10 -- 10.00 Hz |
| Depth | Modulation amount | 0--100% |
| Mix | Wet/dry | 0--100% |
| Feedback | Signal feedback | 0--100% |
| Headroom | Gain reduction | -20.0 -- 0.0 dB FS |

### Tube Drive

| Parameter | Description | Value Range |
|-----------|-------------|-------------|
| Drive | Drive amount | 0--100% |
| Headroom | Distortion threshold | -30.0 -- 0.0 dB |
| Saturation | Saturation amount | 0--100% |
| Output | Post-drive output level | -20.0 -- 0 -- +20.0 dB |

### Compressor

| Parameter | Description | Value Range |
|-----------|-------------|-------------|
| Type | Compression type | Peak, RMS, Opto |
| Threshold | Signal level threshold | -40.0 -- 0.0 dB |
| Ratio | Compression ratio | 1.0:1 -- 100:1 |
| Output | Output gain | -20.0 -- 0 -- +20.0 dB |
| Knee | Knee shape | 0--100% |
| Attack | Attack time | 10.0 us -- 100 ms |
| Release | Release time | 10.0 ms -- 10.00 s |
| Low Sens | Low frequency sensitivity | -12.0 -- 0 -- +12.0 dB |
| High Sens | High frequency sensitivity | -12.0 -- 0 -- +12.0 dB |

### Expander

| Parameter | Description | Value Range |
|-----------|-------------|-------------|
| Threshold | Signal level threshold | -40.0 -- 0.0 dB |
| Ratio | Expansion ratio | 1:1.0 -- 1:100 |
| Range | Dynamic range above threshold | 0.0 -- 40.0 dB |
| Output | Output level | -20.0 -- 0 -- +20.0 dB |
| Attack | Attack time | 10.0 us -- 100 ms |
| Release | Release time | 10.0 ms -- 10.00 s |

---

## Global FX 1--2

27 available effects. Effects shared with Insert FX have the same parameters (see above). Additional Global FX-only effects listed below.

### MultiChorus

| Parameter | Description | Value Range |
|-----------|-------------|-------------|
| Rate | Modulation speed | 0.01 -- 10.0 Hz |
| Depth | Modulation depth | 0.00 -- 24.00 ms |
| Voice Count | Number of chorus copies | 3, 4, 6 |
| Mix | Wet/dry | 0--100% |
| Delay | Delay between original and modulated | 0.00 -- 24.00 ms |
| Width | Stereo width | 0--100% |
| Low Cut | Low-cut filter | 20.0 Hz -- 1.00 kHz |
| LFO Wave | Modulation wave type | Tri, Sine |

### Tape Delay

| Parameter | Description | Value Range |
|-----------|-------------|-------------|
| Tape Head On/Off 1--4 | Enable/disable each head | Off, On |
| Tape Head Delay 1--4 | Percent of main Delay | 0--100% |
| Tape Head Mix 1--4 | Wet/dry per head | 0--100% |
| Tape Head Pan 1--4 | Stereo panning per head | L100 -- C -- R100 |
| Tape Head Feedback 1--4 | Feedback per head | 0--100% |
| Sync | Sync to tempo | Off, On |
| Delay (free) | Delay time | 1 -- 4000 ms |
| Delay (sync) | Delay in note divisions | 16 -- 8/4 |
| Speed | Simulated tape speed | 0.0 -- 15.0 ips |
| Input | Input level | -inf -- 0 -- +12.0 dB |
| Output | Output level | -inf -- 0 -- +12.0 dB |
| Feedback | Signal feedback | 0--100% |
| Low Cut | Low-cut filter | 20.0 Hz -- 1.0 kHz |
| High Cut | High-cut filter | 1.00 kHz -- 20.0 kHz |
| Mix | Wet/dry | 0--100% |
| Wow Rate | Wow pitch variation speed | 0.10 -- 20.0 Hz |
| Wow Depth | Wow modulation depth | 0--100% |
| Flutter Rate | Flutter pitch variation speed | 10.0 Hz -- 1.00 kHz |
| Flutter Depth | Flutter modulation depth | 0--100% |

### EQ (Parametric)

| Parameter | Description | Value Range |
|-----------|-------------|-------------|
| Output | Post-EQ gain | -20.0 -- 0.0 -- +20.0 dB |
| Band On/Off | Enable/disable each band | Off, On |
| Low Cut | Low cut slope | 6, 12, 18, 24 dB |
| Low Cut Freq | Low cut frequency | 20.0 Hz -- 8.00 kHz |
| Low Gain | Low band level | -12.0 -- 0.0 -- +12.0 dB |
| Low Q | Low band width | 0.40 -- 2.00 |
| Low Freq | Low band frequency | 20.0 Hz -- 1.00 kHz |
| Low Shelf/Bell | Band type | Shelf, Bell |
| Low Mid Gain | Low mid level | -18.0 -- 0.0 -- +18.0 dB |
| Low Mid Q | Low mid width | 0.40 -- 10.00 |
| Low Mid Freq | Low mid frequency | 40.0 Hz -- 8.00 kHz |
| High Mid Gain | High mid level | -18.0 -- 0.0 -- +18.0 dB |
| High Mid Q | High mid width | 0.40 -- 10.00 |
| High Mid Freq | High mid frequency | 120 Hz -- 16.0 kHz |
| High Gain | High band level | -18.0 -- 0.0 -- +18.0 dB |
| High Q | High band width | 0.40 -- 2.00 |
| High Freq | High band frequency | 1.20 -- 20.0 kHz |
| High Shelf/Bell | Band type | Shelf, Bell |
| High Cut | High cut slope | 6, 12, 18, 24 dB |
| High Cut Freq | High cut frequency | 120 Hz -- 20.0 kHz |

### Flanger

| Parameter | Description | Value Range |
|-----------|-------------|-------------|
| Sync | Sync Rate to tempo | Off, On |
| Trigger | Manual LFO phase reset | Off, On |
| Invert | Invert polarity of flanged signal | Off, On |
| Rate (free) | Speed | 0.01 -- 10.0 Hz |
| Rate (sync) | Speed in note divisions | 8/4 -- 16 |
| Depth | Delay time modulation | 0.00 -- 12.00 ms |
| Delay | Dry-to-delayed time | 0.00 -- 12.00 ms |
| Mix | Wet/dry | 0--100% |
| Feedback | Signal regeneration | -100 -- 0 -- +100% |
| Low Cut | Low-cut filter | 20.0 Hz -- 1.00 kHz |
| LFO Wave | Triangle to sine blend | 0--100% |
| Offset | Stereo LFO phase offset | -180 -- 0 -- +180 deg |

### Amp Sim

| Parameter | Description | Value Range |
|-----------|-------------|-------------|
| Cab Model | Amplifier type | D.I., Brit, 1x8", 1x12", 2x10", 2x12", 4x10", 4x12", 1x15" Bass, 4x10" Bass, Radio |
| Drive | Drive amount | 0--11.0 |
| Feedback | Amplifier feedback | 0--100% |
| Output | Output level | -12.0 -- 0.0 -- +12.0 dB |
| Soft Clip | Soft clipping warmth | 0--100% |
| Top Boost | Treble gain boost | 0--100% |
| Edge | Asymmetric clipping | 0--100% |
| Treble | High frequency EQ | -12.0 -- 0.0 -- +12.0 dB |
| Mid | Mid frequency EQ | -12.0 -- 0.0 -- +12.0 dB |
| Mid Freq | Mid center frequency | 250 Hz -- 4.00 kHz |
| Bass | Low frequency EQ | -12.0 -- 0.0 -- +12.0 dB |
| Mono/Stereo | Output mode | Mono, Stereo |

### Talk Box

| Parameter | Description | Value Range |
|-----------|-------------|-------------|
| LFO Wave | LFO waveform | Sine, Tri, Saw, Square, S&H, Random |
| LFO Sync | Sync to tempo | Off, On |
| LFO Rate (free) | Speed | 0.01 -- 10.0 Hz |
| LFO Rate (sync) | Speed in note divisions | 8/4 -- 16 |
| LFO Depth | Modulation amount | -100 -- 0 -- +100% |
| Vowel | Formant filter shape | 0.000--1.000 (OO->OU->AU->AH->AA->AE->EA->EE->EH->ER->UH->OH->OO) |
| Formant | Center formant shift | -12.00 -- 0.00 -- +12.00 semitones |
| Mix | Wet/dry | 0--100% |
| Env Threshold | Envelope follower threshold | -60.0 -- 0.0 dB |
| Env Attack | Envelope trigger time | 0.1 -- 10.0 s |
| Env Release | Envelope release time | 0.1 -- 10.0 s |
| Env Depth | Envelope offset to Vowel | -100 -- 0 -- 100% |

### FuzzWah

| Parameter | Description | Value Range |
|-----------|-------------|-------------|
| Fuzz | Enable fuzz distortion | Off, On |
| Drive | Fuzz gain | 0 -- 40 dB |
| Tone | Fuzz brightness | 1.00 -- 10.0 kHz |
| Output | Fuzz output level | -Inf -- 0.0 dB |
| Wah | Enable wah | Off, On |
| Pedal | Wah pedal position | 0--100% |
| Rate | Modulation speed | 8/4 -- 16 |
| Depth | Modulation depth | -100 -- 0 -- +100% |
| Filter | Wah filter type | Lowpass, Bandpass, Highpass |
| Mod | Modulation source | LFO, Env |
| Min Freq | Wah filter min frequency | 50.0 Hz -- 4.00 kHz |
| Max Freq | Wah filter max frequency | 50.0 Hz -- 4.00 kHz |
| Min Reso | Wah min resonance | 0--100% |
| Max Reso | Wah max resonance | 0--100% |
| Order | Effect order | Fuzz>Wah, Wah>Fuzz |
| Fuzz Mix | Fuzz wet/dry | 0--100% |
| Wah Mix | Wah wet/dry | 0--100% |
| Mix | Combined wet/dry | 0--100% |

### Overdrive

| Parameter | Description | Value Range |
|-----------|-------------|-------------|
| Drive | Input overdrive amount | 0--60 dB |
| Mode | Overdrive mode | Hard, Soft, Wrap |
| Stereo | Stereo/mono output | Off, On |
| Output | Output level | 0--100% |
| Mix | Wet/dry | 0--100% |
| Pre-Shape | Treble gain boost/cut in processed signal | -100 -- 0 -- 100% |
| Threshold | Dynamic range headroom | -20.0 -- 0.0 dB FS |
| Edge | Asymmetric clipping | 0--100% |
| High-Cut | High-cut filter | 1.00 -- 20.0 kHz |

### Decimator

| Parameter | Description | Value Range |
|-----------|-------------|-------------|
| Bit Depth | Bit depth reduction | 1.0 -- 16.0 bit |
| Sample Rate | Sample rate reduction | 500 Hz -- 50.0 kHz |
| Mix | Wet/dry | 0--100% |
| Anti-Alias | Enable anti-aliasing | Off, On |
| Pre | Pre-resample filter cutoff | 0.125 -- 2.000 Fs |
| Post | Post-resample filter cutoff | 0.125 -- 2.000 Fs |
| Clip | Transistor-like distortion | 0.0 -- 40.0 dB |
| Rectify | Waveshaper distortion | 0--100% |
| Noise Mod | Noisy modulation | 0--100% |
| LFO Sync | Sync Rate to tempo | Off, On |
| LFO Rate (free) | Speed | 0.01 -- 10.0 Hz |
| LFO Rate (sync) | Speed in note divisions | 8/4 -- 16 |
| LFO Depth | Modulation amount | -100 -- 0 -- 100% |
| Attack | Envelope attack time | 0.1 -- 10.0 s |
| Release | Envelope release time | 0.1 -- 10.0 s |
| Env Depth | Envelope offset | -100 -- 0 -- +100% |

### Maximizer

| Parameter | Description | Value Range |
|-----------|-------------|-------------|
| Threshold | Signal level threshold | -40.0 -- 0.0 dB |
| Ceiling | Maximum output level | -20.0 -- 0.0 dB FS |
| Look Ahead | Preview time for smoothing attacks | 0.0 -- 20.0 ms |
| Release | Release time | 10.0 ms -- 10.0 s |
| Knee | Knee shape | Hard, Soft |
| LF Mono | Frequencies below this are summed to mono | 10.0 Hz -- 1.00 kHz |

### Enhancer

| Parameter | Description | Value Range |
|-----------|-------------|-------------|
| Low Gain | Low frequency enhancement | 0.0 -- 12.0 dB |
| Low Freq | Low band center frequency | 40.0 -- 640 Hz |
| Harmonics | Harmonic overtone level | 0.0 -- 12.0 dB |
| Phase | Harmonic polarity | + (positive), - (negative) |
| Output | Output level | -Inf -- 0.0 dB |
| High Gain | High frequency enhancement | 0.0 -- 12.0 dB |
| High Freq | High band center frequency | 1.0 -- 10.0 kHz |

### Reverb

| Parameter | Description | Value Range |
|-----------|-------------|-------------|
| Type | Reverb type | Hall, Stadium, Room, Abstract |
| Time | Reverb tail length | 0.4 s -- +Inf s |
| Low Cut | Low-cut filter | 1 -- 1000 Hz |
| High Cut | High-cut filter | 1.0 -- 20.0 kHz |
| Mix | Wet/dry | 0--100% |

### Spring Reverb

| Parameter | Description | Value Range |
|-----------|-------------|-------------|
| Pre-Delay | Delay before reverb | 0 -- 250 ms |
| Time | Reverb tail length | 1.00 -- 10.0 s |
| Mix | Wet/dry | 0--100% |
| Low Cut | Low-cut filter | 20.0 Hz -- 1.00 kHz |
| Diffusion | Reflection density | 0--100% |
| Width | Stereo width | 0--100% |

### Gated Reverb

| Parameter | Description | Value Range |
|-----------|-------------|-------------|
| Dry Delay | Delay on dry signal | 0--1500 ms |
| Pre-Delay | Delay before reverb | 0--250 ms |
| Time | Reverb tail length | 0--1000 ms |
| Mix | Wet/dry | 0--100% |
| Diffusion | Reflection density | 0--100% |
| Low Cut | Low-cut filter | 20.0 Hz -- 1.00 kHz |
| High Cut | High-cut filter | 20.0 Hz -- 1.00 kHz |
| Width | Stereo width | 0--100% |
| Shape | Reverb shape | Gated, Reverse |

---

## XPJ Storage

Like all plugin instruments, OPx-4 parameters are stored as an opaque base64-encoded state blob in the `Data` field of the `.xpm` program file. Structured metadata includes: Name, DescriptiveName, FormatName (`MPC`), Category (`Synth`), ManufacturerName, FileOrID, UID, IsInstrument, NumInput (0), NumOutput (2), PresetName.

**Observed in project files:**
- `Plugin 001.Plugin.xpm` (preset: OPx-4-Subbass-03) -- Techsession-01
- `Min-01-synth-call-02.Plugin.xpm` -- Tech-11, Tech-session-2, Tech-10

## Sources

- AIR OPx-4 User Guide v1.1 (`docs/opx-4.pdf`, 28 pages)
- Real project file analysis (UID 1330673716 / `MPC:OPx-4`)
