# CSS Customization

NYA Core OBS overlays are fully customizable using CSS. This guide covers all available options and techniques.

## Built-in Customization

### OBS Settings Panel

Access via Settings > OBS:

| Option | Description |
|--------|-------------|
| Background Color | Main background color |
| Background Opacity | 0-100% transparency |
| Text Color | Primary text color |
| Accent Color | Highlight color |
| Font Family | Font selection |
| Font Size | Base font size |
| Border Radius | Corner rounding |

### Theme Presets

Save and load complete themes:
1. Customize all settings
2. Click "Save Theme"
3. Enter a name
4. Load from dropdown anytime

## CSS Variables

All overlays use CSS custom properties for easy theming:

```css
:root {
  /* Colors */
  --background-primary: #1a1a2e;
  --background-secondary: #16213e;
  --text-primary: #ffffff;
  --text-secondary: #a0a0a0;
  --accent-color: #e94560;
  --success-color: #4ecca3;
  --warning-color: #ffc107;
  --error-color: #ff6b6b;

  /* Typography */
  --font-family: 'Segoe UI', sans-serif;
  --font-size-base: 16px;
  --font-size-large: 24px;
  --font-size-small: 12px;
  --font-weight-normal: 400;
  --font-weight-bold: 700;

  /* Spacing */
  --spacing-xs: 4px;
  --spacing-sm: 8px;
  --spacing-md: 16px;
  --spacing-lg: 24px;
  --spacing-xl: 32px;

  /* Borders */
  --border-radius: 8px;
  --border-color: #2a2a4a;
  --border-width: 1px;

  /* Effects */
  --shadow: 0 4px 6px rgba(0, 0, 0, 0.3);
  --transition: 0.2s ease;
}
```

## Counter Overlay

### Structure

```html
<div class="counter-container">
  <div class="split-name">Boss Name</div>
  <div class="hits-display">
    <span class="hits-value">5</span>
    <span class="hits-label">HITS</span>
  </div>
  <div class="total-display">
    <span class="total-value">25</span>
    <span class="total-label">TOTAL</span>
  </div>
  <div class="timer">12:34.567</div>
  <div class="pb-delta ahead">-3</div>
</div>
```

### Custom Styles

```css
/* Large hit counter */
.hits-value {
  font-size: 72px;
  font-weight: bold;
  color: var(--accent-color);
}

/* Glowing effect when ahead */
.pb-delta.ahead {
  color: #4ecca3;
  text-shadow: 0 0 10px #4ecca3;
}

/* Behind indicator */
.pb-delta.behind {
  color: #ff6b6b;
}

/* Timer styling */
.timer {
  font-family: 'Roboto Mono', monospace;
  font-size: 32px;
  letter-spacing: 2px;
}
```

## Splits Overlay

### Structure

```html
<div class="splits-container">
  <div class="split-item completed">
    <img class="split-image" src="...">
    <span class="split-name">Boss 1</span>
    <span class="split-hits">3</span>
    <span class="split-delta">+1</span>
  </div>
  <div class="split-item current">
    <img class="split-image" src="...">
    <span class="split-name">Boss 2</span>
    <span class="split-hits">2</span>
  </div>
  <div class="split-item upcoming">
    <span class="split-name">Boss 3</span>
  </div>
</div>
```

### Custom Styles

```css
/* Completed splits */
.split-item.completed {
  opacity: 0.7;
  background: rgba(78, 204, 163, 0.1);
}

.split-item.completed .split-name {
  text-decoration: line-through;
}

/* Current split highlight */
.split-item.current {
  background: var(--accent-color);
  transform: scale(1.02);
  box-shadow: 0 0 20px rgba(233, 69, 96, 0.5);
}

/* Upcoming splits */
.split-item.upcoming {
  opacity: 0.5;
}

/* Boss images */
.split-image {
  width: 40px;
  height: 40px;
  border-radius: 4px;
  object-fit: cover;
}

/* Gold split (best segment) */
.split-item.gold .split-delta {
  color: gold;
  font-weight: bold;
}
```

## Progress Bar

### Structure

```html
<div class="progress-container">
  <div class="progress-bar" style="width: 45%"></div>
  <span class="progress-text">45%</span>
</div>
```

### Custom Styles

