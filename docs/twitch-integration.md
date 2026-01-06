# Twitch Integration

NYA Core integrates with Twitch for chat commands, channel point redemptions, and live statistics display.

## Setup

### Connecting Your Account

1. Go to Settings > Twitch
2. Click "Connect with Twitch"
3. Authorize NYA Core in browser
4. Return to app - connected!

### Required Scopes

NYA Core requests:
- `chat:read` - Read chat messages
- `chat:edit` - Send chat messages
- `channel:read:redemptions` - Read channel points
- `channel:manage:redemptions` - Create/manage redemptions

## Chat Commands

### Default Commands

| Command | Response |
|---------|----------|
| `!hits` | Current hit count |
| `!pb` | Personal best |
| `!split` | Current split name |
| `!timer` | Current run time |
| `!boss` | Current boss name |
| `!total` | Total hits this run |
| `!delta` | Comparison to PB |

### Command Customization

Customize command responses:
1. Settings > Twitch > Commands
2. Click command to edit
3. Modify response template
4. Save changes

### Response Variables

Use these in custom responses:

| Variable | Value |
|----------|-------|
| `{hits}` | Current split hits |
| `{total}` | Total run hits |
| `{pb}` | Personal best |
| `{delta}` | PB comparison |
| `{split}` | Current split name |
| `{timer}` | Current time |
| `{game}` | Game name |
| `{preset}` | Preset name |
| `{attempt}` | Attempt number |

Example:
```
!hits response: "{split}: {hits} hits ({delta} vs PB)"
Output: "Vordt: 3 hits (+1 vs PB)"
```

### Custom Commands

Create your own commands:
1. Settings > Twitch > Custom Commands
2. Click "Add Command"
3. Enter trigger (e.g., `!stats`)
4. Enter response template
5. Set cooldown (optional)

### Command Settings

| Option | Description |
|--------|-------------|
| Cooldown | Seconds between uses |
| User Cooldown | Per-user cooldown |
| Mod Only | Only mods can use |
| Enabled | Toggle command on/off |

## Channel Points

### Automatic Redemptions

NYA Core can create channel point rewards:
- Add a hit (troll redemption)
- Remove a hit (nice viewers)
- Show stats
- Play sound

### Setting Up Redemptions

1. Settings > Twitch > Channel Points
2. Click "Create Redemption"
3. Configure:
   - Title
   - Cost
   - Action
   - Cooldown
4. Enable on Twitch

### Redemption Actions

| Action | Effect |
|--------|--------|
| Add Hit | Increases hit counter |
| Remove Hit | Decreases hit counter |
| Show Stats | Posts stats to chat |
| Custom Message | Sends custom message |

### Managing Redemptions

- **Pause** - Temporarily disable
- **Edit** - Modify settings
- **Delete** - Remove completely

## Live Statistics

### Stats Display

Real-time stats in your Twitch panel:
1. Settings > Twitch > Panels
2. Copy panel URL
3. Add as Extension or Panel

### Available Displays

- Current run progress
- PB comparison
- Split list with hits
- Run timer

## Predictions

### Auto Predictions

Create predictions automatically:
- "Will they no-hit this boss?"
- "Over/under X hits?"

### Setup

1. Settings > Twitch > Predictions
2. Enable auto-predictions
3. Configure triggers:
   - On split start
   - On boss approach
   - Manual only

### Prediction Templates

Customize prediction text:
```
Title: "Will {streamer} no-hit {split}?"
Option 1: "Clean!"
Option 2: "They'll get hit"
Duration: 60 seconds
```

## Event Notifications

### Chat Announcements

Auto-announce events:
- New PB achieved
- Run completed
- Milestone hits
- Gold splits

### Configuration

1. Settings > Twitch > Announcements
2. Toggle which events to announce
3. Customize messages

Example:
```
PB Message: "NEW PB! {total} hits (previous: {old_pb})"
```

## Moderator Features

### Mod Commands

Additional commands for moderators:
- `!reset` - Reset current run
- `!sethits X` - Set hit count
- `!setpb X` - Override PB

### Permissions

Configure who can use commands:
- Broadcaster only
- Moderators
- VIPs
- Subscribers
- Everyone

## Bot Settings

### Bot Identity

NYA Core can respond as:
- Your account (default)
- Separate bot account

### Bot Account Setup

1. Create bot Twitch account
2. Settings > Twitch > Bot Account
3. Connect bot account
4. Use bot for responses

## API Integration

### Custom Integrations

Access Twitch data via API:
```javascript
const twitchState = await Tauri.invoke('get_twitch_state');
console.log(twitchState.connected);
console.log(twitchState.channel);
```

### Webhooks

Send data to external services:
1. Settings > Twitch > Webhooks
2. Add webhook URL
3. Select events to send
4. Test connection

## Troubleshooting

### Connection Issues

1. **Not Connecting**
   - Check internet connection
   - Revoke and re-authorize
   - Verify Twitch is online

2. **Commands Not Working**
   - Check command is enabled
   - Verify bot has permissions
   - Check cooldown status

3. **Channel Points Not Syncing**
   - Ensure proper scopes
   - Check Twitch dashboard
   - Reconnect account

### Rate Limits

Twitch rate limits apply:
- 20 messages per 30 seconds
- 100 messages per 30 seconds (mod)
- Channel point limits vary

NYA Core handles rate limiting automatically.

## Privacy

### Data Handling

- OAuth tokens stored locally
- No chat logs saved by default
- Viewer data not collected

### Disconnecting

To fully disconnect:
1. Settings > Twitch > Disconnect
2. Revoke access on Twitch:
   - Twitch > Settings > Connections
   - Remove NYA Core

## Best Practices

### For Streamers

- Test commands before going live
- Set reasonable channel point costs
- Use cooldowns to prevent spam
- Consider mod-only for sensitive commands

### For Community

- Announce new commands
- Create channel point events
- Integrate with other bots
- Engage with predictions
