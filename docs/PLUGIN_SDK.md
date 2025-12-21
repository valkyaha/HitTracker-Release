# NYA Core Plugin SDK

Create custom game plugins for NYA Core with optional autosplitter support.

## Overview

The NYA Core Plugin SDK allows you to:
- Add support for new games
- Define boss lists and presets
- Implement autosplitter functionality (automatic boss defeat detection)

## Plugin Structure

Plugins are TOML configuration files that define game metadata, bosses, presets, and optionally autosplitter patterns.

### Basic Plugin

```toml
[plugin]
id = "mygame"
name = "My Game"
short_name = "MG"
version = "1.0.0"
author = "Your Name"
description = "Support for My Game"

[process]
names = ["MyGame.exe"]

# Boss definitions
[[bosses]]
id = "boss1"
name = "First Boss"
location = "Area 1"
required = true
order = 1

[[bosses]]
id = "boss2"
name = "Second Boss"
location = "Area 2"
required = true
order = 2

[[bosses]]
id = "boss3"
name = "Final Boss"
location = "Final Area"
required = true
order = 3

# Preset definitions
[[presets]]
id = "all-bosses"
name = "All Bosses"
description = "All 3 bosses"
boss_ids = ["boss1", "boss2", "boss3"]
```

## Boss Definition

Each boss entry supports:

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `id` | string | Yes | Unique identifier |
| `name` | string | Yes | Display name |
| `location` | string | No | In-game location |
| `required` | bool | No | Required for any% |
| `is_dlc` | bool | No | DLC content |
| `dlc_name` | string | No | DLC name |
| `flag_id` | u32 | No* | Memory flag ID (*required for autosplitter) |
| `order` | u32 | No | Sort order |
| `aliases` | array | No | Alternative names |

## Autosplitter Support

To add automatic boss defeat detection:

```toml
[autosplitter]
enabled = true
algorithm = "category_decomposition"  # Choose algorithm

# Memory patterns for finding pointers
[[autosplitter.patterns]]
name = "sprj_event_flag_man"
pattern = "48 c7 05 ? ? ? ? 00 00 00 00"
rip_offset = 3
instruction_len = 11

# Bosses with flag IDs
[[bosses]]
id = "first_boss"
name = "First Boss"
flag_id = 14000800  # Memory flag for defeat detection
```

### Available Algorithms

| Algorithm | Games | Description |
|-----------|-------|-------------|
| `category_decomposition` | DS3, Sekiro | Category-based flag storage |
| `binary_tree` | Elden Ring, AC6 | Binary tree traversal |
| `offset_table` | DS1 Remastered | Offset table lookup |
| `kill_counter` | DS2 SOTFS | Kill counter array |

### Algorithm Configuration

#### Category Decomposition
```toml
[autosplitter.category_config]
primary_pattern = "sprj_event_flag_man"
secondary_pattern = "field_area"
base_offset = 0x218
entry_size = 0x18
category_count = 6
category_multiplier = 0xa8
```

#### Binary Tree
```toml
[autosplitter.tree_config]
primary_pattern = "virtual_memory_flag"
divisor_offset = 0x1c
tree_root_offset = 0x38
multiplier_offset = 0x20
base_addr_offset = 0x28
```

#### Offset Table
```toml
[autosplitter.offset_table_config]
primary_pattern = "event_flags"

[autosplitter.offset_table_config.group_offsets]
"0" = 0x00000
"1" = 0x00500
```

#### Kill Counter
```toml
[autosplitter.kill_counter_config]
primary_pattern = "game_manager_imp"
chain_offsets = [0xD0, 0x60, 0x8]
```

## Memory Pattern Format

Patterns use hex bytes with `?` wildcards:

```toml
[[autosplitter.patterns]]
name = "my_pattern"
pattern = "48 8B 0D ? ? ? ? 48 85 C9"
rip_offset = 3      # Offset to displacement in instruction
instruction_len = 7  # Total instruction length
```

### RIP-Relative Resolution

For x64 instructions like `mov rcx, [rip+0x12345678]`:

```
48 8B 0D 78 56 34 12
       ^^ rip_offset = 3
          instruction_len = 7

Final address = instruction_addr + instruction_len + displacement
```

## Pointer Chains

For traversing pointer structures:

```toml
[[autosplitter.pointer_chains]]
name = "player_hp"
base_pattern = "world_chr_man"
offsets = [0x80, 0x68, 0x0, 0x3E8]
```

## Plugin Installation

1. Create your plugin folder in `plugins/`
2. Add your `plugin.toml`
3. Restart NYA Core
4. Your game will appear in the Games list

## Complete Example

```toml
[plugin]
id = "hollow_knight"
name = "Hollow Knight"
short_name = "HK"
version = "1.0.0"
author = "Community"
description = "Hollow Knight boss tracker"

# No autosplitter - manual splits only

[[bosses]]
id = "false_knight"
name = "False Knight"
location = "Forgotten Crossroads"
required = true
order = 1

[[bosses]]
id = "hornet_1"
name = "Hornet (Greenpath)"
location = "Greenpath"
required = true
order = 2

[[bosses]]
id = "soul_master"
name = "Soul Master"
location = "City of Tears"
required = false
order = 3

[[presets]]
id = "main-bosses"
name = "Main Bosses"
description = "Required story bosses"
boss_ids = ["false_knight", "hornet_1"]

[[presets]]
id = "all-bosses"
name = "All Bosses"
boss_ids = ["false_knight", "hornet_1", "soul_master"]
```

## Rust SDK Types (for advanced users)

If building Rust plugins:

```rust
use hitcounter_sdk::{
    Boss, PatternConfig, MemoryContext,
    FlagReader, PluginError
};

// Boss definition
pub struct Boss {
    pub id: String,
    pub name: String,
    pub flag_id: Option<u32>,
    pub required: bool,
    pub is_dlc: bool,
}

// Pattern configuration
pub struct PatternConfig {
    pub name: String,
    pub pattern: String,
    pub rip_offset: u32,
    pub instruction_len: u32,
    pub fallback_patterns: Vec<FallbackPattern>,
}

// Flag reader trait
pub trait FlagReader: Send + Sync {
    fn algorithm_name(&self) -> &'static str;
    fn is_flag_set(&self, ctx: &MemoryContext, flag_id: u32) -> bool;
    fn get_defeated_flags(&self, ctx: &MemoryContext, flag_ids: &[u32]) -> Vec<u32>;
}
```

## Support

For questions or issues, open a GitHub issue.