```css
/* Container */
.progress-container {
  width: 100%;
  height: 24px;
  background: var(--background-secondary);
  border-radius: 12px;
  overflow: hidden;
  position: relative;
}

/* Bar */
.progress-bar {
  height: 100%;
  background: linear-gradient(90deg, #e94560, #0f3460);
  transition: width 0.3s ease;
}

/* Animated stripes */
.progress-bar {
  background-image: repeating-linear-gradient(
    45deg,
    transparent,
    transparent 10px,
    rgba(255,255,255,0.1) 10px,
    rgba(255,255,255,0.1) 20px
  );
  animation: stripes 1s linear infinite;
}

@keyframes stripes {
  0% { background-position: 0 0; }
  100% { background-position: 40px 0; }
}

/* Text overlay */
.progress-text {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  font-weight: bold;
  text-shadow: 0 1px 2px rgba(0,0,0,0.5);
}
```

## Game Tabs (Multi-Run)

### Structure

```html
<div class="tabs-container">
  <div class="game-tab completed">
    <span class="game-name">DS3</span>
    <span class="game-hits">45</span>
  </div>
  <div class="game-tab current">
    <span class="game-name">Sekiro</span>
    <span class="game-hits">12</span>
  </div>
  <div class="game-tab upcoming">
    <span class="game-name">ER</span>
  </div>
</div>
```

### Custom Styles

```css
/* Horizontal layout */
.tabs-container {
  display: flex;
  gap: 8px;
}

/* Tab styling */
.game-tab {
  padding: 8px 16px;
  border-radius: 20px;
  background: var(--background-secondary);
}

.game-tab.current {
  background: var(--accent-color);
  transform: scale(1.1);
}

.game-tab.completed {
  background: var(--success-color);
}
```

## Animation Examples

### Fade In

```css
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(-10px); }
  to { opacity: 1; transform: translateY(0); }
}

.split-item {
  animation: fadeIn 0.3s ease;
}
```

### Pulse on Update

```css
@keyframes pulse {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.1); }
}

.hits-value.updated {
  animation: pulse 0.3s ease;
}
```

### Glow Effect

```css
@keyframes glow {
  0%, 100% { box-shadow: 0 0 5px var(--accent-color); }
  50% { box-shadow: 0 0 20px var(--accent-color); }
}

.counter-container.active {
  animation: glow 2s infinite;
}
```

### Slide In

```css
@keyframes slideIn {
  from { transform: translateX(-100%); opacity: 0; }
  to { transform: translateX(0); opacity: 1; }
}

.split-item {
  animation: slideIn 0.5s ease;
}
```

## Responsive Design

### Container Queries

```css
.counter-container {
  container-type: inline-size;
}

@container (max-width: 300px) {
  .hits-value {
    font-size: 48px;
  }
}
```

### Flexible Layout

```css
.splits-container {
  display: flex;
  flex-direction: column;
  max-height: 100vh;
  overflow-y: auto;
}

.split-item {
  flex-shrink: 0;
}
```

## Transparency & Chroma Key

### Transparent Background

```css
body {
  background: transparent !important;
}

.counter-container {
  background: rgba(26, 26, 46, 0.8);
}
```

### Green Screen

```css
body {
  background: #00ff00 !important;
}
```

## Font Integration

### Google Fonts

Add to your custom CSS:

```css
@import url('https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap');

.counter-container {
  font-family: 'Press Start 2P', cursive;
}
```

### Popular Fonts for Streaming

- **Roboto Mono** - Clean monospace
- **Press Start 2P** - Retro pixel
- **Bebas Neue** - Bold display
- **Orbitron** - Sci-fi
- **VT323** - Terminal style

## URL Parameters

Pass CSS via URL:

```
http://localhost:8085/overlay/counter?css=.hits-value{color:red}
```

URL-encode special characters:
- Space → `%20`
- `#` → `%23`
- `:` → `%3A`

## Debugging

### Browser DevTools

1. Open overlay URL in browser
2. Press F12 for DevTools
3. Inspect elements
4. Test CSS changes live

### Common Issues

1. **Styles not applying**
   - Check specificity
   - Use `!important` if needed
   - Verify selector matches

2. **Fonts not loading**
   - Check network tab
   - Verify font URL
   - Use fallback fonts

3. **Animations stuttering**
   - Use `transform` and `opacity`
   - Avoid animating `width`/`height`
   - Use `will-change` hint

## Examples

### Minimal Dark Theme

```css
:root {
  --background-primary: #000000;
  --text-primary: #ffffff;
  --accent-color: #ff0000;
}

.counter-container {
  background: transparent;
  border: 2px solid var(--accent-color);
}
```

### Retro Gaming Theme

```css
@import url('https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap');

:root {
  --font-family: 'Press Start 2P', cursive;
  --background-primary: #000000;
  --text-primary: #00ff00;
  --accent-color: #ff00ff;
}

.counter-container {
  image-rendering: pixelated;
  border: 4px solid var(--text-primary);
}
```

### Glassmorphism Theme

```css
.counter-container {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.2);
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
}
```
