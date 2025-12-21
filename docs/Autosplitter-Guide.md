# Autosplitter Guide

This guide explains how the NYA Core autosplitter works and how to configure it for new games.

## Overview

The autosplitter automatically detects boss defeats by reading the game's memory. When a boss is defeated, the game sets an "event flag" in memory. The autosplitter monitors these flags and advances splits automatically.

## How It Works

1. **Process Detection** - Find the game process by executable name
2. **Pattern Scanning** - Search memory for known byte patterns to find data structures
3. **Pointer Resolution** - Resolve RIP-relative addresses to find the actual data
4. **Flag Reading** - Periodically check if boss defeat flags are set
5. **Split Advancement** - Advance to next split when a boss is defeated

## Pattern Scanning

### What Are Patterns?

Patterns are sequences of bytes that uniquely identify locations in the game's executable. They remain consistent across game versions and can be found using tools like Cheat Engine.

### Pattern Format

```
48 c7 05 ? ? ? ? 00 00 00 00
```

- Hex bytes: Must match exactly (`48`, `c7`, `05`)
- Wildcards: `?` matches any byte (used for variable data like addresses)

### RIP-Relative Addressing

Modern x64 code uses RIP-relative addressing. The pattern points to an instruction, and we need to calculate the actual address:

```
Instruction: 48 c7 05 XX XX XX XX 00 00 00 00
Position:    0  1  2  3  4  5  6  7  8  9  10
             ^        ^              ^
             Start    Offset (4 bytes) End

Resolved Address = InstructionAddr + InstructionLen + SignedOffset
```

Configuration:
- `rip_offset`: Position of the 4-byte offset (e.g., `3`)
- `instruction_len`: Total instruction length (e.g., `11`)

## Memory Structures

### FromSoftware Event Flag System

Most FromSoftware games use a similar event flag system:

1. **EventFlagMan/VirtualMemoryFlag** - Main manager class
2. **Category Tables** - Organized by flag ID ranges
3. **Bit Arrays** - Individual flags stored as bits

### Flag ID Structure

#### Dark Souls 3 / Sekiro
8-digit flag IDs decomposed into:
- `div_10m` = (flag_id / 10,000,000) % 10
- `div_1k` = (flag_id / 1,000) % 10
- `mod_1000` = flag_id % 1000

Example: Flag `14000800`
- div_10m = 1, div_1k = 0, mod_1000 = 800
- Bit offset = (800 >> 5) * 4 = 100
- Bit index = 31 - (800 & 31) = 31 - 0 = 31

#### Elden Ring
Uses a binary tree structure:
- `category` = flag_id / divisor
- `remainder` = flag_id % divisor
- Tree traversal to find category node
- Bit extraction from data pointer

#### Dark Souls Remastered
Offset table lookup:
- 8-digit flag decomposed into group/area/section/number
- Group offset (0-7) + Area index + Section offset + Number offset

#### Dark Souls 2
Kill counter array:
- `flag_id` is the byte offset into the counter array
- Boss defeated if counter > 0

## Finding Flag IDs

### Using Cheat Engine

1. Defeat a boss
2. Search for value changes (boolean 0→1 or counter 0→1)
3. Find the flag address
4. Reverse engineer the flag ID from the address

### From Existing Tools

Check existing autosplitters and modding communities:
- SoulSplitter source code
- Speed Souls community
- Game modding wikis

### Common Flag ID Ranges

| Game | Main Bosses | DLC Bosses |
|------|-------------|------------|
| DS3 | 13xxxxxx, 14xxxxxx | 145xxxxx, 15xxxxxx |
| ER | 1xxxxxxx | 2xxxxxxx |
| Sekiro | 9xxx (counters) | - |

## Configuration Examples

### Dark Souls 3

```toml
[autosplitter]
enabled = true
algorithm = "category_decomposition"

[[autosplitter.patterns]]
name = "sprj_event_flag_man"
pattern = "48 c7 05 ? ? ? ? 00 00 00 00 48 8b 7c 24 38 c7 46 54 ff ff ff ff 48 83 c4 20 5e c3"
rip_offset = 3
instruction_len = 11

[[autosplitter.patterns]]
name = "field_area"
pattern = "4c 8b 3d ? ? ? ? 8b 45 87"
rip_offset = 3
instruction_len = 7
```

### Elden Ring

```toml
[autosplitter]
enabled = true
algorithm = "binary_tree"

[[autosplitter.patterns]]
name = "virtual_memory_flag"
pattern = "44 89 7c 24 28 4c 8b 25 ? ? ? ? 4d 85 e4"
rip_offset = 8
instruction_len = 12
```

### Dark Souls 2

```toml
[autosplitter]
enabled = true
algorithm = "kill_counter"

[[autosplitter.patterns]]
name = "game_manager_imp"
pattern = "48 8b 35 ? ? ? ? 48 8b e9 48 85 f6"
rip_offset = 3
instruction_len = 7
```

## Troubleshooting

### Autosplitter Not Connecting

1. Check process names match exactly (case-insensitive)
2. Run NYA Core as administrator
3. Check for anti-cheat interference
4. Verify patterns are correct for your game version

### Flags Not Detected

1. Verify flag_id values are correct
2. Check algorithm matches your game's structure
3. Test with a known working flag first
4. Enable debug logging to see memory values

### Pattern Scan Fails

1. Patterns may have changed between game versions
2. Try alternative/fallback patterns
3. Use Cheat Engine to find updated patterns
4. Check if game uses anti-tamper protection

## Debug Logging

Enable debug output by checking the logs:

```
DS3: SprjEventFlagMan at 0x12345678
DS3: Connected! ptr1=0x12345678, ptr2=0x87654321
Boss defeated: Iudex Gundyr (flag: 14000800)
```

If you see `Pattern scan failed`, the memory patterns need updating.
