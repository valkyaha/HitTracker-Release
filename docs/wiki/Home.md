# VNoHitTracker Wiki

Welcome to the VNoHitTracker wiki! This documentation covers how to use the application, create game plugins, and extend its functionality.

## Quick Links

- [Creating Plugins](Creating-Plugins.md) - Add support for new games
- [Autosplitter Guide](Autosplitter-Guide.md) - How the autosplitter works
- [SDK Reference](SDK-Reference.md) - API documentation for developers
- [Flag Algorithms](Flag-Algorithms.md) - Memory reading algorithms explained

## What is VNoHitTracker?

VNoHitTracker is a no-hit run tracker for FromSoftware Souls games and similar titles. It features:

- **Hit Counting** - Track hits taken during your run
- **Boss Tracking** - Automatic split advancement when bosses are defeated
- **OBS Integration** - Display your run status in OBS
- **Multi-Run Support** - Track multiple games in a single session
- **Personal Bests** - Compare against your best runs
- **Autosplitter** - Automatic boss detection via memory reading

## Supported Games

Built-in support for:
- Dark Souls Remastered
- Dark Souls II: Scholar of the First Sin
- Dark Souls III
- Sekiro: Shadows Die Twice
- Elden Ring (including Shadow of the Erdtree DLC)

## Adding Game Support

There are two ways to add support for new games:

### 1. TOML Plugin (Recommended for most games)

Create a `plugin.toml` file with boss definitions and autosplitter patterns:

```toml
[plugin]
id = "your_game"
name = "Your Game Name"

[process]
names = ["YourGame.exe"]

[autosplitter]
enabled = true
algorithm = "ds3"  # Use existing algorithm

[[bosses]]
id = "first_boss"
name = "First Boss"
flag_id = 12345678
```

See [Creating Plugins](Creating-Plugins.md) for full documentation.

### 2. Rust Plugin (For custom memory structures)

Implement the `FlagReader` trait for games with unique memory layouts:

```rust
use hitcounter_sdk::{FlagReader, MemoryContext, PatternConfig};

pub struct MyGameFlagReader;

impl FlagReader for MyGameFlagReader {
    fn algorithm_name(&self) -> &'static str { "my_game" }
    // ... implement pattern scanning and flag reading
}
```

See [SDK Reference](SDK-Reference.md) for full documentation.

## Getting Help

- [GitHub Issues](https://github.com/valkyaha/HitCounter/issues) - Report bugs or request features
- [Releases](https://github.com/valkyaha/HitTracker-Release/releases) - Download latest version
