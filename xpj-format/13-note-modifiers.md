# 13 — Note Modifiers

> **XPJ Format** | [Index](README.md) | **Manual:** [MPC Feature Reference](A1-mpc-feature-reference.md)

Every type-3 note event has 16 modifier slots, each with a value (float) and active state (bool). The slot-to-name mapping is defined in `program.drum.modifierPlaybackProperties`.

## Slot-to-Name Mapping

| Slot | Name | Default Value | Active by Default |
| ---- | ---- | ------------- | ----------------- |
| 0 | Tuning (coarse) | 0.5 | **true** |
| 1 | Tuning (fine) | 0.5 | false |
| 2 | Filter | 0.0 | false |
| 3 | Layer | 0.0 | false |
| 4 | Attack | 0.0 | false |
| 5 | Decay | 0.5 | false |
| 6 | Slice/Pad | 1.0 | false |
| 7 | _(unknown)_ | 0.0 | false |
| 8 | _(unknown)_ | 0.0 | false |
| 9 | _(unknown)_ | 0.0 | false |
| 10 | _(unknown)_ | 0.0 | false |
| 11 | _(unknown)_ | 0.0 | false |
| 12 | _(unknown)_ | 0.0 | false |
| 13 | _(unknown)_ | 0.0 | false |
| 14 | _(unknown)_ | 0.0 | false |
| 15 | _(unknown)_ | 0.0 | false |

Source: `modifierPlaybackProperties` array in drum program, confirmed against MPC manual.

## Event Format

Each note event stores modifiers as flat numbered fields:

```json
{
  "note": {
    "note": 39,
    "velocity": 0.842,
    "length": 377,
    "modifierValue0": 0.5,
    "modifierActiveState0": true,
    "modifierValue1": 0.5,
    "modifierActiveState1": false,
    "modifierValue2": 0.0,
    "modifierActiveState2": false,
    // ... slots 3–15
    "EnumCerealisationWrapper(selectedModifierType)": "Tuning (coarse)"
  }
}
```

## Modifier Behavior

Each modifier has two modes (per `modifierPlaybackProperties`):
- **Absolute:** Value replaces the parameter
- **Relative:** Value offsets the parameter (0.5 = no change, <0.5 = decrease, >0.5 = increase)

The value range is 0.0–1.0 regardless of mode.

## Active Modifiers in Data

Across 8,052 note events:

| Slot | Non-Default Values | Active Count | Notes |
| ---- | ------------------ | ------------ | ----- |
| 0 (Tuning coarse) | Common | 7,205 | Most notes have this active |
| 1 (Tuning fine) | Rare | 0 | Value set but never active |
| 5 (Decay) | Rare | 0 | Value set but never active |
| 6 (Slice/Pad) | Common | 0 | Default 1.0, never active |
| 2–4, 7–15 | None | 0 | Always default values |

Slot 0 (Tuning coarse) is the only modifier actively used in existing project data.

## EnumCerealisationWrapper

The `EnumCerealisationWrapper(selectedModifierType)` field stores the last UI-selected modifier type as a human-readable string. Observed values:
- `"Tuning (coarse)"`

This is UI state only — it doesn't affect playback.

## Notes

- The modifier system supports per-note parameter overrides, enabling humanization and per-step sound design.
- Slots 7–15 are structurally present but their names are not confirmed — they may map to additional parameters on newer firmware.
- The `modifierPlaybackProperties` array in the drum program provides the authoritative mapping.
- Despite 16 slots being available, real-world usage is concentrated in slot 0 (Tuning coarse).

Full example: [event-note.json](examples/event-note.json)
