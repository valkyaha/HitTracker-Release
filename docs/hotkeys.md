# Hotkeys

NYA Core supports fully customizable global hotkeys that work even when the app is in the background.

## Default Hotkeys

### Counter Controls

| Action | Default Key | Description |
|--------|-------------|-------------|
| Add Hit | Numpad + | Increment hit counter |
| Undo Hit | Numpad - | Decrement hit counter |
| Split | Numpad Enter | Complete current split |
| Previous Split | Numpad * | Go back one split |
| Reset | Numpad 0 | Reset run to beginning |

### Timer Controls

| Action | Default Key | Description |
|--------|-------------|-------------|
| Start Timer | F1 | Start/resume timer |
| Pause Timer | F2 | Pause timer |
| Reset Timer | F3 | Reset timer to zero |

### Mode Toggles

| Action | Default Key | Description |
|--------|-------------|-------------|
| Toggle Practice | F5 | Enter/exit practice mode |
| Toggle Overlay | F6 | Show/hide OBS overlay |

## Configuring Hotkeys

### Accessing Settings

1. Go to Settings > Hotkeys
2. View all available actions
3. Click an action to modify

### Setting a Hotkey

1. Click the hotkey field
2. Press desired key/combination
3. Key displays in field
4. Click "Save" to confirm

### Clearing a Hotkey

1. Click the hotkey field
2. Press Escape or Backspace
3. Field shows "Not Set"
4. Save changes

## Supported Keys

### Standard Keys

- All letter keys (A-Z)
- All number keys (0-9)
- Function keys (F1-F24)
- Arrow keys
- Home, End, Page Up, Page Down
- Insert, Delete
- Escape, Tab, Space
- Enter, Backspace

### Numpad Keys

- Numpad 0-9
- Numpad +, -, *, /
- Numpad Enter
- Num Lock

### Modifier Combinations

Combine with modifiers:
- Ctrl + Key
- Alt + Key
- Shift + Key
- Ctrl + Shift + Key
- Ctrl + Alt + Key
- Any combination

### Special Keys

- Media keys (Play, Pause, Next, Previous)
- Mouse buttons (Mouse4, Mouse5)
- Scroll wheel (with limitations)

## Global Hotkeys

### How They Work

NYA Core registers global hotkeys with the operating system:
- Work when any app is focused
- Work when game is running
- Work with fullscreen games
- Low latency response

### Conflicts

If a hotkey conflicts:
- Warning displayed
- Existing hotkey takes priority
- Choose different key

Common conflicts:
- Windows shortcuts (Win + X)
- Game hotkeys
- Other apps (Discord, OBS)

## Hotkey Profiles

### Creating Profiles

Save different configurations:
1. Settings > Hotkeys
2. Configure your hotkeys
3. Click "Save Profile"
4. Enter profile name

### Switching Profiles

1. Settings > Hotkeys
2. Select profile from dropdown
3. Hotkeys update immediately

### Per-Game Profiles

Associate profiles with games:
1. Games tab > Select game
2. Settings > Hotkey Profile
3. Choose profile
4. Auto-switches on game load

## Advanced Options

### Repeat Delay

Configure key repeat behavior:
1. Settings > Hotkeys > Advanced
2. Set repeat delay (ms)
3. Set repeat rate (ms)

Default: No repeat (single press only)

### Key Debounce

Prevent accidental double-presses:
- Default: 50ms debounce
- Adjustable in settings
- Per-key configuration

### Modifier Behavior

Options for modifier keys:
- **Strict** - Exact combo required
- **Flexible** - Extra modifiers allowed

## Foot Pedal / External Devices

### USB Devices

NYA Core works with:
- USB foot pedals
- Stream decks
- Gaming keypads
- Macro keyboards

### Configuration

1. Configure device to send keystrokes
2. Set those keys as NYA Core hotkeys
3. Works seamlessly

### Recommended Devices

- **Foot Pedal** - For hands-free splits
- **Stream Deck** - Visual buttons
- **Elgato Pedal** - Multi-action foot control

## Troubleshooting

### Hotkeys Not Working

1. **Check Global Registration**
   - Settings > Hotkeys > Status
   - Should show "Registered"
   - If failed, key may be in use

2. **Conflicts**
   - Close other apps temporarily
   - Test in isolation
   - Choose different key

3. **Focus Issues**
   - Ensure NYA Core is running
   - Check system tray
   - Restart application

4. **Elevated Permissions**
   - Some games require admin
   - Run NYA Core as admin
   - Or run game without admin

### Keys Not Detected

1. **Keyboard Layout**
   - Verify correct layout selected
   - Some keys differ by layout
   - Use scan codes if needed

2. **Driver Issues**
   - Update keyboard drivers
   - Test in other apps
   - Try different USB port

### High Latency

1. **Reduce Background Apps**
   - Close unnecessary programs
   - Check CPU usage
   - Disable antivirus temporarily

2. **Adjust Settings**
   - Lower debounce time
   - Disable repeat
   - Update NYA Core

## Best Practices

### Key Selection

- Use keys not used by your game
- Avoid common system shortcuts
- Consider ergonomics
- Numpad works well for most

### Muscle Memory

- Practice hotkeys before runs
- Keep consistent across games
- Use physical device labels
- Start simple, add complexity

### Backup

Export hotkey configuration:
1. Settings > Hotkeys > Export
2. Saves to JSON file
3. Import on new installations

## Complete Action List

All configurable actions:

### Core Actions
- Add Hit
- Undo Hit
- Split
- Previous Split
- Reset Run

### Timer Actions
- Start Timer
- Pause Timer
- Reset Timer
- Toggle Timer

### Mode Actions
- Toggle Practice Mode
- Exit Practice Mode
- Toggle Overlay

### Navigation
- Next Split (no hit)
- Previous Split
- First Split
- Last Split

### Multi-Run
- Next Game
- Previous Game
- Toggle Multi-Run

### Quick Actions
- Quick Save
- Quick Load
- Undo All (reset hits, keep position)
