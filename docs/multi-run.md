# Multi-Run Mode

Multi-Run mode allows you to track hits across multiple games in a single session. Perfect for marathon runs, multi-game challenges, or "God Run" attempts.

## Overview

Multi-Run combines multiple games into one continuous tracking session:
- Track each game separately
- Aggregate total hits
- Combined timer
- Unified OBS overlays

## Setting Up Multi-Run

### Creating a Multi-Run

1. Go to Multi-Run tab
2. Click "New Multi-Run"
3. Enter a name (e.g., "Soulsborne Marathon")

### Adding Games

1. Click "Add Game"
2. Select from available games
3. Choose preset for each game
4. Repeat for all games

### Game Order

Reorder games by:
- Drag and drop in list
- Use up/down arrows
- Games play in listed order

### Removing Games

- Click X on game card
- Confirm removal
- Adjusts totals automatically

## Running Multi-Run

### Starting

1. Select your Multi-Run
2. Click "Start Multi-Run"
3. First game activates automatically

### During Multi-Run

Controls work as normal:
- Add/undo hits
- Split within current game
- Timer tracks continuously

### Advancing Games

When completing a game:
1. Complete final split of current game
2. Automatically advances to next game
3. Current game marked complete
4. New game splits activate

Or manually advance:
- Click next game tab
- Use Next Game hotkey

### Completing Multi-Run

After final split of final game:
- Multi-Run marked complete
- Total stats saved
- Per-game stats preserved

## Multi-Run Display

### Counter View

Shows:
- Current game name
- Current split within game
- Game hits / Total hits
- Combined timer
- Overall PB comparison

### Game Tabs

Visual tabs show:
- Completed games (checkmark)
- Current game (highlighted)
- Upcoming games (dimmed)

### Progress

Track overall progress:
- Games completed / Total games
- Percentage complete
- Combined hit total

## OBS Integration

### Game Tabs Overlay

URL: `http://localhost:8085/overlay/tabs`

Shows all games as tabs:
- Completed tabs show hit count
- Current tab highlighted
- Upcoming tabs visible

### Combined Counter

URL: `http://localhost:8085/overlay/counter?multirun=true`

Shows:
- Current game + split
- Game hits / Marathon hits
- Combined timer

### Customization

Style game tabs:
```css
.game-tab {
  padding: 8px 16px;
  border-radius: 4px;
}

.game-tab.completed {
  background: var(--success-color);
}

.game-tab.current {
  background: var(--accent-color);
  transform: scale(1.1);
}
```

## Multi-Run Data

### Per-Game Tracking

Each game in Multi-Run tracks:
- Individual split hits
- Game total hits
- Game time
- Game PB comparison

### Aggregate Tracking

Multi-Run overall tracks:
- Combined hit total
- Total elapsed time
- Overall PB
- Completion status

### Personal Bests

PBs work at multiple levels:
- Per-split PB within each game
- Per-game PB totals
- Multi-Run overall PB

## Advanced Features

### Preset Multi-Runs

Save common game combinations:
1. Configure Multi-Run
2. Click "Save as Preset"
3. Load from Presets dropdown

### Game-Specific Settings

Each game can have:
- Different hotkey profiles
- Different timer settings
- Separate autosplitter config

### Pausing Between Games

Option to pause timer between games:
1. Settings > Multi-Run
2. Enable "Pause Between Games"
3. Timer pauses on game completion
4. Resume when ready

## Hotkeys

Multi-Run specific hotkeys:

| Action | Description |
|--------|-------------|
| Next Game | Advance to next game |
| Previous Game | Return to previous game |
| Toggle Multi-Run | Enter/exit Multi-Run mode |

## Statistics

Multi-Run stats tracked:
- Marathon attempts
- Marathon completions
- Best marathon time
- Best marathon hits
- Per-game averages

## Tips

### Preparation

- Test all games before marathon
- Verify all presets correct
- Check autosplitters working
- Prepare backup saves

### During Marathon

- Take breaks between games
- Save frequently
- Keep hydrated
- Track with friends

### Recovery

If you need to stop mid-marathon:
- Progress saves automatically
- Resume from last position
- Or reset individual games

## Example: Soulsborne Marathon

Setup:
1. Dark Souls Remastered - All Bosses
2. Dark Souls II - All Bosses
3. Dark Souls III - All Bosses
4. Bloodborne (via capture) - All Bosses
5. Sekiro - All Bosses
6. Elden Ring - All Remembrances

Tips:
- Enable autosplitter for each
- Use consistent hit criteria
- Plan break points
- Have backup controllers
