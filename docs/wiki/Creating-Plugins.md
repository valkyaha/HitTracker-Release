# Creating Plugins

This guide explains how to create game plugins for VNoHitTracker.

## Plugin Structure

Each plugin lives in its own folder under `plugins/`:

```
plugins/
└── your_game/
    └── plugin.toml    # Plugin manifest
```

## Plugin Manifest (plugin.toml)

The `plugin.toml` file defines everything about your game plugin.

### Basic Structure

```toml
# Plugin metadata
[plugin]
id = "your_game"                    # Unique identifier (lowercase, no spaces)
name = "Your Game Name"             # Display name
short_name = "YG"                   # Short name for UI (2-4 chars)
version = "1.0.0"
author = "Your Name"
description = "Description of your plugin"
image = "your_game.png"             # Optional: Icon image

# Process detection
[process]
names = ["YourGame.exe", "yourgame.exe"]  # Possible executable names

# Autosplitter configuration
[autosplitter]
enabled = true
algorithm = "ds3"                   # Algorithm to use (see below)

# Boss definitions
[[bosses]]
id = "first_boss"
name = "First Boss"
flag_id = 12345678

# Preset definitions
[[presets]]
id = "all-bosses"
name = "All Bosses"
boss_ids = ["first_boss", "second_boss"]
```

## Available Algorithms

Choose an algorithm based on how your game stores boss defeat flags:

| Algorithm | Games | Description |
|-----------|-------|-------------|
| `ds3` | DS3, Sekiro | Category decomposition with div_10m/div_1k/mod_1000 |
| `eldenring` | Elden Ring | Binary tree traversal for category lookup |
| `ds1` | DS1 Remastered | Offset table lookup with group/area decomposition |
| `ds2` | DS2 SOTFS | Kill counter array (flag_id = array offset) |
| `sekiro` | Sekiro | Similar to DS3 but without category fallback |

## Autosplitter Configuration

### Using a Built-in Algorithm

If your game uses the same flag structure as an existing game:

```toml
[autosplitter]
enabled = true
algorithm = "ds3"    # Use Dark Souls 3 algorithm
```

### Custom Memory Patterns

If you need to specify custom patterns:

```toml
[autosplitter]
enabled = true
algorithm = "ds3"

# Memory patterns for finding pointers
[[autosplitter.patterns]]
name = "sprj_event_flag_man"
pattern = "48 c7 05 ? ? ? ? 00 00 00 00"
rip_offset = 3
instruction_len = 11

[[autosplitter.patterns]]
name = "field_area"
pattern = "4c 8b 3d ? ? ? ? 8b 45 87"
rip_offset = 3
instruction_len = 7
fallback_patterns = ["48 8B 1D ? ? ? ? 48 8B F8"]  # Alternative pattern

# Pointer chains
[[autosplitter.pointer_chains]]
name = "event_flags"
offsets = [0x218]
```

### Pattern Syntax

- Patterns are space-separated hex bytes
- Use `?` for wildcard bytes (match any value)
- `rip_offset`: Position of the 32-bit offset in the instruction
- `instruction_len`: Total length of the instruction for RIP resolution

## Boss Definitions

Each boss needs a unique definition:

```toml
[[bosses]]
id = "boss_internal_id"           # Unique ID (used in presets)
name = "Boss Display Name"        # Shown in UI
location = "Area Name"            # Optional: Where boss is located
flag_id = 12345678               # Event flag ID or kill counter offset
required = true                   # Is this boss required for completion?
is_dlc = false                    # Is this DLC content?
dlc_name = "DLC Name"            # Name of DLC (if is_dlc = true)
order = 1                         # Sort order
aliases = ["Alt Name", "Other"]   # Optional: Alternative names
```

### Finding Flag IDs

Flag IDs vary by game:

- **DS3/Sekiro**: 8-digit event flags (e.g., `14000800`)
- **Elden Ring**: Event flags with category/remainder (e.g., `10000800`)
- **DS1**: Mix of small values (0-99) and 8-digit flags
- **DS2**: Array offsets (e.g., `124`, `112`)

Use Cheat Engine or similar tools to find flag IDs. See [Flag Algorithms](Flag-Algorithms.md) for details.

## Preset Definitions

Presets define groups of bosses for different run types:

```toml
[[presets]]
id = "all-bosses"
name = "All Bosses"
description = "All main game bosses"
boss_ids = [
    "boss1", "boss2", "boss3",
    "boss4", "boss5"
]

[[presets]]
id = "any-percent"
name = "Any%"
description = "Required bosses only"
boss_ids = ["boss1", "boss3", "boss5"]

[[presets]]
id = "all-bosses-dlc"
name = "All Bosses + DLC"
description = "All bosses including DLC"
boss_ids = [
    "boss1", "boss2", "boss3",
    "boss4", "boss5",
    "dlc_boss1", "dlc_boss2"
]
```

## Complete Example

Here's a complete plugin for a hypothetical game:

```toml
[plugin]
id = "example_game"
name = "Example Souls-like"
short_name = "EXG"
version = "1.0.0"
author = "Plugin Author"
description = "Support for Example Souls-like game"

[process]
names = ["ExampleGame.exe"]

[autosplitter]
enabled = true
algorithm = "ds3"

[[autosplitter.patterns]]
name = "sprj_event_flag_man"
pattern = "48 c7 05 ? ? ? ? 00 00 00 00 48 8b 7c 24 38"
rip_offset = 3
instruction_len = 11

[[autosplitter.pointer_chains]]
name = "event_flags"
offsets = [0x218]

# Main game bosses
[[bosses]]
id = "tutorial_boss"
name = "Tutorial Boss"
location = "Starting Area"
flag_id = 10000800
required = true
order = 1

[[bosses]]
id = "first_lord"
name = "First Lord"
location = "Main Castle"
flag_id = 11000800
required = true
order = 2

[[bosses]]
id = "optional_boss"
name = "Hidden Boss"
location = "Secret Area"
flag_id = 12000800
required = false
order = 3

[[bosses]]
id = "final_boss"
name = "Final Boss"
location = "End Zone"
flag_id = 19000800
required = true
order = 4

# DLC Boss
[[bosses]]
id = "dlc_boss"
name = "DLC Boss"
location = "DLC Area"
flag_id = 20000800
required = true
is_dlc = true
dlc_name = "Example DLC"
order = 5

# Presets
[[presets]]
id = "all-bosses"
name = "All Bosses"
description = "All main game bosses"
boss_ids = ["tutorial_boss", "first_lord", "optional_boss", "final_boss"]

[[presets]]
id = "any-percent"
name = "Any%"
description = "Required bosses only"
boss_ids = ["tutorial_boss", "first_lord", "final_boss"]

[[presets]]
id = "all-bosses-dlc"
name = "All Bosses + DLC"
description = "All bosses including DLC"
boss_ids = ["tutorial_boss", "first_lord", "optional_boss", "final_boss", "dlc_boss"]
```

## Testing Your Plugin

1. Place your plugin folder in the `plugins/` directory
2. Restart VNoHitTracker
3. Check the logs for any parsing errors
4. Your game should appear in the game selection dropdown
5. Test the autosplitter by defeating a boss
