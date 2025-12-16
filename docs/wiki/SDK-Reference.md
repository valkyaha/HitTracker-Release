# SDK Reference

The HitCounter SDK provides types and traits for creating game plugins.

## Installation

Add to your `Cargo.toml`:

```toml
[dependencies]
hitcounter-sdk = { git = "https://github.com/valkyaha/HitCounter" }
```

## Core Types

### PatternConfig

Memory pattern for finding game data structures.

```rust
pub struct PatternConfig {
    /// Pattern name (e.g., "sprj_event_flag_man")
    pub name: String,
    /// Byte pattern with ? wildcards (e.g., "48 c7 05 ? ? ? ?")
    pub pattern: String,
    /// Position of RIP-relative offset
    pub rip_offset: usize,
    /// Total instruction length
    pub instruction_len: usize,
    /// Fallback patterns if primary fails
    pub fallback_patterns: Vec<String>,
}
```

### PointerChainConfig

Pointer chain for memory traversal.

```rust
pub struct PointerChainConfig {
    /// Chain name (e.g., "event_flags")
    pub name: String,
    /// Offsets to follow from base pointer
    pub offsets: Vec<i64>,
}
```

### AutosplitterConfig

Autosplitter configuration from plugin.toml.

```rust
pub struct AutosplitterConfig {
    /// Whether autosplitter is enabled
    pub enabled: bool,
    /// Algorithm name ("ds3", "eldenring", etc.)
    pub algorithm: String,
    /// Memory patterns
    pub patterns: Vec<PatternConfig>,
    /// Pointer chains
    pub pointer_chains: Vec<PointerChainConfig>,
}
```

### BossDefinition

Boss definition from plugin.toml.

```rust
pub struct BossDefinition {
    pub id: String,
    pub name: String,
    pub location: String,
    pub required: bool,
    pub is_dlc: bool,
    pub dlc_name: Option<String>,
    pub flag_id: u32,
    pub order: u32,
    pub aliases: Vec<String>,
}
```

### MemoryContext

Context for flag reading after connecting to a game.

```rust
pub struct MemoryContext {
    /// Process handle (platform-specific)
    pub handle: usize,
    /// Discovered pointers (pattern_name -> address)
    pub pointers: HashMap<String, usize>,
}
```

## FlagReader Trait

Implement this trait for custom flag reading algorithms.

```rust
pub trait FlagReader: Send + Sync {
    /// Algorithm name (e.g., "my_game")
    fn algorithm_name(&self) -> &'static str;

    /// Scan patterns and return discovered pointers
    fn scan_patterns(
        &self,
        handle: usize,
        base: usize,
        size: usize,
        patterns: &[PatternConfig],
    ) -> Option<HashMap<String, usize>>;

    /// Check if a flag is set
    fn is_flag_set(&self, ctx: &MemoryContext, flag_id: u32) -> bool;

    /// Get all defeated flags (optional override)
    fn get_defeated_flags(&self, ctx: &MemoryContext, flag_ids: &[u32]) -> Vec<u32> {
        flag_ids.iter()
            .filter(|&&id| self.is_flag_set(ctx, id))
            .copied()
            .collect()
    }
}
```

## Memory Utilities

### parse_pattern

Parse a pattern string into bytes and mask.

```rust
use hitcounter_sdk::memory::parse_pattern;

let pattern = parse_pattern("48 c7 05 ? ? ? ?");
// Returns: [(0x48, false), (0xc7, false), (0x05, false), (0, true), ...]
```

### resolve_rip_relative

Resolve a RIP-relative address.

```rust
use hitcounter_sdk::memory::resolve_rip_relative;

let addr = resolve_rip_relative(
    instruction_addr,  // Where the instruction is
    instruction_len,   // Total length (e.g., 11)
    offset_value,      // The 4-byte signed offset
);
```

## Example: Custom FlagReader

```rust
use hitcounter_sdk::{FlagReader, MemoryContext, PatternConfig};
use std::collections::HashMap;

pub struct MyGameFlagReader;

impl FlagReader for MyGameFlagReader {
    fn algorithm_name(&self) -> &'static str {
        "my_game"
    }

    fn scan_patterns(
        &self,
        handle: usize,
        base: usize,
        size: usize,
        patterns: &[PatternConfig],
    ) -> Option<HashMap<String, usize>> {
        let mut result = HashMap::new();

        // Get pattern from config or use default
        let pattern = patterns.iter()
            .find(|p| p.name == "event_flags")
            .map(|p| p.pattern.as_str())
            .unwrap_or("48 8B 0D ? ? ? ?");

        // TODO: Implement pattern scanning
        // - Read memory at base..base+size
        // - Search for pattern match
        // - Resolve RIP-relative address
        // - Store in result map

        // result.insert("event_flags".to_string(), found_address);

        if result.is_empty() {
            None
        } else {
            Some(result)
        }
    }

    fn is_flag_set(&self, ctx: &MemoryContext, flag_id: u32) -> bool {
        let event_flags = match ctx.pointers.get("event_flags") {
            Some(&addr) => addr,
            None => return false,
        };

        // TODO: Implement flag reading logic
        // - Follow pointer chain from event_flags
        // - Calculate bit position from flag_id
        // - Read and check the bit

        false
    }
}
```

## GamePlugin Trait

For hit tracking (separate from autosplitter):

```rust
pub trait GamePlugin: Send + Sync {
    fn game_name(&self) -> &str;
    fn version(&self) -> &str;
    fn initialize(&mut self, process_id: u32) -> Result<(), PluginError>;
    fn poll_hits(&mut self) -> Result<Option<HitEvent>, PluginError>;
    fn poll_boss_defeats(&mut self) -> Result<Option<BossDefeatEvent>, PluginError>;
    fn get_bosses(&self) -> Vec<Boss>;
    fn is_boss_defeated(&self, boss_id: &str) -> Result<bool, PluginError>;
    fn get_stats(&self) -> HitStats;
    fn reset(&mut self);
    fn shutdown(&mut self) -> Result<(), PluginError>;
}
```

## Error Types

```rust
pub enum PluginError {
    ProcessAttachFailed(String),
    MemoryReadFailed(String),
    ProcessNotFound,
    InvalidAddress(String),
    NotInitialized,
    Custom(String),
}
```

## Best Practices

1. **Use Default Patterns** - Provide sensible defaults in your FlagReader
2. **Handle Failures Gracefully** - Return None/false instead of crashing
3. **Log Useful Info** - Use log::info! for successful connections
4. **Support Fallback Patterns** - Game updates may change patterns
5. **Test Thoroughly** - Verify with actual game memory
