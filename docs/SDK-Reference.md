# SDK Reference

VNoHitTracker uses a TOML-based plugin system. Plugins are loaded from the `plugins/` directory next to the executable.

## Plugin Structure

Each plugin is a folder containing a `plugin.toml` file:

```
plugins/
  ds3/
    plugin.toml
  eldenring/
    plugin.toml
  my_custom_game/
    plugin.toml
```

## Plugin Manifest (plugin.toml)

### Basic Structure

```toml
[plugin]
id = "my_game"
name = "My Game Name"
short_name = "MG"
version = "1.0.0"
author = "Your Name"
description = "Description of the plugin"

[process]
names = ["MyGame.exe"]

[autosplitter]
enabled = true
algorithm = "category_decomposition"  # or "binary_tree", "offset_table", "kill_counter"

# Algorithm-specific configuration goes here

# Memory patterns
[[autosplitter.patterns]]
name = "pattern_name"
pattern = "48 8B 0D ? ? ? ?"
rip_offset = 3
instruction_len = 7

# Boss definitions
[[bosses]]
id = "first_boss"
name = "First Boss"
flag_id = 12345678

# Presets
[[presets]]
id = "all-bosses"
name = "All Bosses"
boss_ids = ["first_boss", "second_boss"]
```

## Plugin Metadata

```toml
[plugin]
id = "unique_id"           # Unique identifier (lowercase, no spaces)
name = "Display Name"      # Full game name
short_name = "ABBR"        # Short abbreviation (optional)
version = "1.0.0"          # Plugin version
author = "Author Name"     # Plugin author
description = "..."        # Brief description
```

## Process Detection

```toml
[process]
names = ["Game.exe", "Game_Alt.exe"]  # Process names to detect
```

## Autosplitter Algorithms

VNoHitTracker supports four flag reading algorithms based on how different games store boss defeat flags:

### 1. Category Decomposition (DS3/Sekiro)

Used for games that store flags in categorized memory structures.

```toml
[autosplitter]
enabled = true
algorithm = "category_decomposition"

[autosplitter.category_config]
primary_pattern = "sprj_event_flag_man"
secondary_pattern = "field_area"        # Optional, for world area flags
base_offset = 0x218                     # Offset to category table
entry_size = 0x18                       # Size of each category entry
category_count = 6                      # Number of categories (DS3: 6, Sekiro: 1)
category_multiplier = 0xa8              # Category multiplier in address calc
field_area_base_offset = 0x0            # First offset from FieldArea
world_info_offset = 0x10                # Offset to world info (DS3: 0x10, Sekiro: 0x18)
world_info_struct_size = 0x38           # Size of worldInfo struct
world_block_struct_size = 0x70          # Size of worldBlockInfo (DS3: 0x70, Sekiro: 0xb0)
```

### 2. Binary Tree (Elden Ring/Armored Core 6)

Used for games that store flags in a binary tree structure.

```toml
[autosplitter]
enabled = true
algorithm = "binary_tree"

[autosplitter.tree_config]
primary_pattern = "virtual_memory_flag"
pointer_chain = [0x0, 0x28]             # Offsets after initial resolution
divisor_offset = 0x1c                   # Offset to divisor value
tree_root_offset = 0x38                 # Offset to tree root pointer
multiplier_offset = 0x20                # Offset to multiplier value
base_addr_offset = 0x28                 # Offset to base address value
```

### 3. Offset Table (Dark Souls Remastered)

Used for games with static offset tables for flag groups.

```toml
[autosplitter]
enabled = true
algorithm = "offset_table"

[autosplitter.offset_table_config]
primary_pattern = "event_flags"

[autosplitter.offset_table_config.group_offsets]
"0" = 0x00000
"1" = 0x00500
"5" = 0x05F00
"6" = 0x06100
"7" = 0x06300

[autosplitter.offset_table_config.area_indices]
"000" = 0
"100" = 1
"101" = 2
# ... more area mappings
```

### 4. Kill Counter (Dark Souls 2)

Used for games that track boss defeats via kill counters.

```toml
[autosplitter]
enabled = true
algorithm = "kill_counter"

[autosplitter.kill_counter_config]
primary_pattern = "game_manager_imp"
chain_offsets = [0xD0, 0x60, 0x8]       # Pointer chain to kill counter array
```

## Memory Patterns

Patterns are byte sequences used to find game data structures in memory.

```toml
[[autosplitter.patterns]]
name = "pattern_name"                   # Identifier for this pattern
pattern = "48 c7 05 ? ? ? ? 00 00"      # Byte pattern (? = wildcard)
rip_offset = 3                          # Position of RIP-relative offset
instruction_len = 11                    # Total instruction length
fallback_patterns = [                   # Alternative patterns (optional)
    "48 8b 0d ? ? ? ?",
]
```

### Pattern Syntax

- Hex bytes: `48 c7 05`
- Wildcards: `?` or `??` (matches any byte)
- Example: `"48 c7 05 ? ? ? ? 00 00 00 00"` matches instructions like `mov qword ptr [rip+offset], 0`

## Pointer Chains

Named pointer chains for memory traversal:

```toml
[[autosplitter.pointer_chains]]
name = "event_flags"
offsets = [0x10, 0x28, 0x0]
```

## Boss Definitions

```toml
[[bosses]]
id = "boss_unique_id"                   # Unique identifier
name = "Boss Display Name"              # Name shown in UI
location = "Area Name"                  # Where the boss is (optional)
required = true                         # Required for any% (optional)
is_dlc = false                          # DLC boss (optional)
dlc_name = "DLC Name"                   # DLC name if is_dlc (optional)
flag_id = 12345678                      # Memory flag ID for defeat
order = 1                               # Display order (optional)
aliases = ["Nickname", "Alt Name"]      # Search aliases (optional)
```

## Presets

Presets define boss lists for different run categories:

```toml
[[presets]]
id = "all-bosses"
name = "All Bosses"
description = "All main game bosses"
boss_ids = [
    "first_boss",
    "second_boss",
    "third_boss"
]
```

## Complete Example

See the built-in plugins for complete examples:
- `plugins/ds3/plugin.toml` - Category decomposition example
- `plugins/eldenring/plugin.toml` - Binary tree example
- `plugins/ds1/plugin.toml` - Offset table example
- `plugins/ds2/plugin.toml` - Kill counter example

## Finding Flag IDs

Flag IDs for boss defeats can be found using:
1. Cheat Engine with game-specific tables
2. Existing autosplitter projects (LiveSplit, SoulSplitter)
3. Game modding communities and wikis

## Testing Your Plugin

1. Create your plugin folder in `plugins/`
2. Add your `plugin.toml`
3. Launch VNoHitTracker
4. Select your game from the Games view
5. Check the autosplitter status in the Autosplitter view
