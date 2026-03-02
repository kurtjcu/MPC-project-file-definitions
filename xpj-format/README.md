# XPJ Project File Format Reference

The `.xpj` file is the MPC Standalone OS project format (firmware 3.7+). This knowledge base documents the complete structure derived from analysis of 18 real project files.

## File Format

An `.xpj` file is **gzip-compressed text** with this structure:

```
Line 1: "ACVS" (magic header)
Line 2: firmware version (e.g., "3.7.0.56")
Line 3: "SerialisableProjectData"
Line 4: "json"
Line 5: "Linux"
Line 6+: JSON body
```

The JSON body has two top-level keys:

```json
{
  "formatVersion": 2,
  "data": { /* entire project state */ }
}
```

All project content lives under `data`.

## Key Constants

| Constant | Value | Notes |
| -------- | ----- | ----- |
| Schema version | 28 | `data.version` |
| PPQ (pulses per quarter) | 960 | All timing in pulse units |
| Pad count | 128 | `instruments[]` always has 128 slots |
| Layer count | 8 | `layersv[]` always has 8 slots per pad |
| Max sends | 4 | `sends[4]` on every mixable |
| Song slots | 32 | `songs[]` always 32 entries (always empty) |
| Locators | 6 | Per sequence |
| Product ID | `ACVB` | `lastSavedProductIdentifier` |

## Sections

| # | Section | Description |
| - | ------- | ----------- |
| 00 | [Format Reference](00-format-reference.md) | Serialization patterns, known quirks, 2.x vs 3.x detection |
| 01 | [Project Structure](01-project-structure.md) | Top-level `data{}` keys, metadata, global settings, UI state |
| 02 | [Track Types](02-track-types.md) | Program type table, shared track/program fields, MIDI/CV tracks |
| 03 | [Drum Program](03-drum-program.md) | `program.drum`: instruments, layers, synth section, pad effects |
| 04 | [Keygroup Program](04-keygroup-program.md) | `program.keygroup`: zones, key ranges, shared drum infrastructure |
| 05 | [Plugin Program](05-plugin-program.md) | `program.plugin`: 7 confirmed instrument UIDs |
| 06 | [Audio Program](06-audio-program.md) | `program.audio`: input source, type-4 audio events |
| 07 | [Mixer & Routing](07-mixer-routing.md) | Mixable structure, audioRoute, sends, inserts, Mother Ducker |
| 08 | [Effects Catalog](08-effects-catalog.md) | 103 confirmed effect UIDs, plugin description schema |
| 09 | [Sequences & Events](09-sequences-events.md) | Sequence properties, clip matrix, 960 PPQ timing |
| 10 | [Event Types](10-event-types.md) | Event type 1/2/3/4 with full field schemas |
| 11 | [Automation](11-automation.md) | 80+ confirmed parameter IDs, address space layout |
| 12 | [Samples](12-samples.md) | Sample pool structure, metadata, layer references |
| 13 | [Note Modifiers](13-note-modifiers.md) | 16 modifier slots, slot-to-name mapping, active states |
| 14 | [Unknown Fields](14-unknown-fields.md) | ~120 undocumented fields, constants, known corrections |

## Appendices

| # | Section | Description |
| - | ------- | ----------- |
| A1 | [MPC Feature Reference](A1-mpc-feature-reference.md) | What MPC features do (parameter ranges, UI behavior, signal flow) |
| A2 | [OPx-4 Parameters](A2-opx4-parameters.md) | AIR OPx-4 FM synth internal parameter reference |

## Full Examples

Complete real JSON objects from actual projects are in [examples/](examples/):

| File | Description | Source |
| ---- | ----------- | ------ |
| [track-drum.json](examples/track-drum.json) | Type-0 drum track | hiphop-demo |
| [track-keygroup.json](examples/track-keygroup.json) | Type-1 keygroup track | clip-demo |
| [track-plugin.json](examples/track-plugin.json) | Type-3 plugin track | ducker-sub |
| [track-midi.json](examples/track-midi.json) | Type-4 MIDI track | midi-cv-test |
| [track-type4.json](examples/track-type4.json) | Type-4 MIDI track (alternate) | — |
| [track-cv-melodic.json](examples/track-cv-melodic.json) | Type-5 CV track (Melodic mode) | midi-cv-test |
| [track-cv-drum.json](examples/track-cv-drum.json) | Type-5 CV track (Drum mode) | midi-cv-test |
| [track-audio.json](examples/track-audio.json) | Type-6 audio track | capsun |
| [track-return.json](examples/track-return.json) | Type-7 return track | ducker-sub |
| [instrument-drum.json](examples/instrument-drum.json) | Full drum instrument | hiphop-demo |
| [instrument-keygroup.json](examples/instrument-keygroup.json) | Keygroup instrument with zones | clip-demo |
| [sequence.json](examples/sequence.json) | Sequence with events | hiphop-demo |
| [event-note.json](examples/event-note.json) | Type-3 note event | hiphop-demo |
| [event-automation.json](examples/event-automation.json) | Type-1/2 automation events | ducker-sub |
| [event-audio.json](examples/event-audio.json) | Type-4 audio event | capsun |

## Data Sources

- **18 extracted project files** from Akai factory demos and test projects
- Analysis output in [../xpj-analysis/](../xpj-analysis/)
