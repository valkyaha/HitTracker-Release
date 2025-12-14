# VNoHitTracker Plugin SDK

Create custom game plugins for VNoHitTracker with optional autosplitter support.

## Overview

The VNoHitTracker Plugin SDK allows you to:
- Add support for new games
- Define boss lists and presets
- Implement autosplitter functionality (automatic boss defeat detection)

## Plugin Structure

Plugins are Rust dynamic libraries (`.dll` on Windows) that implement the `HitCounterPlugin` trait.

### Basic Plugin

```rust
use hitcounter_sdk::{
    HitCounterPlugin, PluginInfo, PluginError,
    BossEntry, PresetInfo, BossDefeatEvent
};

pub struct MyGamePlugin {
    // Plugin state
}

impl HitCounterPlugin for MyGamePlugin {
    fn info(&self) -> PluginInfo {
        PluginInfo {
            id: "mygame".to_string(),
            name: "My Game".to_string(),
            short_name: "MG".to_string(),
            version: "1.0.0".to_string(),
            author: "Your Name".to_string(),
            description: "Support for My Game".to_string(),
            game_image: None,
            has_autosplitter: false,
        }
    }

    fn get_bosses(&self) -> Vec<BossEntry> {
        vec![
            BossEntry::new("boss1", "First Boss", "Area 1", true),
            BossEntry::new("boss2", "Second Boss", "Area 2", true),
            BossEntry::new("boss3", "Final Boss", "Final Area", true),
        ]
    }

    fn get_presets(&self) -> Vec<PresetInfo> {
        vec![
            PresetInfo {
                id: "all-bosses".to_string(),
                name: "All Bosses".to_string(),
                description: "All 3 bosses".to_string(),
                boss_ids: vec![
                    "boss1".to_string(),
                    "boss2".to_string(),
                    "boss3".to_string(),
                ],
            },
        ]
    }
}

// Required export function
#[no_mangle]
pub extern "C" fn create_plugin() -> *mut dyn HitCounterPlugin {
    Box::into_raw(Box::new(MyGamePlugin {}))
}
```

## BossEntry

Represents a boss in the game:

```rust
pub struct BossEntry {
    pub id: String,           // Unique identifier
    pub name: String,         // Display name
    pub location: String,     // In-game location
    pub is_required: bool,    // Required for any% completion
    pub is_dlc: bool,         // DLC boss
    pub aliases: Vec<String>, // Alternative names
}

// Helper constructors
BossEntry::new(id, name, location, is_required)
BossEntry::dlc(id, name, location, dlc_name)
BossEntry::optional(id, name, location)
```

## Autosplitter Support

To add automatic boss defeat detection, implement the autosplitter methods:

```rust
impl HitCounterPlugin for MyGamePlugin {
    // ... basic methods ...

    fn has_autosplitter(&self) -> bool {
        true
    }

    fn connect(&mut self) -> Result<(), PluginError> {
        // Find and attach to game process
        Ok(())
    }

    fn disconnect(&mut self) {
        // Cleanup when disconnecting
    }

    fn is_connected(&self) -> bool {
        false
    }

    fn poll(&mut self) -> Option<BossDefeatEvent> {
        // Called every frame
        // Return Some(event) when a boss is defeated
        None
    }

    fn get_defeated_bosses(&self) -> Vec<String> {
        vec![]
    }

    fn reset(&mut self) {
        // Reset autosplitter state
    }
}
```

### Memory Reading

The SDK provides utilities for reading game memory:

```rust
use hitcounter_sdk::MemoryReader;

let reader = MemoryReader::attach("game.exe")?;

// Read a value at an address
let value: u32 = reader.read(0x12345678)?;

// Read with pointer chain
let value: u32 = reader.read_pointer_chain(
    base_address,
    &[0x10, 0x20, 0x30]
)?;
```

## Plugin Installation

1. Build your plugin as a `.dll`
2. Place it in the `plugins` folder next to the executable
3. Restart VNoHitTracker
4. Your game will appear in the Games list

## Building Plugins

### Cargo.toml

```toml
[package]
name = "mygame-plugin"
version = "0.1.0"
edition = "2021"

[lib]
crate-type = ["cdylib"]

[dependencies]
hitcounter-sdk = { path = "../hitcounter-sdk" }
```

### Build Command

```bash
cargo build --release
```

The output `.dll` will be in `target/release/`.

## Support

For questions or issues, open a GitHub issue.
