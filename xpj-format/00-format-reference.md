# 00 — Format Reference

> **XPJ Format** | [Index](README.md)

This document collects the cross-cutting conventions, serialization patterns, format detection rules, and known quirks that apply throughout the XPJ schema. It serves as a lookup reference for anyone reading or writing MPC project files.

---

## Confidence Levels

Every field entry in the schema documentation carries a confidence rating:

| Level | Meaning |
|-------|---------|
| **HIGH** | Field observed in 3+ project files with consistent type and value behavior, OR directly tested with known inputs |
| **MEDIUM** | Field observed in 1-2 files, OR inferred from key name + value pattern |
| **LOW** | Field present but purpose unclear; only present in one file or always at default; never varied |

## Coverage Statuses

Each field also carries a coverage status:

| Status | Meaning |
|--------|---------|
| **CONFIRMED** | Directly observed and verified against hardware behavior or manual |
| **PARTIAL** | Observed but incomplete understanding (e.g., some enum values unknown) |
| **UNCONFIRMED** | Expected based on manual or structure, but not yet observed in any project file |
| **CONSTANT** | Always the same value across all 18 analyzed files; true purpose inferred |

---

## Key Serialization Patterns

The MPC firmware's C++ serialization layer produces several recurring patterns in the JSON output. These must be handled correctly by any parser or builder.

| Pattern | Description | Example |
|---------|-------------|---------|
| **indexed-dict** | Arrays stored as an object with `value0`/`value1`/`valueN` keys | `"noteForPad": {"value0": 36, "value1": 37, ...}` |
| **EnumCerealisationWrapper** | Enum values wrapped with a type-annotation key | `"EnumCerealisationWrapper(behaviour)": "Off"` |
| **Object volume** | Layer/pad volumes are objects, NOT floats | `"volume": {"gainCoefficient": 0.5, "controlValue": 0.5, "law": 0}` |
| **Float volume** | Track-level volumes ARE simple floats | `"volume": 0.7079` |
| **ADSR wrapper** | Envelope params wrapped in single-key objects | `"Attack": {"value0": 0.0}` |
| **Pan center** | Pan is 0.0-1.0 with 0.5039... as center (float precision artifact) | `"pan": 0.5039370059967041` |

---

## Known Quirks

| Quirk | Detail |
|-------|--------|
| `poliphony` typo | JSON uses `"poliphony"` (missing 'y') -- must be preserved exactly as-is |
| INT64_MAX sentinel | `"length": 9223372036854775807` = unbounded sequence |
| `note=256` sentinel | Automation events targeting track-level params use `note=256` (out of MIDI range) |
| BPM float imprecision | Tempos stored as 32-bit float: `93.00003814697266` not `93.0` |
| Colour as integer | `"colour": 16711680` = `0xFF0000` = red; encoding is `(R<<16)\|(G<<8)\|B` |

---

## MPC 2.x vs 3.x Format Detection

Both format generations share the `.xpj` extension. Detection must be content-based.

### Structure comparison

| Aspect | MPC 2.x | MPC 3.x |
|--------|---------|---------|
| Container | Plain XML text | Gzip-compressed ACVS header + JSON |
| Magic bytes | `3C 3F 78 6D 6C` (`<?xml`) | `1F 8B` (gzip) |
| Project structure | XML manifest + companion folder (.xpm, .xal, .sxq, .mcn) | Single gzipped JSON + WAV-only companion folder |
| Sequences | Standard MIDI files (.sxq at 960 PPQ) | Embedded JSON events |
| Programs | Separate .xpm XML files | Embedded in `tracks[]` JSON array |
| Effects | XML `<Insert1>`...`<Insert4>` tags | JSON `effects[]` array |
| Plugin state | `<Data>` XML tag, custom base64 variant | `state` JSON field, standard base64 |
| Settings | Separate `Project Settings.xml` | Embedded in root JSON |
| MIDI controller | Binary `.mcn` file (~350 KB) | Embedded `midiLearnSettings` JSON |
| Firmware range | 2.0 through ~2.12.x | 3.0 through current (3.7.0.56 confirmed) |

### Detection method

| Check | MPC 2.x (XML) | MPC 3.x (JSON) |
|-------|----------------|-----------------|
| `file` command | `XML 1.0 document, ASCII text` | `gzip compressed data, from Unix` |
| Magic bytes | `3C 3F 78 6D 6C` (`<?xml`) | `1F 8B` (gzip magic) |
| First readable line | `<?xml version="1.0" encoding="UTF-8"?>` | `ACVS` (after decompression) |
| Companion folder | Contains .xpm, .xal, .sxq, .mcn, .xml files | Contains only .wav files |

