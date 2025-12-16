# Flag Algorithms

This document explains the different memory reading algorithms used by VNoHitTracker for each game.

## Overview

FromSoftware games store boss defeat information in "event flags" - bits in memory that track game progress. Each game has a slightly different way of organizing these flags.

## Dark Souls 3 Algorithm

**Pattern Name:** `sprj_event_flag_man`

### Flag ID Structure

8-digit flag IDs (e.g., `14000800`):

```
14000800
│└┤└─┤
│ │  └── mod_1000 (800) - Bit position within category
│ └───── div_1k (0) - Sub-category index
└──────── div_10m (1) - Main category index
```

### Decomposition

```rust
let div_10m = (flag_id / 10_000_000) % 10;  // Main category
let div_1k = (flag_id / 1_000) % 10;         // Sub-category
let mod_1000 = flag_id % 1000;                // Bit position
```

### Pointer Chain

```
SprjEventFlagMan → +0x218 → [div_10m * 0x18] → 0x0 → [(div_1k << 4) + (category * 0xa8)]
```

### Bit Extraction

```rust
let byte_offset = (mod_1000 >> 5) * 4;
let bit_index = 0x1f - (mod_1000 & 0x1f);
let mask = 1u32 << bit_index;
```

### Category Fallback

DS3 tries 6 categories (0-5) because the exact category for a flag isn't always predictable.

### Example Flags

| Boss | Flag ID | div_10m | div_1k | mod_1000 |
|------|---------|---------|--------|----------|
| Iudex Gundyr | 14000800 | 1 | 0 | 800 |
| Vordt | 13000800 | 1 | 0 | 800 |
| Soul of Cinder | 14100800 | 1 | 0 | 800 |

## Elden Ring Algorithm

**Pattern Name:** `virtual_memory_flag`

### Binary Tree Structure

Elden Ring uses a more complex binary tree for flag lookup:

1. Read divisor from `VirtualMemoryFlag + 0x1c`
2. Calculate category: `flag_id / divisor`
3. Calculate remainder: `flag_id % divisor`
4. Traverse binary tree to find category node
5. Read flag bit from data pointer

### Tree Traversal

```rust
let mut current = read_ptr(root + 0x8);
loop {
    let marker = read_u8(current + 0x19);
    if marker != 0 { break; }

    let node_category = read_u32(current + 0x20);
    if node_category < target_category {
        current = read_ptr(current + 0x10);  // Go right
    } else {
        result_node = current;
        current = read_ptr(current);          // Go left
    }
}
```

### Data Pointer Resolution

Two modes based on `mystery` value at node + 0x28:

```rust
let mystery = read_i32(result_node + 0x28);
let data_ptr = if mystery == 0 {
    // Calculated pointer
    let mult = read_i32(vmf + 0x20);
    let offset = read_i32(result_node + 0x30);
    let base = read_u64(vmf + 0x28);
    (mult * offset + base) as usize
} else if mystery == 1 {
    return false;  // Not valid
} else {
    read_u64(result_node + 0x30) as usize
};
```

### Bit Extraction (Different from DS3)

```rust
let bit_index = 7 - (remainder & 7);
let byte_offset = (remainder >> 3) as usize;
let byte_val = read_u8(data_ptr + byte_offset);
(byte_val & (1 << bit_index)) != 0
```

### Example Flags

| Boss | Flag ID |
|------|---------|
| Margit | 10000850 |
| Godrick | 10000800 |
| Radagon | 19000800 |
| Malenia | 15000800 |

## Sekiro Algorithm

**Pattern Name:** `event_flag_man`

Similar to DS3 but simpler - no category fallback:

### Pointer Chain

```
EventFlagMan → +0x218 → [div_10m * 0x18] → 0x0 → [div_1k << 4]
```

### Key Difference

Sekiro directly reads from `div_1k << 4` without trying multiple categories.

### Example Flags

Sekiro uses internal counter IDs rather than traditional event flags:

| Boss | Flag ID |
|------|---------|
| Gyoubu | 9301 |
| Genichiro | 9303 |
| Isshin | 9312 |

## Dark Souls Remastered Algorithm

**Pattern Name:** `event_flags`

### Flag ID Structure

8-digit flags decomposed into group/area/section/number:

```
11010901
│└┬┘│└┬┘
│ │ │ └── number (901)
│ │ └──── section (0)
│ └────── area (101)
└──────── group (1)
```

### Group Offsets

```rust
let group_offset = match group {
    "0" => 0x00000,
    "1" => 0x00500,
    "5" => 0x05F00,
    "6" => 0x0B900,
    "7" => 0x11300,
    _ => return false,
};
```

### Area Index Table

```rust
let area_index = match area {
    "000" => 0, "100" => 1, "101" => 2, "102" => 3,
    "110" => 4, "120" => 5, "121" => 6, "130" => 7,
    "131" => 8, "132" => 9, "140" => 10, "141" => 11,
    "150" => 12, "151" => 13, "160" => 14, "170" => 15,
    "180" => 16, "181" => 17, "200" => 18, "210" => 19,
    _ => return false,
};
```

### Offset Calculation

```rust
let offset = group_offset
    + (area_index * 0x500)
    + (section * 128)
    + ((number - (number % 32)) / 8);
let mask = 0x80000000u32 >> (number % 32);
```

### Special Case: Small Flags

Flags < 100 are stored directly:

```rust
if flag_id < 100 {
    let value = read_u32(event_flags + flag_id * 4);
    return value != 0;
}
```

### Example Flags

| Boss | Flag ID |
|------|---------|
| Taurus Demon | 11010901 |
| O&S | 12 |
| Gwyn | 15 |

## Dark Souls 2 Algorithm

**Pattern Name:** `game_manager_imp`

### Kill Counter Array

DS2 uses a simpler system - a kill counter array where each boss has an offset:

### Pointer Chain

```
GameManagerImp → +0x70 → +0x28 → +0x20 → +0x8 + offset
```

### Flag Reading

```rust
let count = read_i32(counters + boss_offset);
count > 0  // Boss defeated if killed at least once
```

### Example Offsets

| Boss | Offset |
|------|--------|
| Last Giant | 124 |
| Pursuer | 112 |
| Nashandra | 132 |
| Fume Knight | 204 |

## Choosing an Algorithm

| Your Game's Structure | Use Algorithm |
|----------------------|---------------|
| Category decomposition (div_10m, div_1k) | `ds3` |
| Binary tree categories | `eldenring` |
| Group/area/section tables | `ds1` |
| Kill counter array | `ds2` |
| Same as Sekiro (no category fallback) | `sekiro` |

## Implementing a New Algorithm

If your game has a unique structure:

1. Implement the `FlagReader` trait
2. Register it in `create_flag_reader()`
3. Document the algorithm here
