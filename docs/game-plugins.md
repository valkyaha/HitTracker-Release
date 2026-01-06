# Game Plugins (TOML)

Game plugins define support for specific games including boss lists, presets, and autosplitter configuration. These are static configuration files that don't execute code.

## Overview

TOML plugins provide:
- Boss/split definitions
- Speedrun category presets
- Autosplitter configuration
- Save location paths
- Recommended tools

## Plugin Structure

```
plugins/
└── game-id/
    └── plugin.toml
```

## Plugin Manifest

### Basic Information

```toml
[plugin]
id = "dark-souls-3"
name = "Dark Souls III"
version = "1.0.0"
author = "NYA Core Team"
description = "Support for Dark Souls III"
```

### Process Detection

```toml
[process]
names = ["DarkSoulsIII.exe", "darksoulsiii.exe"]
```

### Save Location

```toml
[save_location]
windows = "%APPDATA%/DarkSoulsIII"
linux = "~/.local/share/Steam/userdata"
```

### Autosplitter Configuration

```toml
[autosplitter]
enabled = true
algorithm = "category_decomposition"  # or "binary_tree", "offset_table", "kill_counter"

# Memory patterns to find base addresses
[[autosplitter.patterns]]
name = "event_flags"
pattern = "48 8B 0D ?? ?? ?? ?? 48 85 C9 74 ?? 41 B8 ?? ?? ?? ?? E8"
offset = 3
relative = true

# Algorithm-specific configuration
[autosplitter.config]
base_offset = 0
category_size = 0x180
entry_size = 4
```

### Boss Definitions

```toml
[[bosses]]
id = "iudex_gundyr"
name = "Iudex Gundyr"
location = "Cemetery of Ash"
flag_id = 13000800
required = true        # Required for All Bosses
dlc = false

[[bosses]]
id = "champion_gundyr"
name = "Champion Gundyr"
location = "Untended Graves"
flag_id = 14000800
required = true
dlc = false

[[bosses]]
id = "sister_friede"
name = "Sister Friede"
location = "Painted World of Ariandel"
flag_id = 45000800
required = false
dlc = true
dlc_name = "Ashes of Ariandel"
```

### Presets

```toml
[[presets]]
id = "any_percent"
name = "Any%"
description = "Beat the game as fast as possible"
bosses = ["iudex_gundyr", "vordt", "dancer", "dragonslayer", "princes", "soul_of_cinder"]

[[presets]]
id = "all_bosses"
name = "All Bosses"
description = "Defeat all required bosses"
include_required = true
include_dlc = false

[[presets]]
id = "all_bosses_dlc"
name = "All Bosses + DLC"
description = "Defeat all bosses including DLC"
include_required = true
include_dlc = true
```

### Bonfires/Checkpoints

```toml
[[bonfires]]
id = "firelink_shrine"
name = "Firelink Shrine"
area = "Firelink Shrine"

[[bonfires]]
id = "high_wall"
name = "High Wall of Lothric"
area = "High Wall of Lothric"
```

### Tools

```toml
[[tools]]
name = "DS3 Practice Tool"
url = "https://github.com/..."
description = "Practice tool for Dark Souls III"

[[tools]]
name = "Gadget"
url = "https://github.com/..."
description = "Memory viewer"
```

## Autosplitter Algorithms

### Category Decomposition (DS3, Sekiro)

Used for games that store flags in categorized memory blocks.

```toml
[autosplitter]
algorithm = "category_decomposition"

[autosplitter.config]
base_offset = 0
category_size = 0x180
entry_size = 4
divisor_10m = 10000000
divisor_1k = 1000
```

**How it works:**
1. Flag ID is decomposed: `div_10m`, `div_1k`, `mod_1000`
2. Category offset = `div_10m * category_size`
3. Bit index calculated from remainder
4. Bit checked at calculated address

### Binary Tree (Elden Ring, AC6)

Used for games with tree-structured flag storage.

```toml
[autosplitter]
algorithm = "binary_tree"

[autosplitter.config]
divisor = 10000
left_offset = 0x8
right_offset = 0x10
data_offset = 0x18
```

**How it works:**
1. Category = `flag_id / divisor`
2. Traverse binary tree to find category node
3. Check bit at `flag_id % divisor`

### Offset Table (DS1 Remastered)

Used for games with direct offset tables.

```toml
[autosplitter]
algorithm = "offset_table"

[autosplitter.config]
group_offset = 0
area_offset = 0x4
section_offset = 0x8
```

