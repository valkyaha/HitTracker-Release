# Statistics

NYA Core tracks comprehensive statistics for all your runs. This guide covers available metrics and how to use them.

## Overview

Statistics are tracked automatically for:
- Individual splits
- Complete runs
- Games/presets
- Multi-runs
- Overall usage

## Accessing Statistics

1. Go to Statistics tab
2. Select game/preset
3. View detailed breakdowns

## Run Statistics

### Attempt Tracking

- **Total Attempts** - All run starts
- **Completed Runs** - Runs finished to the end
- **Completion Rate** - Percentage of completed runs
- **Current Streak** - Consecutive completions

### Hit Metrics

| Metric | Description |
|--------|-------------|
| Personal Best | Lowest total hits in completed run |
| Average Hits | Mean hits across completed runs |
| Median Hits | Middle value of all completed runs |
| Worst Run | Highest hits in completed run |
| Total Hits | Sum of all hits ever taken |

### Time Metrics

| Metric | Description |
|--------|-------------|
| Best Time | Fastest completed run |
| Average Time | Mean completion time |
| Total Time | Cumulative playtime |
| Last Session | Most recent play session |

## Split Statistics

Each split tracks individually:

### Performance

- **Current Hits** - This attempt
- **PB Hits** - Best in a completed run
- **Gold Hits** - Best ever (any attempt)
- **Average Hits** - Mean across attempts

### Comparison

| Delta | Meaning |
|-------|---------|
| -X (Green) | X hits ahead of PB |
| +X (Red) | X hits behind PB |
| 0 (White) | On pace with PB |
| Gold | New best segment |

### History

View per-split history:
- Last 10 attempts
- Trend over time
- Improvement rate

## Game Statistics

Aggregate stats per game/preset:

### Overview

- Total attempts across all time
- Completion count and rate
- Current and best streaks
- First tracked date

### Progression

- PB history over time
- Improvement chart
- Milestone achievements

### Comparison

Compare performance across:
- Different presets
- Different time periods
- Before/after practice

## Charts and Graphs

### Hit Distribution

Histogram showing:
- X-axis: Hit count ranges
- Y-axis: Number of runs
- Distribution of completion hits

### Progress Over Time

Line graph showing:
- PB improvements
- Average trend
- Completion rate over time

### Split Comparison

Bar chart comparing:
- Your average vs PB
- Split-by-split breakdown
- Identifying problem areas

## Session Statistics

Per-session tracking:

| Metric | Description |
|--------|-------------|
| Session Attempts | Runs started this session |
| Session Completions | Runs finished this session |
| Session Time | Total time played today |
| Session Hits | Total hits this session |
| Best This Session | Best run of the session |

## Exporting Data

### Export Options

1. Go to Statistics tab
2. Click Export
3. Select format:
   - CSV (spreadsheet)
   - JSON (raw data)
   - PDF (formatted report)

### Export Contents

- All attempts with timestamps
- Per-split breakdowns
- Aggregate statistics
- Comparison data

### Automation

Export via command line:
```bash
nyacore --export-stats "game-id" --format csv --output stats.csv
```

## Data Management

### Clearing Statistics

1. Settings > Data
2. Select game/preset
3. Click "Clear Statistics"
4. Confirm action

This removes:
- Attempt history
- PB data
- All time-based stats

But preserves:
- Split configuration
- Game settings
- Current run

### Backup

Statistics backed up in:
- Auto-save every 5 minutes
- On application close
- Before major actions

Backup location: `%APPDATA%/NYA Core/backups/`

### Restore

1. Settings > Data
2. Click "Restore Backup"
3. Select backup file
4. Confirm restore

## Privacy

### Local Storage

All statistics stored locally:
- No cloud sync by default
- No data sent to servers
- Full user control

### Optional Sharing

If you choose to share:
- Export and share files manually
- Or enable Twitch integration for live stats

## Analysis Tips

### Identifying Weak Splits

1. Look at average hits per split
2. Compare to your PB for that split
3. Focus practice on high-variance splits

### Tracking Improvement

1. Compare weekly averages
2. Note PB frequency
3. Watch completion rate trend

### Setting Goals

Based on statistics:
1. Current average: 45 hits
2. PB: 32 hits
3. Goal: Consistent sub-40 runs

## Achievements

Unlock achievements based on stats:

| Achievement | Requirement |
|-------------|-------------|
| First Blood | Complete first run |
| Consistent | 10 completions in a row |
| Perfectionist | No-hit a split |
| Marathon | 100+ total completions |
| Dedicated | 100+ hours tracked |

## API Access

For advanced users, access stats via API:

```javascript
// Get current game stats
const stats = await Tauri.invoke('get_game_statistics', {
  gameId: 'dark-souls-3'
});

console.log(stats.personalBest);
console.log(stats.attempts);
```

See [API Reference](api-reference.md) for complete documentation.
