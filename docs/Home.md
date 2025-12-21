# NYA Core Documentation

Welcome to the NYA Core documentation! This covers how to use the application, create game plugins, and extend its functionality.

## Quick Links

- [Creating Plugins](Creating-Plugins.md) - Add support for new games
- [Autosplitter Guide](Autosplitter-Guide.md) - How the autosplitter works
- [SDK Reference](SDK-Reference.md) - API documentation for developers
- [Flag Algorithms](Flag-Algorithms.md) - Memory reading algorithms explained
- [Language SDK](LANGUAGE_SDK.md) - Create custom translations
- [Plugin SDK](PLUGIN_SDK.md) - Detailed plugin development guide

## What is NYA Core?

NYA Core is a no-hit run tracker for FromSoftware Souls games and similar titles. It features:

- **Hit Counting** - Track hits taken during your run
- **Boss Tracking** - Automatic split advancement when bosses are defeated
- **OBS Integration** - Display your run status in OBS
- **Multi-Run Support** - Track multiple games in a single session
- **Personal Bests** - Compare against your best runs
- **Autosplitter** - Automatic boss detection via memory reading
- **Low-Level Hotkeys** - Works in fullscreen games

## Supported Games

Built-in support for:
- Dark Souls Remastered
- Dark Souls II: Scholar of the First Sin
- Dark Souls III
- Sekiro: Shadows Die Twice
- Elden Ring
- Armored Core 6

## Adding Game Support

### TOML Plugin (Recommended)

Create a `plugin.toml` file with boss definitions and autosplitter patterns:

```toml
[plugin]
id = "your_game"
name = "Your Game Name"

[process]
names = ["YourGame.exe"]

[autosplitter]
enabled = true
algorithm = "category_decomposition"

[[bosses]]
id = "first_boss"
name = "First Boss"
flag_id = 12345678

[[presets]]
id = "all-bosses"
name = "All Bosses"
boss_ids = ["first_boss", "second_boss"]
```

See [Creating Plugins](Creating-Plugins.md) for full documentation.

## Getting Help

- [GitHub Issues](https://github.com/valkyaha/HitTracker-Release/issues) - Report bugs or request features
- [Releases](https://github.com/valkyaha/HitTracker-Release/releases) - Download latest version