**How it works:**
1. Flag ID decomposed into group/area/section/number
2. Direct lookup in offset table
3. Check bit at calculated position

### Kill Counter (DS2 SOTFS)

Used for games that track kill counts.

```toml
[autosplitter]
algorithm = "kill_counter"

[autosplitter.config]
counter_offset = 0x8
```

**How it works:**
1. Flag ID = byte offset in counter array
2. Boss defeated if counter > 0

## Memory Patterns

Patterns use hex bytes with `?` wildcards:

```toml
[[autosplitter.patterns]]
name = "world_chr_man"
pattern = "48 8B 05 ?? ?? ?? ?? 48 85 C0 74 0F 48 39 88"
offset = 3
relative = true

[[autosplitter.patterns]]
name = "event_flags_alt"
pattern = "48 8B 3D ?? ?? ?? ?? 48 85 FF 0F 84"
offset = 3
relative = true
```

- `??` = wildcard byte (matches any value)
- `offset` = bytes after pattern start to find address
- `relative` = address is RIP-relative (needs calculation)

## Pointer Chains

For multi-level pointer dereferencing:

```toml
[[autosplitter.pointers]]
name = "event_manager"
base_pattern = "event_flags"
offsets = [0x0, 0x28, 0x10]
```

## Example: Complete Dark Souls III Plugin

```toml
[plugin]
id = "dark-souls-3"
name = "Dark Souls III"
version = "2.1.0"
author = "NYA Core Team"
description = "Full autosplitter support for Dark Souls III"

[process]
names = ["DarkSoulsIII.exe"]

[save_location]
windows = "%APPDATA%/DarkSoulsIII"

[autosplitter]
enabled = true
algorithm = "category_decomposition"

[[autosplitter.patterns]]
name = "event_flags"
pattern = "48 8B 0D ?? ?? ?? ?? 48 85 C9 74 ?? 41 B8 ?? ?? ?? ?? E8"
offset = 3
relative = true

[autosplitter.config]
base_offset = 0
category_size = 0x180
entry_size = 4

# Main Game Bosses
[[bosses]]
id = "iudex_gundyr"
name = "Iudex Gundyr"
location = "Cemetery of Ash"
flag_id = 13000800
required = true
dlc = false

[[bosses]]
id = "vordt"
name = "Vordt of the Boreal Valley"
location = "High Wall of Lothric"
flag_id = 13100800
required = true
dlc = false

# ... more bosses ...

# DLC Bosses
[[bosses]]
id = "sister_friede"
name = "Sister Friede"
location = "Ariandel Chapel"
flag_id = 45000800
required = false
dlc = true
dlc_name = "Ashes of Ariandel"

# Presets
[[presets]]
id = "any_percent"
name = "Any%"
bosses = ["iudex_gundyr", "vordt", "dancer", "dragonslayer", "princes", "soul_of_cinder"]

[[presets]]
id = "all_bosses"
name = "All Bosses"
include_required = true
include_dlc = false

[[presets]]
id = "all_bosses_dlc"
name = "All Bosses + DLC"
include_required = true
include_dlc = true

# Key Bonfires
[[bonfires]]
id = "firelink"
name = "Firelink Shrine"
area = "Firelink Shrine"

[[bonfires]]
id = "high_wall"
name = "High Wall of Lothric"
area = "High Wall"

# Recommended Tools
[[tools]]
name = "DS3 Practice Tool"
url = "https://github.com/Jerem584/ds3-practice-tool"
description = "Practice tool with teleport and more"
```

## Plugin Auto-Update

Plugins are automatically updated from the [NYA-Core-Assets](https://github.com/valkyaha/NYA-Core-Assets) repository:

1. App checks for updates on startup
2. Compares versions with manifest
3. Downloads updated files
4. Extracts to plugins directory
5. Reloads plugin manager

## Creating a Custom Plugin

1. Create a folder in `plugins/` with your game ID
2. Create `plugin.toml` with boss definitions
3. Add autosplitter configuration if supported
4. Test with your game
5. Submit to NYA-Core-Assets repository

## Best Practices

1. **Unique IDs** - Use lowercase with hyphens
2. **Accurate flags** - Test flag IDs thoroughly
3. **Multiple patterns** - Include fallback patterns for game versions
4. **Clear descriptions** - Help users understand presets
5. **Complete boss lists** - Include all bosses for the game
