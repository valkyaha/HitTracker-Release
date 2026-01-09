# NYA Core - No-Hit Run Tracker

A professional no-hit run tracker for Souls-like games with OBS integration, autosplitter support, and multi-run tracking.

[![Latest Release](https://img.shields.io/github/v/release/valkyaha/HitTracker-Release)](https://github.com/valkyaha/HitTracker-Release/releases/latest)
[![Downloads](https://img.shields.io/github/downloads/valkyaha/HitTracker-Release/total)](https://github.com/valkyaha/HitTracker-Release/releases)
[![Ko-fi](https://img.shields.io/badge/Ko--fi-Support%20Development-FF5E5B?logo=ko-fi&logoColor=white)](https://ko-fi.com/darkittyvt)

## Downloads

Download the latest version from [**Releases**](https://github.com/valkyaha/HitTracker-Release/releases/latest)

| File | Description |
|------|-------------|
| `NYA_Core_x.x.x_portable.zip` | Portable version (no installation) |
| `NYA Core_x.x.x_x64-setup.exe` | NSIS Installer (recommended) |
| `NYA Core_x.x.x_x64_en-US.msi` | MSI Installer |

## Features

- **Hit Tracking** - Track hits across boss fights with undo support
- **Split Timer** - Full timer with pause/resume and PB comparison
- **Autosplitter** - Automatic boss detection via memory reading
- **Vision Autosplitter** - Screen capture detection for console games
- **Multi-Run Mode** - Track multiple games in sequence (Trilogy, God Runs)
- **OBS Integration** - Customizable overlays via browser source
- **Twitch Integration** - Chat commands, channel points, and auto-clips
- **Personal Bests** - Statistics tracking and run comparisons
- **Practice Mode** - Train specific splits with save state sync
- **Low-Level Hotkeys** - Works even in fullscreen games
- **Plugin System** - Add support for new games via TOML plugins
- **Internationalization** - Full i18n with community translations

## Supported Games

| Game | Autosplitter | Algorithm |
|------|:------------:|-----------|
| Dark Souls Remastered | ✓ | offset_table |
| Dark Souls II: SOTFS | ✓ | kill_counter |
| Dark Souls III | ✓ | category_decomposition |
| Sekiro: Shadows Die Twice | ✓ | category_decomposition |
| Elden Ring | ✓ | binary_tree |
| Armored Core 6 | ✓ | binary_tree |
| Bloodborne | Manual | - |
| Demon's Souls | Manual | - |

*More games available via [community plugins](https://github.com/valkyaha/NYA-Core-Assets)*

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

1. Go to **OBS Integration** in the sidebar
2. Click **Start Server** (default port: 9876)
3. In OBS, add a **Browser Source** with the overlay URL
4. Customize appearance in the Live Preview tab

### Available Overlays

| Overlay | Description |
|---------|-------------|
| Hit Counter | Main counter display with timer |
| Splits List | Boss checklist with progress |
| Progress Bar | Visual progress indicator |
| Game Tabs | Multi-run game selector |

## Documentation

Full documentation available in the [Wiki](https://github.com/valkyaha/HitTracker-Release/wiki):

- [Installation](wiki/Installation.md) - Setup guide
- [Features](wiki/Features.md) - Complete feature overview
- [OBS Integration](wiki/OBS-Integration.md) - Overlay setup and customization
- [Twitch Integration](wiki/Twitch-Integration.md) - Chat commands and channel points
- [Autosplitter](wiki/Autosplitter.md) - Memory-based boss detection
- [Plugin SDK](wiki/Plugin-SDK.md) - Create plugins for new games
- [Language SDK](wiki/Language-SDK.md) - Create translations
- [CSS Customization](wiki/CSS-Customization.md) - Advanced overlay styling

## System Requirements

- Windows 10/11 (64-bit)
- Linux (AppImage) - limited autosplitter support via Proton
- ~50MB disk space

## Antivirus Notes

The autosplitter uses memory reading APIs which may trigger antivirus false positives. This is common with speedrun tools like LiveSplit and SoulSplitter. You may need to add an exception for NYA Core.

## License

- **NYA Core Application**: MIT OR Apache-2.0
- **Plugins**: GPL-3.0 (due to derivative data from SoulSplitter)

## Special Thanks

- **[FrankvdStam](https://github.com/FrankvdStam)** - Creator of [SoulSplitter](https://github.com/FrankvdStam/SoulSplitter), whose comprehensive boss/bonfire/flag data made the autosplitter functionality possible

## Contact & Support

| | |
|---|---|
| **Discord** | [darkittyvt](https://discord.com/users/darkittyvt) |
| **Email** | darkvalhalla@proton.me |
| **Ko-fi** | [Support Development](https://ko-fi.com/darkittyvt) |
| **Issues** | [GitHub Issues](https://github.com/valkyaha/HitTracker-Release/issues) |

---

<p align="center">
  <a href="https://ko-fi.com/darkittyvt">
    <img src="https://ko-fi.com/img/githubbutton_sm.svg" alt="Support on Ko-fi">
  </a>
</p>
