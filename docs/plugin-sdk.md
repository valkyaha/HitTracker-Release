# Plugin SDK

The Plugin SDK allows developers to create dynamic plugins with custom UI components and backend integration. This is different from TOML game plugins - SDK plugins can execute code and provide interactive UI.

## Overview

SDK plugins can:
- Add new views to the sidebar
- Add widgets to existing views
- Read and write application state
- Subscribe to and emit events
- Invoke commands on other plugins
- Include custom HTML/CSS/JavaScript

## Plugin Structure

```
plugins/
└── my-plugin/
    ├── plugin.toml        # Plugin manifest
    └── ui/
        ├── view.html      # Main view component
        ├── styles.css     # Stylesheet
        └── main.js        # JavaScript logic
```

## Plugin Manifest

Create a `plugin.toml` file in your plugin folder:

```toml
[plugin]
id = "my-plugin"
name = "My Plugin"
version = "1.0.0"
api_version = "1"
author = "Your Name"
description = "Description of your plugin"

# UI Configuration
[ui]
enabled = true
entry = "ui/main.js"         # Main JavaScript entry point
styles = ["ui/styles.css"]   # CSS files to load

# Sidebar view
[[ui.views]]
id = "main-view"
name = "My Plugin"           # Display name in sidebar
component = "ui/view.html"   # HTML component
order = 90                   # Position in sidebar (lower = higher)

# Optional: Widget in another view
[[ui.widgets]]
id = "my-widget"
name = "Quick Widget"
component = "ui/widget.html"
target = "counter"           # Target view
position = "bottom"          # Position in target

# Permissions
[permissions.state]
read = ["current_run", "stats"]
write = []

[permissions.events]
subscribe = ["split:completed", "run:reset"]
emit = ["my-plugin:action"]

# Native code (optional)
[native]
enabled = false
```

## JavaScript API

Your plugin JavaScript receives a `host` object with the following API:

### State Access

```javascript
// Read application state
const currentRun = await host.readState('current_run');
const splits = await host.readState('current_run.splits');
const gameName = await host.readState('current_run.game_name');

// Write state (requires permission)
await host.writeState('current_run.splits.0.name', 'New Name');
```

### Events

```javascript
// Subscribe to events
const unsubscribe = host.subscribe('split:completed', (data) => {
    console.log('Split completed:', data);
});

// Emit events
await host.emit('my-plugin:action', {
    action: 'something',
    data: { ... }
});

// Unsubscribe when done
unsubscribe();
```

### Logging

```javascript
host.debug('Debug message');
host.info('Info message');
host.warn('Warning message');
host.error('Error message');
```

### Inter-Plugin Communication

```javascript
// Call another plugin's command
const result = await host.invokeCommand('other-plugin', 'commandName', {
    arg1: 'value'
});
```

## Available State Paths

| Path | Description |
|------|-------------|
| `current_run` | Current run state |
| `current_run.splits` | Array of splits |
| `current_run.game_name` | Current game name |
| `current_run.preset_name` | Current preset name |
| `current_run.total_hits` | Total hits count |
| `current_run.timer` | Timer state |
| `games` | Available games |
| `stats` | Statistics data |
| `config` | Application config |
| `multi_run` | Multi-run state |

## Available Events

| Event | Payload | Description |
|-------|---------|-------------|
| `split:completed` | `{ index, name, hits }` | Split was completed |
| `run:reset` | `{}` | Run was reset |
| `run:completed` | `{ totalHits, time }` | Run was completed |
| `preset:loaded` | `{ game, preset }` | Preset was loaded |
| `hit:added` | `{ splitIndex, hits }` | Hit was added |
| `timer:started` | `{}` | Timer started |
| `timer:paused` | `{}` | Timer paused |
| `timer:stopped` | `{}` | Timer stopped |

## Example Plugin

