# Autosplitter

The autosplitter automatically detects boss defeats by reading game memory. When a boss is killed, NYA Core automatically advances to the next split.

## Supported Games

| Game | Algorithm | Status |
|------|-----------|--------|
| Dark Souls Remastered | Offset Table | Full Support |
| Dark Souls II SOTFS | Kill Counter | Full Support |
| Dark Souls III | Category Decomposition | Full Support |
| Sekiro | Category Decomposition | Full Support |
| Elden Ring | Binary Tree | Full Support |
| Armored Core VI | Binary Tree | Full Support |
| Bloodborne | Vision (Capture Card) | Via Plugin |
| Demon's Souls | Vision (Capture Card) | Via Plugin |

## How It Works

1. **Process Detection** - NYA Core finds the running game
2. **Pattern Scanning** - Locates memory structures via byte patterns
3. **Flag Reading** - Checks boss defeat flags in memory
4. **Auto-Split** - Advances split when boss flag is set

## Using the Autosplitter

### Enable Autosplitter

1. Select a game with autosplitter support
2. Start the game
3. Go to Settings > Autosplitter
4. Click "Start Autosplitter"

### Status Indicators

| Status | Meaning |
|--------|---------|
| Searching... | Looking for game process |
| Attached | Connected to game |
| Running | Actively monitoring |
| Error | Connection failed |

### Manual Override

You can still use hotkeys while autosplitter is active:
- Hotkeys work alongside autosplitter
- Manual splits take priority
- Reset works normally

## Configuration

### Per-Game Settings

Each game plugin defines:
- Process names to detect
- Memory patterns to scan
- Flag IDs for each boss
- Algorithm configuration

### Global Settings

In Settings > Autosplitter:
- Enable/disable autosplitter
- Auto-start on game detection
- Scan interval (ms)

## Algorithms

### Category Decomposition

Used by: Dark Souls III, Sekiro

Flags are stored in categorized memory blocks:
```
Flag ID: 13000800
├── Category: 13 (div by 10,000,000)
├── Sub-category: 0 (div by 1,000)
└── Bit index: 800 (mod 1,000)
```

### Binary Tree

Used by: Elden Ring, Armored Core VI

Flags stored in tree structure:
```
Flag ID: 10000800
├── Category: 1000 (flag / 10,000)
└── Bit: 800 (flag % 10,000)
```

The tree is traversed to find the category node.

### Offset Table

Used by: Dark Souls Remastered

Direct offset lookup:
```
Flag ID decomposed into:
├── Group
├── Area
├── Section
└── Number
```

### Kill Counter

Used by: Dark Souls II SOTFS

Tracks kill counts:
```
Flag ID = byte offset
Boss dead = counter > 0
```

## Vision-Based Autosplitter

For console games via capture card:

### Setup

1. Connect capture card
2. Install vision plugin for your game
3. Configure capture source
4. Calibrate detection regions

### How It Works

1. Captures frames from video source
2. Analyzes screen for boss defeat indicators
3. Uses image matching or color detection
4. Triggers split on match

### Supported Sources

- Capture cards (Elgato, AVerMedia, etc.)
- OBS virtual camera
- Window capture

## Troubleshooting

### Autosplitter Not Attaching

1. **Game not detected**
   - Verify game is running
   - Check process name in plugin
   - Run NYA Core as administrator

2. **Pattern not found**
   - Game version may be unsupported
   - Check for game updates
   - Try alternative patterns

3. **Anti-cheat interference**
   - Some games block memory reading
   - EasyAntiCheat may prevent attachment
   - Check game's anti-cheat status

### Splits Not Triggering

1. **Wrong flag ID**
   - Verify boss flag in plugin
   - Test with known working bosses
   - Check plugin version

2. **Timing issues**
   - Increase scan interval
   - Flag may be set delayed
   - Check for duplicate flags

3. **Boss already defeated**
   - Flag already set from previous run
   - Reset game save
   - Start new game

### False Positives

1. **Split triggers too early**
   - Flag set before boss death
   - Contact plugin maintainer
   - Use manual override

2. **Multiple triggers**
   - Duplicate flag IDs
   - Check boss definitions
   - Report to plugin maintainer

## Creating Autosplitter Plugins

### Finding Flag IDs

Tools for finding boss flags:
- Cheat Engine
- Game-specific practice tools
- Memory scanners

### Testing Flags

1. Identify potential flag addresses
2. Monitor during boss fight
3. Confirm value changes on defeat
4. Test with multiple bosses

### Memory Patterns

Finding stable patterns:
1. Use signature scanning
2. Include enough context bytes
3. Add wildcard for variable bytes
4. Test across game versions

### Submitting Plugins

1. Fork NYA-Core-Assets repository
2. Add/update plugin.toml
3. Test thoroughly
4. Submit pull request

## Advanced Features

### Custom Triggers

Beyond boss defeats:
- Item pickups
- Area transitions
- Cutscene triggers
- Attribute thresholds

### Expression-Based Triggers

```toml
[[autosplitter.triggers]]
name = "low_health_split"
expression = "player.health < 100"
action = "split"
```

### Multi-Condition Triggers

```toml
[[autosplitter.triggers]]
name = "boss_and_item"
conditions = [
    { type = "flag", id = 13000800 },
    { type = "item", id = 100 }
]
action = "split"
```

## Performance

### Resource Usage

- CPU: Minimal (< 1%)
- Memory: ~10MB
- Scan rate: Configurable

### Optimization Tips

1. Use appropriate scan intervals
2. Disable when not needed
3. Close unused games
4. Monitor resource usage
