# Installation

This guide covers installing and setting up NYA Core on your system.

## System Requirements

### Minimum
- **OS:** Windows 10 (64-bit)
- **RAM:** 4 GB
- **Storage:** 100 MB
- **Display:** 1280x720

### Recommended
- **OS:** Windows 11 (64-bit)
- **RAM:** 8 GB
- **Storage:** 500 MB (for plugins and assets)
- **Display:** 1920x1080

## Download Options

### Installer (Recommended)

1. Download the latest `.msi` installer from [Releases](https://github.com/valkyaha/HitTracker-Release/releases)
2. Run the installer
3. Follow the setup wizard
4. Launch NYA Core from Start Menu

### Portable Version

1. Download the `.zip` portable archive from Releases
2. Extract to your preferred location
3. Run `NYA Core.exe`

The portable version includes:
- Pre-bundled plugins
- Language files
- All dependencies

## First Launch

### Initial Setup

On first launch, NYA Core will:
1. Create configuration directory at `%APPDATA%/NYA Core`
2. Download latest game plugins
3. Download language files
4. Initialize default settings

### Configuration Folder Structure

```
%APPDATA%/NYA Core/
├── config.json           # Application settings
├── games/                # Saved game data
├── plugins/              # Downloaded game plugins
├── languages/            # Translation files
├── themes/               # Custom OBS themes
└── logs/                 # Application logs
```

## Updating

### Auto-Update

NYA Core checks for updates on startup:
1. If update available, notification appears
2. Click "Update Now" to download
3. App restarts automatically

### Manual Update

1. Download latest version from Releases
2. Close NYA Core
3. Run new installer (overwrites old version)
4. Or replace portable folder contents

### Plugin Updates

Game plugins update independently:
1. Go to Settings > About
2. Click "Check for Updates"
3. Plugins download automatically

## Uninstallation

### Installer Version

1. Open Windows Settings > Apps
2. Find "NYA Core"
3. Click Uninstall

### Portable Version

1. Delete the application folder
2. Optionally delete `%APPDATA%/NYA Core`

### Complete Removal

To remove all data:
1. Uninstall the application
2. Delete `%APPDATA%/NYA Core`

## Troubleshooting Installation

### App Won't Start

1. **Missing Visual C++ Runtime**
   - Download from Microsoft
   - Install both x64 and x86 versions

2. **Antivirus Blocking**
   - Add exception for NYA Core
   - Memory reading may trigger false positives

3. **Windows Defender SmartScreen**
   - Click "More info"
   - Click "Run anyway"

### Plugins Not Loading

1. Check internet connection
2. Verify firewall allows NYA Core
3. Try Settings > About > "Check for Updates"

### Configuration Reset

To reset to defaults:
1. Close NYA Core
2. Delete `%APPDATA%/NYA Core/config.json`
3. Restart application

## Migration from Previous Versions

### From HitCounter

1. Export your games via Settings > Data > Export
2. Install NYA Core
3. Import via Settings > Data > Import
4. Verify all data transferred

### Data Compatibility

NYA Core can import:
- HitCounter v2.x exports
- LiveSplit splits (`.lss`)
- Standard CSV format

## Network Requirements

NYA Core requires internet for:
- Auto-updates
- Plugin downloads
- Twitch integration
- OBS overlay (localhost only)

### Firewall Ports

| Port | Purpose |
|------|---------|
| 8085 | OBS HTTP server (localhost) |
| 443  | GitHub API (updates) |
| 443  | Twitch API |
