# API Reference

NYA Core provides multiple APIs for integration and customization.

## HTTP API

The OBS server exposes HTTP endpoints for external access.

### Base URL

```
http://localhost:8085
```

Port configurable in Settings > OBS.

### Endpoints

#### GET /api/state

Returns complete application state.

**Response:**
```json
{
  "currentSplitIndex": 2,
  "currentSplitName": "Vordt",
  "currentSplitHits": 3,
  "totalHits": 15,
  "timer": "12:34.567",
  "timerState": "running",
  "status": "active",
  "splits": [...],
  "gameName": "Dark Souls III",
  "presetName": "All Bosses",
  "pbTotalHits": 45,
  "pbDelta": "+3",
  "aheadBehind": "behind",
  "multiRun": {...},
  "practiceMode": false
}
```

#### GET /api/splits

Returns split list with details.

**Response:**
```json
{
  "splits": [
    {
      "id": "iudex_gundyr",
      "name": "Iudex Gundyr",
      "hits": 5,
      "pbHits": 3,
      "goldHits": 2,
      "completed": true,
      "current": false,
      "imageUrl": "asset://..."
    }
  ]
}
```

#### GET /api/timer

Returns timer state.

**Response:**
```json
{
  "elapsed": 754567,
  "formatted": "12:34.567",
  "state": "running",
  "startTime": 1699123456789
}
```

#### GET /api/game

Returns current game info.

**Response:**
```json
{
  "id": "dark-souls-3",
  "name": "Dark Souls III",
  "preset": "all_bosses",
  "presetName": "All Bosses"
}
```

#### POST /api/action

Trigger actions remotely.

**Request:**
```json
{
  "action": "hit",
  "value": 1
}
```

**Available Actions:**
| Action | Value | Description |
|--------|-------|-------------|
| `hit` | number | Add/remove hits |
| `split` | - | Complete current split |
| `reset` | - | Reset run |
| `timer_start` | - | Start timer |
| `timer_pause` | - | Pause timer |
| `timer_reset` | - | Reset timer |

### WebSocket

Real-time updates via WebSocket at `ws://localhost:8085/ws`.

**Events:**
```json
{
  "type": "state_update",
  "data": { ... }
}
```

Event types:
- `state_update` - Full state changed
- `hit` - Hit count changed
- `split` - Split changed
- `timer` - Timer state changed
- `reset` - Run reset

## Tauri Commands

Internal commands for frontend communication.

### Counter Commands

#### get_current_state
```javascript
const state = await Tauri.invoke('get_current_state');
```

#### add_hit
```javascript
await Tauri.invoke('add_hit', { count: 1 });
```

#### undo_hit
```javascript
await Tauri.invoke('undo_hit', { count: 1 });
```

#### split
```javascript
await Tauri.invoke('split');
```

#### previous_split
```javascript
await Tauri.invoke('previous_split');
```

#### reset_run
```javascript
await Tauri.invoke('reset_run');
```

### Game Commands

#### list_games
```javascript
const games = await Tauri.invoke('list_games');
```

#### select_game
```javascript
await Tauri.invoke('select_game', { gameId: 'dark-souls-3' });
```

#### select_preset
```javascript
await Tauri.invoke('select_preset', { presetId: 'all_bosses' });
```

#### get_game_details
```javascript
const details = await Tauri.invoke('get_game_details', {
  gameId: 'dark-souls-3'
});
```

### Split Commands

#### get_splits
```javascript
const splits = await Tauri.invoke('get_splits');
```

#### add_split
```javascript
await Tauri.invoke('add_split', {
  name: 'New Boss',
  index: 5
});
```

#### remove_split
```javascript
await Tauri.invoke('remove_split', { index: 5 });
```

#### rename_split
```javascript
await Tauri.invoke('rename_split', {
  index: 5,
  name: 'Renamed Boss'
});
```

#### reorder_splits
```javascript
await Tauri.invoke('reorder_splits', {
  fromIndex: 3,
  toIndex: 5
});
```

### Timer Commands

#### start_timer
```javascript
await Tauri.invoke('start_timer');
```

#### pause_timer
```javascript
await Tauri.invoke('pause_timer');
```

#### reset_timer
```javascript
await Tauri.invoke('reset_timer');
```

#### get_timer_state
```javascript
const timer = await Tauri.invoke('get_timer_state');
```

### Settings Commands

#### get_settings
```javascript
const settings = await Tauri.invoke('get_settings');
```

#### update_settings
```javascript
await Tauri.invoke('update_settings', {
  settings: { ... }
});
```

### Statistics Commands

#### get_game_statistics
```javascript
const stats = await Tauri.invoke('get_game_statistics', {
  gameId: 'dark-souls-3'
});
```

#### get_split_statistics
```javascript
const stats = await Tauri.invoke('get_split_statistics', {
  gameId: 'dark-souls-3',
  splitId: 'vordt'
});
```

## Plugin API

SDK plugins receive a `PluginHost` instance for communication.

### PluginHost Methods

#### sendEvent
```javascript
pluginHost.sendEvent('my_event', { data: 'value' });
```

#### onEvent
```javascript
pluginHost.onEvent('game_selected', (data) => {
  console.log('Game selected:', data.gameId);
});
```

#### getState
```javascript
const state = await pluginHost.getState();
```

#### callCommand
```javascript
const result = await pluginHost.callCommand('custom_command', {
  arg: 'value'
});
```

#### getStorage
```javascript
const value = await pluginHost.getStorage('key');
```

#### setStorage
```javascript
await pluginHost.setStorage('key', 'value');
```

### Events

Available events for plugins:

| Event | Data | Description |
|-------|------|-------------|
| `hit` | `{ count, total }` | Hit added/removed |
| `split` | `{ index, name }` | Split completed |
| `reset` | `{}` | Run reset |
| `timer_start` | `{ time }` | Timer started |
| `timer_pause` | `{ time }` | Timer paused |
| `game_selected` | `{ gameId }` | Game changed |
| `preset_selected` | `{ presetId }` | Preset changed |
| `state_update` | `{ state }` | Full state update |

## Webhook API

Configure webhooks for external integrations.

### Configuration

Settings > Advanced > Webhooks

Add webhook URL and select events.

### Payload Format

```json
{
  "event": "split_completed",
  "timestamp": 1699123456789,
  "data": {
    "splitIndex": 3,
    "splitName": "Vordt",
    "hits": 2,
    "totalHits": 15
  }
}
```

### Available Events

- `run_started`
- `split_completed`
- `run_completed`
- `run_reset`
- `pb_achieved`
- `gold_achieved`

## Error Handling

### HTTP Errors

| Code | Meaning |
|------|---------|
| 200 | Success |
| 400 | Bad request |
| 404 | Not found |
| 500 | Internal error |

### Tauri Errors

Commands throw errors on failure:
```javascript
try {
  await Tauri.invoke('select_game', { gameId: 'invalid' });
} catch (error) {
  console.error('Failed:', error);
}
```

## Rate Limiting

### HTTP API
- 100 requests/second
- 1000 requests/minute

### WebSocket
- 50 messages/second
- Automatic reconnection

## Authentication

Currently no authentication required for local APIs.

For remote access (future):
- API key authentication
- OAuth for Twitch integration