### plugin.toml
```toml
[plugin]
id = "run-notes"
name = "Run Notes"
version = "1.0.0"
api_version = "1"
author = "Community"
description = "Take notes during your runs"

[ui]
enabled = true
entry = "ui/main.js"
styles = ["ui/styles.css"]

[[ui.views]]
id = "notes-view"
name = "Notes"
component = "ui/view.html"
order = 85

[permissions.state]
read = ["current_run"]
write = []

[permissions.events]
subscribe = ["split:completed"]
emit = ["notes:added"]
```

### ui/view.html
```html
<div class="notes-container">
    <h2>Run Notes</h2>
    <textarea id="notes-input" placeholder="Add a note..."></textarea>
    <button id="add-note">Add Note</button>
    <div id="notes-list"></div>
</div>
```

### ui/styles.css
```css
.notes-container {
    padding: 20px;
}

#notes-input {
    width: 100%;
    height: 80px;
    margin-bottom: 10px;
    background: var(--background-secondary);
    color: var(--text-primary);
    border: 1px solid var(--border-color);
    border-radius: 4px;
    padding: 10px;
}

#add-note {
    background: var(--accent-color);
    color: white;
    border: none;
    padding: 8px 16px;
    border-radius: 4px;
    cursor: pointer;
}

#notes-list {
    margin-top: 20px;
}

.note-item {
    padding: 10px;
    background: var(--background-secondary);
    border-radius: 4px;
    margin-bottom: 8px;
}
```

### ui/main.js
```javascript
// Notes are stored in localStorage
const STORAGE_KEY = 'run-notes';

function loadNotes() {
    const data = localStorage.getItem(STORAGE_KEY);
    return data ? JSON.parse(data) : [];
}

function saveNotes(notes) {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(notes));
}

function renderNotes() {
    const notes = loadNotes();
    const list = document.getElementById('notes-list');
    list.innerHTML = notes.map(note => `
        <div class="note-item">
            <strong>${note.split || 'General'}</strong>: ${note.text}
            <small>${new Date(note.timestamp).toLocaleString()}</small>
        </div>
    `).join('');
}

// Add note button
document.getElementById('add-note').addEventListener('click', async () => {
    const input = document.getElementById('notes-input');
    const text = input.value.trim();

    if (text) {
        const currentRun = await host.readState('current_run');
        const notes = loadNotes();

        notes.unshift({
            text,
            split: currentRun?.splits?.[currentRun.current_split]?.name,
            timestamp: Date.now()
        });

        saveNotes(notes);
        renderNotes();
        input.value = '';

        await host.emit('notes:added', { text });
    }
});

// Subscribe to split completions
host.subscribe('split:completed', (data) => {
    host.info(`Split completed: ${data.name}`);
});

// Initial render
renderNotes();
host.info('Run Notes plugin initialized');
```

## Permissions

Plugins must declare required permissions in their manifest:

### State Permissions
```toml
[permissions.state]
read = ["current_run", "stats"]  # Paths that can be read
write = ["current_run.splits"]   # Paths that can be written
```

### Event Permissions
```toml
[permissions.events]
subscribe = ["split:completed", "run:reset"]  # Events to receive
emit = ["my-plugin:custom"]                   # Events to send
```

### Network Permissions (for native plugins)
```toml
[permissions.network]
allowed_hosts = ["api.example.com"]
```

## Best Practices

1. **Use unique IDs** - Prefix your plugin ID with your name/org
2. **Handle errors** - Wrap async calls in try/catch
3. **Clean up** - Unsubscribe from events when done
4. **Respect permissions** - Only access what you need
5. **Use localStorage** - For plugin-specific persistent data
6. **Follow CSS conventions** - Use CSS variables for theming

## Testing Your Plugin

1. Place your plugin folder in `plugins/` directory
2. Restart NYA Core
3. Check the console for any errors
4. Your plugin view should appear in the sidebar

## Distribution

To distribute your plugin:

1. Create a GitHub repository
2. Include all plugin files
3. Provide installation instructions
4. Users copy the folder to their `plugins/` directory
