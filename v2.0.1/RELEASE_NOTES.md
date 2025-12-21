# NYA Core v2.0.1 - First Official Release

## What's New

### Major Features
- **Complete UI Redesign** - Modern, polished interface with improved OBS customization
- **Low-Level Keyboard Hooks** - Global hotkeys now work in games that bypass OS-level shortcuts (Dark Souls 1 PTD, Dark Souls 3, etc.)
- **Full Internationalization (i18n)** - Complete Spanish translation, with support for custom language packs
- **Plugin System** - Extensible game support through TOML-based plugins
- **About Page** - New modular about page with credits and feature overview

### OBS Integration Improvements
- Redesigned OBS customization panel with live preview
- Split opacity and badge gap controls
- Fixed overlay scaling issues
- Reduced network polling by ~94% for better performance
- 60fps timer interpolation with millisecond precision

### Autosplitter Enhancements
- Linux compilation support for autosplitter module
- Fixed custom split names being overwritten when selecting bosses
- Dynamic polling (fast only when timer is running)

### Quality of Life
- In-app modal dialogs (no more Windows native popups)
- Easter egg system for special dates
- Trans lives matter message in About footer
- Timer float precision fixes
- Multirun timer sync between app and OBS

### Supported Games
- Dark Souls Remastered
- Dark Souls II: Scholar of the First Sin
- Dark Souls III
- Sekiro: Shadows Die Twice
- Elden Ring
- Armored Core VI
- Bloodborne (via plugin)
- Demon's Souls (via plugin)

## Installation

### Windows
- **Installer (Recommended)**: `NYA Core_2.0.1_x64-setup.exe`
- **MSI Installer**: `NYA Core_2.0.1_x64_en-US.msi`
- **Portable**: `NYA_Core_2.0.1_portable.zip` - Extract and run

### Linux
- **AppImage**: `NYA_Core_2.0.1_amd64.AppImage`
- **Debian/Ubuntu**: `NYA_Core_2.0.1_amd64.deb`

## Notes
- The portable version includes `languages/` and `plugins/` folders for customization
- Custom language packs can be added to the `languages/` folder
- Custom game plugins can be added to the `plugins/` folder

## Bug Fixes
- Fixed timer precision issues with floating point milliseconds
- Fixed multirun reset not syncing properly
- Fixed OBS overlay canvas scaling
- Fixed autosplitter overwriting custom split names
