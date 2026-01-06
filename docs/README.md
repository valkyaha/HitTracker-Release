# NYA Core Documentation

Welcome to the official NYA Core documentation. This guide covers all features, integrations, and development resources.

## Table of Contents

### Getting Started
- [Features Overview](features.md) - Complete list of app features
- [Installation](installation.md) - How to install and set up NYA Core

### Core Features
- [Hit Tracking & Timer](hit-tracking.md) - Counter, splits, and timer system
- [Multi-Run Mode](multi-run.md) - Sequential game tracking
- [Statistics & Analytics](statistics.md) - Tracking and yearly statistics

### Integrations
- [OBS Integration](obs-integration.md) - Overlays and customization
- [Twitch Integration](twitch-integration.md) - Chat bot and clip creation
- [Autosplitter](autosplitter.md) - Automatic boss detection

### Customization
- [CSS Customization](css-customization.md) - OBS overlay styling
- [Hotkeys](hotkeys.md) - Keyboard shortcuts configuration

### Development
- [Plugin SDK](plugin-sdk.md) - Creating dynamic plugins with UI
- [Game Plugins](game-plugins.md) - TOML-based game support plugins
- [Language SDK](language-sdk.md) - Creating translations

### Reference
- [API Reference](api-reference.md) - Backend commands reference
- [Changelog](../CHANGELOG.md) - Version history

---

## Quick Links

| Resource | Description |
|----------|-------------|
| [GitHub Repository](https://github.com/valkyaha/HitCounter) | Source code |
| [Releases](https://github.com/valkyaha/HitTracker-Release/releases) | Download latest version |
| [Plugin Assets](https://github.com/valkyaha/NYA-Core-Assets) | Game plugins and languages |
| [Discord](#) | Community support |

## Supported Games

NYA Core supports the following games out of the box:

- **Dark Souls Remastered** - Full autosplitter support
- **Dark Souls II: Scholar of the First Sin** - Full autosplitter support
- **Dark Souls III** - Full autosplitter support
- **Sekiro: Shadows Die Twice** - Full autosplitter support
- **Elden Ring** - Full autosplitter support
- **Armored Core VI** - Full autosplitter support
- **Bloodborne** - Via vision plugin (capture card)
- **Demon's Souls** - Via vision plugin (capture card)
- **Custom Games** - User-defined games without plugins

## System Requirements

- **OS:** Windows 10/11, Linux, macOS
- **RAM:** 4GB minimum
- **Disk:** 100MB for application
- **For Autosplitter:** Target game must be running

## Version

Current Version: **3.1.0**

Built with Tauri 2.0 and Rust for maximum performance.
