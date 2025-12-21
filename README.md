# NYA Core - No-Hit Run Tracker

A professional no-hit run tracker for Souls-like games with OBS integration, autosplitter support, and multi-run tracking.

[![Latest Release](https://img.shields.io/github/v/release/valkyaha/HitTracker-Release)](https://github.com/valkyaha/HitTracker-Release/releases/latest)
[![Downloads](https://img.shields.io/github/downloads/valkyaha/HitTracker-Release/total)](https://github.com/valkyaha/HitTracker-Release/releases)

## Downloads

Download the latest version from [**Releases**](https://github.com/valkyaha/HitTracker-Release/releases/latest)

| File | Description |
|------|-------------|
| `NYA_Core_x.x.x_portable.zip` | Portable version (no installation) |
| `NYA Core_x.x.x_x64-setup.exe` | NSIS Installer (recommended) |
| `NYA Core_x.x.x_x64_en-US.msi` | MSI Installer |

## Features

- **Hit Tracking** - Track hits across boss fights with undo support
- **Autosplitter** - Automatic boss detection via memory reading
- **Multi-Run Mode** - Track multiple games in sequence (Trilogy, God Runs)
- **OBS Integration** - Customizable overlays via browser source
- **Personal Bests** - Statistics tracking and run comparisons
- **Low-Level Hotkeys** - Works even in fullscreen games
- **Plugin System** - Add support for new games via TOML plugins
- **Internationalization** - Full i18n with community translations

## Supported Games

| Game | Autosplitter | Algorithm |
|------|-------------|-----------|
| Dark Souls Remastered | Yes | offset_table |
| Dark Souls II: SOTFS | Yes | kill_counter |
| Dark Souls III | Yes | category_decomposition |
| Sekiro: Shadows Die Twice | Yes | category_decomposition |
| Elden Ring | Yes | binary_tree |
| Armored Core 6 | Yes | binary_tree |
| Bloodborne | Manual | - |
| Demon's Souls | Manual | - |

## Quick Start

1. Download and extract/install NYA Core
2. Launch the application
3. Select a game and boss preset from the Games view
4. Configure hotkeys in Settings
5. Start your run!

## Hotkeys

| Key | Action |
|-----|--------|
| **Numpad +** | Add hit |
| **Numpad -** | Undo hit |
| **Numpad Enter** | Boss defeated (next split) |
| **Numpad *** | Toggle timer |
| **Numpad /** | Reset run |

### Hotkey Modes

- **Window Only** - Keys work only when app is focused
- **Global Shortcuts** - Works from any window (default)
- **Low-Level Hooks** - Works in games that bypass OS shortcuts (Windows)

## OBS Integration

1. Go to **Settings > OBS Integration**
2. Click **Start Server** (default port: 9876)
3. In OBS, add a **Browser Source** with URL: `http://localhost:9876`
4. Customize appearance in the OBS Preview tab

## Documentation

- [Home](docs/Home.md) - Overview and quick links
- [Creating Plugins](docs/Creating-Plugins.md) - Add support for new games
- [Autosplitter Guide](docs/Autosplitter-Guide.md) - How the autosplitter works
- [SDK Reference](docs/SDK-Reference.md) - Complete API documentation
- [Flag Algorithms](docs/Flag-Algorithms.md) - Memory reading algorithms
- [Language SDK](docs/LANGUAGE_SDK.md) - Create translations
- [Plugin SDK](docs/PLUGIN_SDK.md) - Plugin development guide

## System Requirements

- Windows 10/11 (64-bit)
- Linux (AppImage) - limited autosplitter support
- ~50MB disk space

## Antivirus Notes

The executable uses low-level keyboard hooks and memory reading which may trigger antivirus false positives. You may need to add an exception.

## License

MIT OR Apache-2.0

## Contributing

Contributions welcome! Please submit issues and pull requests to the main repository.
