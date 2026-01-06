# OBS Integration

NYA Core provides professional streaming overlays through a built-in HTTP server. Add browser sources in OBS to display real-time hit tracking data.

## Quick Start

1. Start the OBS server from Settings > OBS
2. Copy the overlay URL (e.g., `http://localhost:8085/overlay/counter`)
3. Add a Browser Source in OBS
4. Paste the URL and set dimensions
5. Customize appearance in NYA Core

## Available Overlays

| Overlay | URL | Description |
|---------|-----|-------------|
| Counter | `/overlay/counter` | Current split hits, total hits, timer |
| Splits | `/overlay/splits` | Boss checklist with hits and status |
| Progress | `/overlay/progress` | Visual completion bar |
| Game Tabs | `/overlay/tabs` | Multi-run game selector |
| Status | `/overlay/status` | Game and preset name |
| Composite | `/overlay/composite` | All-in-one customizable layout |
| Preview | `/overlay/preview` | All overlays for testing |

## Server Configuration

### Default Settings
- **Port:** 8085 (configurable)
- **Update Rate:** 100ms
- **Address:** `http://localhost:8085`

### Changing the Port

1. Go to Settings > OBS
2. Enter a new port number
3. Restart the OBS server
4. Update your OBS browser sources

## Overlay Customization

### Counter Overlay

Options:
- Background color and opacity
- Text color and font
- Timer format (HH:MM:SS or MM:SS)
- Show/hide total hits
- Show/hide PB delta
- Border radius and shadows

### Splits Overlay

Options:
- Layout (vertical/horizontal)
- Show/hide boss images
- Image size and position
- Completed split styling
- Current split highlight
- Max visible splits

### Progress Bar

Options:
- Bar color and background
- Height and border radius
- Show percentage text
- Animation duration

### Composite Overlay

The composite overlay allows you to:
- Drag elements to position them
- Resize elements
- Lock/unlock positions
- Enable snap-to-grid
- Show alignment guides

## CSS Customization

Each overlay accepts custom CSS via URL parameters:

```
http://localhost:8085/overlay/counter?css=background:transparent
```

Or use the built-in CSS editor in Settings > OBS.

See [CSS Customization Guide](css-customization.md) for detailed styling options.

## OBS Browser Source Setup

### Recommended Settings

| Setting | Value |
|---------|-------|
| Width | 400-800px |
| Height | Varies by overlay |
| FPS | 30 |
| Custom CSS | (optional) |
| Shutdown when not visible | Disabled |
| Refresh when scene active | Enabled |

### Example Dimensions

| Overlay | Recommended Size |
|---------|------------------|
| Counter | 400x150 |
| Splits | 300x600 |
| Progress | 400x30 |
| Status | 300x60 |
| Composite | 1920x1080 |

## Data Format

The server provides JSON data at `/api/state`:

```json
{
  "currentSplitIndex": 2,
  "currentSplitName": "Vordt",
  "currentSplitHits": 3,
  "totalHits": 15,
  "timer": "12:34.567",
  "timerState": "running",
  "status": "active",
  "splits": [
    {
      "name": "Iudex Gundyr",
      "hits": 5,
      "pbHits": 3,
      "completed": true,
      "current": false,
      "imageUrl": "..."
    },
    {
      "name": "Vordt",
      "hits": 3,
      "pbHits": 2,
      "completed": false,
      "current": true,
      "imageUrl": "..."
    }
  ],
  "gameName": "Dark Souls III",
  "presetName": "All Bosses",
  "pbTotalHits": 45,
  "pbDelta": "+3",
  "aheadBehind": "behind",
  "multiRun": {
    "active": false,
    "games": [],
    "currentIndex": 0,
    "totalHits": 0,
    "totalTime": "00:00.000"
  },
  "practiceMode": false
}
```

## Theme Presets

Save and load theme configurations:

1. Customize your overlay appearance
2. Click "Save Theme" and enter a name
3. Load saved themes from the dropdown
4. Share theme JSON files with others

### Theme File Format

```json
{
  "name": "Dark Theme",
  "backgroundColor": "#1a1a2e",
  "textColor": "#ffffff",
  "accentColor": "#e94560",
  "fontFamily": "Roboto Mono",
  "fontSize": 16,
  "borderRadius": 8,
  "opacity": 0.95
}
```

## Troubleshooting

### Overlay Not Loading

1. Check if OBS server is running (green indicator)
2. Verify the port is not in use
3. Try refreshing the browser source
4. Check firewall settings

### Data Not Updating

1. Ensure NYA Core is the active window initially
2. Check browser source "Refresh" setting
3. Verify WebSocket connection in browser console

### Styling Issues

1. Clear browser source cache
2. Reload the source
3. Check for CSS syntax errors
4. Verify custom CSS is being applied

## Advanced Usage

### Multiple Monitors

You can run multiple browser sources with different overlays:
- Counter on gaming monitor
- Splits on stream preview
- Composite on stream output

### Custom Overlay Development

Create your own overlays by:
1. Fetching data from `/api/state`
2. Building custom HTML/CSS/JS
3. Hosting locally or embedding in OBS

### Webhook Integration

The server supports webhook callbacks for:
- Split completions
- Run completions
- PB achievements

Configure webhooks in Settings > Advanced.