Always check magic bytes first. Gzip magic (`\x1f\x8b`) = 3.x format. XML declaration = 2.x format. This is the only reliable detection method.

### 3.x ACVS header

After gzip decompression the first 5 lines are a text header before the JSON payload:

| Line | Content | Purpose | Variability |
|------|---------|---------|-------------|
| 1 | `ACVS` | Magic identifier | Likely constant across all 3.x versions |
| 2 | `3.7.0.56` | Firmware version (major.minor.patch.build) | Changes with every firmware release |
| 3 | `SerialisableProjectData` | Object type name | Likely constant for .xpj |
| 4 | `json` | Serialization format | Likely constant |
| 5 | `Linux` | OS platform | `Linux` for standalone hardware; possibly `Windows`/`macOS` for MPC Beats/Software |

**To read:** `gzip.decompress()` -> skip first 5 lines -> `json.loads()` -> access `["data"]`

**To write:** Build JSON -> prepend 5 header lines -> `gzip.compress()` -> save as `.xpj`

---

## Companion File Formats (2.x)

In MPC 2.x the `.xpj` is only a manifest. The actual project data lives in a companion folder named `ProjectName_[ProjectData]/`.

| Extension | Format | Purpose | Notes |
|-----------|--------|---------|-------|
| `.xpj` | XML | Project manifest | Contains `<FileList>`, `<BPM>`, `<MasterVolume>`, `<Mixer>`, `<ProductCode>` |
| `.xpm` | XML with embedded JSON | Program definition | Type specified in `<Program type="Drum\|Midi\|Audio\|Plugin\|Keygroup">`. Contains HTML-entity-escaped JSON blobs for ProgramPads. |
| `.xal` | XML | All Sequences & Songs container | Contains all 128 sequence slots + 32 song slots |
| `.sxq` | Standard MIDI Format 1 | Individual sequence file | 960 PPQ; directly parseable with any MIDI library |
| `.mcn` | Binary/proprietary | MIDI controller configuration | Not yet analyzed; ~350 KB observed |
| `.xfx` | XML | Insert effects chain preset | Contains up to 4 insert slots with plugin descriptions + state blobs |
| `.xpl` | XML | Single plugin state preset | Plugin description + base64 state blob |

In 3.x the companion folder contains only `.wav` sample files. All metadata is consolidated into the single gzipped `.xpj`.

---

## Plugin State Blobs

Effect and instrument parameters are stored as opaque binary blobs that cannot currently be decoded programmatically.

### Encoding by format generation

| Generation | Container | Encoding | Example |
|------------|-----------|----------|---------|
| 2.x (.xfx, .xpl, .xpm) | `<Data>` XML tag | Custom base64 variant prefixed with a byte-count: `116.AMjUSQ...` | `<Data>116.AMjUSQ....PPIIEHDk1bz8lbzk1atA...</Data>` |
| 3.x (.xpj JSON) | `"state"` field | Standard base64 string | `"state": "AMjUSQ..."` |

### The `AMjUS` header pattern

All observed state blobs -- across both format generations and all AIR/Akai effects -- begin with the bytes `AMjUS` followed by a variant character. This is a magic identifier for the plugin state serialization format (likely AIR Music Technology's internal binary format).

| Prefix | Observed in |
|--------|-------------|
| `AMjUSA` | Most simple effects (EQ, compressor, filters, Mother Ducker) |
| `AMjUSI` | AIR Spring Reverb |
| `AMjUSM` | Bus Compressor (with custom settings) |
| `AMjUSQ` | AIR Delay, AIR Distortion |
| `AMjUS4` | AIR Reverb |
| `AMjUSEA` | TubeSynth instrument |

### Decoding status

| Aspect | Status |
|--------|--------|
| Magic header (`AMjUS`) | Identified -- consistent across all observed blobs |
| Parameter value byte offsets | NOT DECODED |
| Float encoding | Suspected IEEE 754 (blob sizes match parameter count x 4 or x 8 bytes) |
| Mother Ducker bus assignment | NOT DECODED (critical for programmatic sidechain routing) |
| Complete reverse engineering | NOT ATTEMPTED |

### Practical guidance

Do not attempt to decode or modify state blobs. When parsing or rebuilding projects, preserve blobs byte-for-byte. Effect metadata (name, preset name, UID, enabled/disabled) is stored in separate JSON fields and is fully accessible. For template generation, use pre-captured blobs at known hardware settings rather than synthesizing blobs programmatically.
