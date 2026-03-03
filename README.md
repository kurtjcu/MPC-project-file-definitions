# MPC Project File Definitions

A knowledge base documenting the Akai MPC `.xpj` project file format (firmware 3.7+).

This project is a work in progress. The documentation has been reverse-engineered from a small corpus of 18 project files and the MPC 3.7 User Guide — it is likely to contain errors, incorrect assumptions, and significant omissions. Field descriptions marked as "UNCONFIRMED" or "LOW" confidence should be treated with particular caution. Contributions and corrections are welcome.

## Documentation

### [XPJ Format Reference](xpj-format/README.md)

Complete documentation of the `.xpj` file structure — track types, program objects, effects, sequences, automation, samples, and more. Derived from analysis of 18 real project files.

### [XPJ Analysis Reports](xpj-analysis/README.md)

Script-generated analysis reports from the 18-file corpus, covering track inventories, effect UIDs, automation parameters, note events, and sample metadata.

## Quick Start

An `.xpj` file is a **gzip-compressed JSON** file with a 5-line text header. To extract:

```python
import gzip, json

with open("project.xpj", "rb") as f:
    raw = gzip.decompress(f.read()).decode("utf-8")

lines = raw.split("\n", 5)
header = lines[:5]   # ACVS, firmware version, object type, encoding, platform
data = json.loads(lines[5])["data"]  # all project content
```

See [00-format-reference.md](xpj-format/00-format-reference.md) for format details, serialization patterns, and known quirks.

## Examples

Visualizations built from parsed `.xpj` project data:

### Drum track with automation
![Drum track with automation](resources/Drum%20track%20with%20automation.png)

A drum track piano roll (8 bars, 184 notes) with two automation lanes — `pad_amp_env_decay` and `pad_mute` — rendered directly from the sequence and automation event data in the `.xpj` file.
