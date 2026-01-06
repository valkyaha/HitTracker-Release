# Hit Tracking

NYA Core provides comprehensive hit tracking for challenge runs. This guide covers all tracking features.

## Core Concepts

### Hits
A "hit" represents damage taken during a run. The goal is typically to minimize hits or achieve a "no-hit" run.

### Splits
Splits divide your run into sections, usually one per boss or major checkpoint. Each split tracks:
- Current hits for this attempt
- Personal best (PB) hits
- Best ever (gold) hits

### Personal Best (PB)
Your best completed run. When you finish a run with fewer total hits than your PB, it becomes the new PB.

### Gold Splits
The best segment for each split across all attempts, regardless of whether the run was completed.

## Counter View

### Main Display

The counter shows:
- **Current Split Name** - Which boss/section you're on
- **Split Hits** - Hits taken on current split
- **Total Hits** - Cumulative hits for the run
- **Timer** - Elapsed time (optional)
- **PB Delta** - How you compare to your PB (+3 behind, -2 ahead)

### Controls

| Action | Default Hotkey | Description |
|--------|---------------|-------------|
| Add Hit | Numpad + | Increment hit counter |
| Undo Hit | Numpad - | Decrement hit counter |
| Split | Numpad Enter | Complete split, move to next |
| Previous Split | Numpad * | Go back one split |
| Reset | Numpad 0 | Reset run to beginning |

## Games and Presets

### Selecting a Game

1. Go to Games tab
2. Browse available games
3. Click to select

Games are loaded from plugins and provide:
- Boss lists
- Speedrun presets
- Autosplitter support

### Presets

Presets are pre-configured split lists for common categories:
- **Any%** - Minimum required bosses
- **All Bosses** - All main game bosses
- **All Bosses + DLC** - Including DLC content
- **Custom** - User-defined

### Custom Games

Create your own game:
1. Go to Games > Custom
2. Click "New Game"
3. Enter game name
4. Add splits manually

## Split Management

### Adding Splits

1. Go to Splits tab
2. Click "Add Split"
3. Enter split name
4. Optionally add image

### Editing Splits

- **Rename** - Click split name to edit
- **Reorder** - Drag and drop
- **Delete** - Click X button

### Split Images

Add images to splits:
1. Select a split
2. Click image area
3. Choose image file
4. Image displays in overlays

Supported formats: PNG, JPG, GIF, WebP

### Importing Splits

Import from external sources:
1. Splits tab > Import
2. Select source:
   - LiveSplit file (.lss)
   - CSV file
   - Preset

## Tracking Modes

### Normal Mode

Standard hit tracking:
- Hits count up from 0
- Splits track per-section hits
- Total accumulates across splits

### Practice Mode

For practicing without affecting stats:
- Toggle via button or hotkey
- Hits don't save to PB
- Golds don't update
- Timer pauses (optional)

### Reverse Mode

Count down instead of up:
- Set starting hit count
- Each hit decreases counter
- Useful for "hit limit" challenges

## Run Lifecycle

### Starting a Run

A run starts when you:
- Add your first hit
- Complete first split
- Start the timer

### During a Run

Track progress:
- Add/undo hits as needed
- Split when completing sections
- Watch PB delta

### Completing a Run

After final split:
- Run marked complete
- If better than PB, new PB saved
- Statistics updated
- Gold splits updated

### Resetting

Reset clears current progress:
- Hits reset to 0
- Timer resets
- Returns to first split
- Current attempt not saved

## Timer

### Timer Modes

- **Auto** - Starts on first action
- **Manual** - Start/pause manually
- **External** - Use with LiveSplit

### Timer Controls

| Action | Hotkey |
|--------|--------|
| Start/Resume | F1 |
| Pause | F2 |
| Reset | F3 |

### Timer Format

Configure display format:
- `HH:MM:SS` - Hours:Minutes:Seconds
- `MM:SS.mmm` - Minutes:Seconds.Milliseconds
- `SS.mmm` - Seconds only

## Statistics Integration

Every run contributes to statistics:
- Attempt count
- Completion rate
- Average hits per split
- Time tracking

See [Statistics](statistics.md) for details.

## Hotkey Configuration

Customize all hotkeys in Settings > Hotkeys:
1. Click the action
2. Press desired key combination
3. Save changes

See [Hotkeys](hotkeys.md) for complete reference.

## Tips for Hit Tracking

### Accuracy

- Use dedicated hotkey device if possible
- Practice hotkey muscle memory
- Consider using foot pedal for splits

### Recovery

- Undo button for accidental hits
- Previous split if you split early
- Reset if run is lost

### Consistency

- Always track the same way
- Define clear hit criteria
- Be honest with tracking
