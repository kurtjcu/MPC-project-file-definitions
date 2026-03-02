# 05 — Plugin Program

> **XPJ Format** | [Index](README.md) | **Manual:** [MPC Feature Reference](A1-mpc-feature-reference.md) | [OPx-4 Parameters](A2-opx4-parameters.md)

Plugin tracks (type 3) host software instruments. Only 1 plugin track has been observed across all 18 analyzed files.

## Schema

```json
{
  "program": {
    "type": 3,
    "plugin": {
      "plugin": {
        "version": 1,
        "description": {
          "UID": 1114860654,
          "name": "Bassline",
          "manufacturer": "",
          "fileOrIdentifier": "MPC:Bassline",
          "category": 1,
          "isInstrument": true,
          "numInputChannels": 0,
          "numOutputChannels": 2,
          "files": []
        },
        "state": "<base64-encoded blob>",
        "presets": [ /* preset list */ ]
      }
    }
  }
}
```

## Plugin Description Fields

| Field | Type | Description |
| ----- | ---- | ----------- |
| `UID` | int | Unique identifier for the plugin |
| `name` | string | Display name |
| `manufacturer` | string | Plugin manufacturer (often empty for built-in) |
| `fileOrIdentifier` | string | Internal identifier (e.g., `"MPC:Bassline"`) |
| `category` | int | Plugin category (1 = instrument) |
| `isInstrument` | bool | True for instruments, false for effects |
| `numInputChannels` | int | Audio inputs (0 for instruments) |
| `numOutputChannels` | int | Audio outputs (2 = stereo) |
| `files` | array | Associated file list (empty for built-in) |

## Known Plugin Instruments

| UID | Name | fileOrIdentifier | Status |
| --- | ---- | ---------------- | ------ |
| 1114860654 | Bassline | `MPC:Bassline` | CONFIRMED (observed in corpus) |
| 1164731747 | Electric | `MPC:Electric` | CONFIRMED (from research registry) |
| 1299534713 | DrumSynth Multi | `MPC:DrumSynth:Multi` | CONFIRMED (from research registry) |
| 1329874009 | Odyssey | `MPC:Odyssey` | CONFIRMED (from research registry) |
| 1330673716 | OPx-4 | `MPC:OPx-4` | CONFIRMED (from research — see [A2-opx4-parameters.md](A2-opx4-parameters.md)) |
| 1399673721 | Hype | `MPC:Hype` | CONFIRMED (from research registry) |
| 1415730041 | TubeSynth | `MPC:TubeSynth` | CONFIRMED (from research registry) |
| — | Mellotron | `MPC:Mellotron` | UNCONFIRMED |
| — | Solina | `MPC:Solina` | UNCONFIRMED |
| — | Stage EP | `MPC:Stage EP` | UNCONFIRMED |

## Notes

- The plugin description is double-nested: `program.plugin.plugin.description` (not `program.plugin.description`).
- Plugin state is an opaque base64-encoded blob — it cannot be meaningfully parsed or generated without the plugin itself. See [00-format-reference.md](00-format-reference.md) §Plugin State Blobs.
- Effect plugins use the same `description` schema but appear in `mixable.inserts.effects[]` rather than in `program.plugin`. See [08-effects-catalog.md](08-effects-catalog.md).

Full example: [track-plugin.json](examples/track-plugin.json)
