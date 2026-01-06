# Features Overview

NYA Core is a professional hit counter and run tracker for speedrunners and streamers. This document provides an overview of all major features.

## Core Features

### Hit Tracking
- **Per-split hit counting** - Track hits for each boss/split independently
- **Personal Best (PB) tracking** - Automatic PB comparison with delta display
- **Undo functionality** - Undo hits and splits to correct mistakes
- **Practice mode** - Isolated statistics that don't affect main records

### Timer System
- **Millisecond precision** - 1ms resolution for accurate timing
- **Multiple states** - Stopped, Running, Paused
- **Segment timing** - Track time per split and cumulative time
- **Best segments** - Gold split tracking for optimal segments
- **Time save calculation** - Shows potential time saves vs PB

### Split Management
- **Add/Remove splits** - Customize your run structure
- **Reorder splits** - Drag and drop to rearrange
- **Rename splits** - Custom names with alias support
- **Boss images** - Attach images to splits for visual display

## Multi-Run Mode

Track multiple games in sequence (e.g., "All Souls" marathons):

- **Game queue** - Add multiple games with presets
- **Auto-progression** - Automatic advancement through games
- **Aggregate stats** - Total hits and time across all games
- **Multi-run presets** - Save and load configurations

## Autosplitter

Automatic boss defeat detection:

- **Memory reading** - Direct game memory access
- **Pattern scanning** - Find addresses via byte patterns
- **Multiple algorithms** - Support for different game engines
- **Plugin-based** - Extensible via TOML plugins
- **Vision mode** - Screen capture for console games

## OBS Integration

Professional streaming overlays:

- **7 overlay types** - Counter, splits, progress, tabs, status, composite, preview
- **HTTP server** - Real-time data on port 8085
- **50+ customization options** - Colors, fonts, layouts, animations
- **Theme presets** - Save and share configurations
- **Drag-and-drop positioning** - Composite layout editor

## Twitch Integration

Interactive chat features:

- **Chat commands** - !hit, !rename, !lasthit
- **Permission system** - Broadcaster, mods, VIPs, subs, viewers
- **Auto-clipping** - Automatic Twitch clips on hits
- **Spam protection** - Cooldowns and rename limits

## Statistics

Comprehensive tracking:

- **Session stats** - Playtime, hits, resets per session
- **Yearly stats** - Activity heatmaps, streaks, milestones
- **Per-game stats** - Best times, completion rates
- **Per-preset stats** - PBs, averages, attempts
- **Per-split stats** - Best segments, hit averages

## Game Support

### Built-in Games
- Dark Souls Remastered
- Dark Souls II: Scholar of the First Sin
- Dark Souls III
- Sekiro: Shadows Die Twice
- Elden Ring
- Armored Core VI

### Plugin Games
- Bloodborne (vision plugin)
- Demon's Souls (vision plugin)
- Community-created plugins

### Custom Games
- User-defined games
- Custom split lists
- No autosplitter required

## Save Management

Practice mode save states:

- **Auto-backup** - Backup saves before each split
- **Restore** - Return to any previous split
- **Game detection** - Automatic save location detection

## Localization

Multi-language support:

- **Built-in languages** - English, Spanish, and more
- **Community translations** - Easy contribution system
- **Auto-updates** - Language files updated automatically

## Hotkeys

Global keyboard shortcuts:

- **Customizable bindings** - Assign any key
- **Low-level hooks** - Work in any window (Windows)
- **Numpad support** - Default numpad bindings
- **Modifier support** - Ctrl, Alt, Shift combinations

## Plugin System

Extensible architecture:

- **TOML Plugins** - Game configuration files
- **SDK Plugins** - Dynamic plugins with code
- **UI Components** - HTML/CSS/JS plugin interfaces
- **Permission system** - Sandboxed plugin access

## Data Management

- **Auto-save** - Configuration saved automatically
- **Corruption recovery** - Automatic backup restoration
- **Factory reset** - Clear all data option
- **Export/Import** - Share configurations
