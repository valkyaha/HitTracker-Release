# VNoHitTracker

A no-hit run tracker for Souls-like games, designed for speedrunners and challenge runners.

## Downloads

| File | Description |
|------|-------------|
| `VNoHitTracker.exe` | Portable executable (no installation required) |
| `VNoHitTracker_1.0.0_x64-setup.exe` | NSIS Installer (recommended) |
| `VNoHitTracker_1.0.0_x64_en-US.msi` | MSI Installer |

## Features

- Track hits across boss fights in Dark Souls, Dark Souls II, Dark Souls III, Demon's Souls, Bloodborne, Sekiro, and Elden Ring
- Boss presets matching HitCounterManager format
- Multi-run mode for tracking multiple games in sequence (Trilogy runs, God Runs, etc.)
- OBS integration with customizable overlays
- Statistics tracking with personal bests
- Custom game and preset support
- Multi-language support (English, Spanish)
- Theme customization

## Quick Start

### Portable Version
1. Download `VNoHitTracker.exe`
2. Run the application
3. Select a game and preset from the Games view

### Installer Version
1. Download `VNoHitTracker_1.0.0_x64-setup.exe`
2. Run the installer
3. Launch VNoHitTracker from the Start menu

## Hotkeys

| Key | Action |
|-----|--------|
| **Numpad +** | Add hit |
| **Numpad -** | Undo hit |
| **Numpad 0** | Boss defeated (next split) |
| **Numpad *** | Reset run |
| **Numpad Enter** | Start/Stop timer |

## OBS Integration

1. Go to OBS Integration view
2. Click "Start Server" (default port: 9876)
3. In OBS, add a Browser Source with URL: `http://localhost:9876`
4. Customize overlay appearance in the app

## Multi-Run Mode

Perfect for Trilogy runs, God Runs, or any multi-game challenge:

1. Go to Multi-Run view
2. Add games in order or use a preset
3. Use "Boss Defeated" to progress through each game
4. Track total hits across all games

### Built-in Presets

- **Souls Trilogy**: DS1 + DS2 + DS3
- **God Run 1**: DeS + DS1 + DS2 + DS3 + BB
- **God Run 2**: DS1 + DS2 + DS3 + BB + Sekiro
- **God Run 3**: DS1 + DS2 + DS3 + Sekiro + ER
- **Ultimate God Run**: All 7 Souls games

## Documentation

- [Language SDK](docs/LANGUAGE_SDK.md) - Create custom translations
- [Plugin SDK](docs/PLUGIN_SDK.md) - Create game plugins with autosplitter support

## Configuration Files

For the portable version, all configuration is stored next to the executable:
- `config.json` - Application settings
- `stats.json` - Statistics and personal bests
- `languages/` - Custom language files

## System Requirements

- Windows 10/11 (64-bit)
- ~50MB disk space

## Antivirus Notes

The executable is not code-signed, which may cause antivirus software to flag it as unknown. This is a false positive. You may need to add an exception in your antivirus software.

## Credits

Boss presets based on [HitCounterManager](https://github.com/topeterk/HitCounterManager) format.

## License

MIT License
