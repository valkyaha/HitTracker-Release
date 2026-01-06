# Language SDK

NYA Core supports multiple languages through JSON-based translation files. This guide explains how to create and contribute translations.

## Language File Structure

Language files are JSON with the following structure:

```json
{
  "info": {
    "id": "es",
    "name": "Spanish",
    "native_name": "Español",
    "author": "Community",
    "version": "1.0.0"
  },
  "strings": {
    "key.path": "Translated text"
  }
}
```

## File Location

Language files are stored in:
```
languages/
├── en.json     # English (default)
├── es.json     # Spanish
├── ja.json     # Japanese
├── de.json     # German
└── ...
```

## Translation Keys

Keys use dot-notation to organize translations:

### Navigation
```json
{
  "nav.counter": "Counter",
  "nav.games": "Games",
  "nav.splits": "Splits",
  "nav.multirun": "Multi-Run",
  "nav.obs": "OBS",
  "nav.twitch": "Twitch",
  "nav.statistics": "Statistics",
  "nav.settings": "Settings",
  "nav.about": "About"
}
```

### Counter View
```json
{
  "counter.hits": "HITS",
  "counter.total": "TOTAL",
  "counter.pb": "PB",
  "counter.delta": "DELTA",
  "counter.ahead": "ahead",
  "counter.behind": "behind",
  "counter.gold": "GOLD"
}
```

### Status Messages
```json
{
  "status.idle": "Idle",
  "status.active": "Active",
  "status.completed": "Completed",
  "status.practice": "Practice Mode"
}
```

### Timer
```json
{
  "timer.start": "Start",
  "timer.pause": "Pause",
  "timer.resume": "Resume",
  "timer.stop": "Stop",
  "timer.reset": "Reset"
}
```

### Multi-Run
```json
{
  "multirun.title": "Multi-Run Mode",
  "multirun.add_game": "Add Game",
  "multirun.remove_game": "Remove Game",
  "multirun.start": "Start Multi-Run",
  "multirun.end": "End Multi-Run",
  "multirun.total_hits": "Total Hits",
  "multirun.total_time": "Total Time"
}
```

### Settings
```json
{
  "settings.general": "General",
  "settings.theme": "Theme",
  "settings.language": "Language",
  "settings.hotkeys": "Hotkeys",
  "settings.autosplitter": "Autosplitter",
  "settings.data": "Data Management",
  "settings.save": "Save",
  "settings.cancel": "Cancel"
}
```

### Common Actions
```json
{
  "common.save": "Save",
  "common.cancel": "Cancel",
  "common.delete": "Delete",
  "common.edit": "Edit",
  "common.add": "Add",
  "common.remove": "Remove",
  "common.confirm": "Confirm",
  "common.close": "Close"
}
```

### Dialogs
```json
{
  "dialogs.confirm_reset": "Are you sure you want to reset?",
  "dialogs.confirm_delete": "Are you sure you want to delete this?",
  "dialogs.unsaved_changes": "You have unsaved changes. Continue?",
  "dialogs.factory_reset": "This will delete all data. Continue?"
}
```

## Creating a New Translation

### Step 1: Copy Base File

Start with the English file as a template:

```bash
cp languages/en.json languages/xx.json
```

Replace `xx` with your language code (ISO 639-1).

### Step 2: Update Info Section

```json
{
  "info": {
    "id": "xx",
    "name": "Language Name",
    "native_name": "Native Name",
    "author": "Your Name",
    "version": "1.0.0"
  }
}
```

### Step 3: Translate Strings

Translate each string value while keeping the keys intact:

```json
{
  "nav.counter": "Your Translation",
  "nav.games": "Your Translation"
}
```

### Step 4: Test Your Translation

1. Place file in `languages/` folder
2. Restart NYA Core
3. Select your language in Settings
4. Check all views for issues

## Translation Guidelines

### Keep It Concise

UI space is limited. Keep translations short:
- Good: "Guardar"
- Bad: "Guardar los cambios actuales"

### Preserve Placeholders

Some strings have placeholders:
```json
{
  "stats.hits_count": "{count} hits"
}
```

Keep `{count}` in your translation:
```json
{
  "stats.hits_count": "{count} golpes"
}
```

### Maintain Consistency

Use consistent terminology:
- Pick one word for "split" and use it everywhere
- Maintain formal/informal tone throughout

### Handle Plurals

Some strings have plural variants:
```json
{
  "stats.hit": "{count} hit",
  "stats.hits": "{count} hits"
}
```

### Preserve Case

If original is uppercase, keep it uppercase:
```json
{
  "counter.hits": "GOLPES"
}
```

## Fallback Behavior

If a translation is missing:
1. NYA Core uses English fallback
2. Console shows warning
3. Key path displayed if no fallback

## Contributing Translations

### Via GitHub

1. Fork [NYA-Core-Assets](https://github.com/valkyaha/NYA-Core-Assets)
2. Add/update language file
3. Test thoroughly
4. Submit pull request

### Quality Checklist

Before submitting:
- [ ] All keys translated
- [ ] No placeholder errors
- [ ] Correct character encoding (UTF-8)
- [ ] Tested in application
- [ ] Native speaker review (if possible)

## Auto-Update

Language files are auto-updated from GitHub:
1. App checks for updates on startup
2. Compares versions
3. Downloads changed files
4. Reloads language manager

## Complete Key Reference

### Navigation (nav.*)
- `nav.counter` - Counter tab
- `nav.games` - Games tab
- `nav.splits` - Splits tab
- `nav.multirun` - Multi-run tab
- `nav.obs` - OBS tab
- `nav.twitch` - Twitch tab
- `nav.statistics` - Statistics tab
- `nav.settings` - Settings tab
- `nav.about` - About tab

### Counter (counter.*)
- `counter.hits` - Hits label
- `counter.total` - Total label
- `counter.pb` - PB label
- `counter.delta` - Delta label
- `counter.ahead` - Ahead text
- `counter.behind` - Behind text
- `counter.even` - Even text
- `counter.gold` - Gold split indicator

### Timer (timer.*)
- `timer.start` - Start button
- `timer.pause` - Pause button
- `timer.resume` - Resume button
- `timer.stop` - Stop button
- `timer.reset` - Reset button

### Status (status.*)
- `status.idle` - Idle state
- `status.active` - Active state
- `status.completed` - Completed state
- `status.paused` - Paused state
- `status.practice` - Practice mode

### Games (games.*)
- `games.select` - Select game
- `games.preset` - Select preset
- `games.custom` - Custom game
- `games.create` - Create new
- `games.delete` - Delete game

### Splits (splits.*)
- `splits.add` - Add split
- `splits.remove` - Remove split
- `splits.rename` - Rename split
- `splits.reorder` - Reorder splits
- `splits.clear` - Clear all

### Settings (settings.*)
- `settings.general` - General section
- `settings.theme` - Theme section
- `settings.language` - Language section
- `settings.hotkeys` - Hotkeys section
- `settings.data` - Data section
- `settings.reset` - Factory reset

### Common (common.*)
- `common.save` - Save action
- `common.cancel` - Cancel action
- `common.delete` - Delete action
- `common.edit` - Edit action
- `common.confirm` - Confirm action
- `common.close` - Close action
- `common.yes` - Yes
- `common.no` - No

## Troubleshooting

### Characters Not Displaying

- Ensure UTF-8 encoding
- Check font support
- Verify JSON is valid

### Missing Translations

- Check key spelling
- Verify JSON syntax
- Check console for errors

### Wrong Language Loading

- Clear app cache
- Check language file name
- Verify info.id matches filename
