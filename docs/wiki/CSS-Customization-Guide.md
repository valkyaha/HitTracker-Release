# CSS Customization Guide

This guide covers all CSS classes, variables, and customization options available for NYA Core OBS overlays.

## Table of Contents
- [Getting Started](#getting-started)
- [Using Custom Images](#using-custom-images)
- [Splits Overlay](#splits-overlay)
- [Hit Counter Overlay](#hit-counter-overlay)
- [Progress Bar Overlay](#progress-bar-overlay)
- [Game Tabs Overlay](#game-tabs-overlay)
- [Animation Classes](#animation-classes)
- [Tips & Tricks](#tips--tricks)

---

## Getting Started

To add custom CSS:
1. Open NYA Core
2. Go to **OBS Integration** tab
3. Click the **CSS** sub-tab
4. Paste your CSS in the **Custom CSS** field
5. Click **Save**
6. Refresh your OBS browser source

---

## Using Custom Images

You can use custom images in your CSS using the `{{image:name}}` syntax.

### Adding Images
1. In the CSS tab, click **Add Image**
2. Browse and select your image
3. Give it a name (e.g., `background`, `logo`, `border`)
4. Use it in CSS: `url('{{image:background}}')`

### Example
```css
.container {
    background: url('{{image:myBackground}}') !important;
    background-size: cover !important;
}
```

---

## Splits Overlay

URL: `http://localhost:9876/splits.html`

### HTML Structure
```html
<div class="container" id="container">
    <div class="header">
        <span class="title" id="title">Splits</span>
        <span class="progress-text" id="progress">0/0</span>
    </div>
    <div id="splits">
        <!-- Split rows rendered dynamically -->
        <div class="split [current|completed|has-hits]">
            <span class="split-icon">▶</span>
            <img class="split-image" src="...">
            <span class="split-name">Boss Name</span>
            <span class="split-hits">0</span>
            <span class="pb better|equal|worse">-1</span>
        </div>
        <div class="separator [dashed|dotted|custom|image]">...</div>
    </div>
</div>
```

### CSS Variables

#### Container
| Variable | Default | Description |
|----------|---------|-------------|
| `--container-bg` | `rgba(26, 26, 46, 0.75)` | Background color |
| `--container-padding` | `12px` | Inner padding |
| `--border-radius` | `8px` | Corner rounding |

#### Typography
| Variable | Default | Description |
|----------|---------|-------------|
| `--font-family` | `'Segoe UI', Arial` | Font family |
| `--font-size` | `14px` | Split name size |
| `--hit-font-size` | `14px` | Hit counter size |
| `--header-font-weight` | `700` | Header text weight |
| `--name-font-weight` | `400` | Split name weight |
| `--hits-font-weight` | `700` | Hit counter weight |

#### Colors
| Variable | Default | Description |
|----------|---------|-------------|
| `--header-color` | `#00bcd4` | Header title color |
| `--text-color` | `#dddddd` | Default text color |
| `--split-bg` | `#2a2a3e` | Split row background |

#### Current Split
| Variable | Default | Description |
|----------|---------|-------------|
| `--current-color` | `#00bcd4` | Current split accent |
| `--current-text-color` | `#00bcd4` | Current split text |
| `--current-bg` | `rgba(0, 188, 212, 0.25)` | Current split background |

#### Completed Split
| Variable | Default | Description |
|----------|---------|-------------|
| `--completed-color` | `#32b450` | Completed accent |
| `--completed-text-color` | `#32b450` | Completed text |
| `--completed-bg` | `rgba(50, 180, 80, 0.2)` | Completed background |

#### Hit Split
| Variable | Default | Description |
|----------|---------|-------------|
| `--hit-color` | `#ff5555` | Hit accent |
| `--hit-text-color` | `#ff5555` | Hit text |
| `--hit-bg` | `rgba(255, 85, 85, 0.25)` | Hit background |

#### PB Comparison
| Variable | Default | Description |
|----------|---------|-------------|
| `--pb-better` | `#32b450` | Better than PB |
| `--pb-equal` | `#32b450` | Equal to PB |
| `--pb-worse` | `#ff5555` | Worse than PB |

#### Layout
| Variable | Default | Description |
|----------|---------|-------------|
| `--split-gap` | `3px` | Gap between splits |
| `--split-padding` | `6px` | Split row padding |
| `--row-height` | `auto` | Fixed row height |
| `--image-width` | `24px` | Split image width |
| `--image-height` | `24px` | Split image height |
| `--image-fit` | `contain` | Image fit mode |
| `--name-min-width` | `60px` | Min width for names |

#### Animation
| Variable | Default | Description |
|----------|---------|-------------|
| `--flash-duration` | `500ms` | Hit flash duration |
| `--separator-color` | `#444444` | Separator line color |

### CSS Classes

#### Split States
| Class | Description |
|-------|-------------|
| `.split` | Base split row |
| `.split.current` | Currently active split |
| `.split.completed` | Finished split |
| `.split.has-hits` | Split with hits > 0 |
| `.split.flash` | Temporary flash on hit |

#### Split Elements
| Class | Description |
|-------|-------------|
| `.split-icon` | Status icon (▶, ✓, ✗, ○) |
| `.split-image` | Boss/split image |
| `.split-name` | Split name text |
| `.split-hits` | Hit counter |
| `.split-hits.zero` | When hits = 0 |
| `.pb` | PB comparison badge |
| `.pb.better` | Better than PB |
| `.pb.equal` | Equal to PB |
| `.pb.worse` | Worse than PB |

#### Text Overflow
| Class | Description |
|-------|-------------|
| `.split-name` | Default: ellipsis |
| `.split-name.wrap` | Word wrap |
| `.split-name.scroll` | Horizontal scroll |
| `.split-name.clip` | Hard clip |

#### Separators
| Class | Description |
|-------|-------------|
| `.separator` | Solid line |
| `.separator.dashed` | Dashed line |
| `.separator.dotted` | Dotted line |
| `.separator.custom` | Custom text |
| `.separator.image` | Image separator |

---

## Hit Counter Overlay

URL: `http://localhost:9876/hit_counter.html`

### HTML Structure
```html
<div class="scale-wrapper" id="scale-wrapper">
    <div class="container [horizontal|compact]" id="container">
        <div class="label" id="label">HITS</div>
        <div class="hits" id="hits">0</div>
        <div class="timer" id="timer">00:00.00</div>
        <div class="game" id="game">Game Name</div>
    </div>
</div>
```

### CSS Variables

#### Container
| Variable | Default | Description |
|----------|---------|-------------|
| `--padding` | `20px` | Container padding |
| `--bg-color` | `rgba(0, 0, 0, 0.7)` | Background color |
| `--bg-image` | `none` | Background image |
| `--border-radius` | `10px` | Corner rounding |

#### Hit Counter
| Variable | Default | Description |
|----------|---------|-------------|
| `--hit-size` | `72px` | Hit number size |
| `--hit-color` | `#ff5555` | Hit number color |
| `--hits-font-weight` | `bold` | Font weight |
| `--hits-text-shadow` | `2px 2px 4px rgba(0,0,0,0.5)` | Text shadow |
| `--hits-letter-spacing` | `normal` | Letter spacing |
| `--hits-text-transform` | `none` | Text transform |
| `--hits-line-height` | `1.2` | Line height |

#### Label
| Variable | Default | Description |
|----------|---------|-------------|
| `--label-font-size` | `14px` | Label size |
| `--label-color` | `#888` | Label color |
| `--label-font-weight` | `normal` | Font weight |
| `--label-text-transform` | `uppercase` | Text transform |
| `--label-letter-spacing` | `2px` | Letter spacing |

#### Timer
| Variable | Default | Description |
|----------|---------|-------------|
| `--timer-size` | `24px` | Timer font size |
| `--timer-color` | `#4CAF50` | Timer color |
| `--timer-font-weight` | `normal` | Font weight |
| `--timer-font-family` | `monospace` | Timer font |

#### Game Name
| Variable | Default | Description |
|----------|---------|-------------|
| `--game-name-size` | `18px` | Game name size |
| `--game-color` | `#00bcd4` | Game name color |
| `--game-font-weight` | `normal` | Font weight |

#### Animation
| Variable | Default | Description |
|----------|---------|-------------|
| `--anim-duration` | `300ms` | Animation duration |
| `--anim-easing` | `ease-out` | Animation easing |

### CSS Classes

| Class | Description |
|-------|-------------|
| `.container` | Main container (vertical) |
| `.container.horizontal` | Horizontal layout |
| `.container.compact` | Compact wrapped layout |
| `.hits` | Hit counter number |
| `.label` | "HITS" label text |
| `.timer` | Timer display |
| `.game` | Game name |
| `.hidden` | Hide element |

---

## Progress Bar Overlay

URL: `http://localhost:9876/progress.html`

### HTML Structure
```html
<div class="scale-wrapper" id="scale-wrapper">
    <div class="container" id="container">
        <div class="header">
            <span id="splits">0/0 splits</span>
            <span id="hits">0 hits</span>
        </div>
        <div class="progress-bar" id="progress-bar">
            <div class="progress-segment normal" style="width: 50%"></div>
            <div class="progress-segment hit" style="width: 10%"></div>
            <div class="progress-segment pending" style="width: 40%"></div>
        </div>
        <div class="progress-text" id="percent">50%</div>
    </div>
</div>
```

### CSS Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `--padding` | `15px` | Container padding |
| `--bg-color` | `rgba(0, 0, 0, 0.7)` | Background |
| `--progress-height` | `20px` | Bar height |
| `--progress-bg` | `#333` | Bar background |
| `--progress-radius` | `4px` | Bar corner radius |
| `--progress-fill` | `linear-gradient(...)` | Fill gradient |
| `--progress-hit-color` | `#ef4444` | Hit segment color |
| `--header-font-size` | `12px` | Header text size |
| `--header-color` | `#888` | Header text color |
| `--percent-font-size` | `14px` | Percentage size |
| `--percent-color` | `white` | Percentage color |

### CSS Classes

| Class | Description |
|-------|-------------|
| `.container` | Main container |
| `.header` | Top header with stats |
| `.progress-bar` | Progress bar container |
| `.progress-segment` | Bar segment |
| `.progress-segment.normal` | Completed segment |
| `.progress-segment.hit` | Hit segment |
| `.progress-segment.pending` | Remaining segment |
| `.progress-text` | Percentage text |

---

## Game Tabs Overlay

URL: `http://localhost:9876/game_tabs.html`

### HTML Structure
```html
<div class="scale-wrapper" id="scale-wrapper">
    <div class="container" id="container">
        <div class="tabs" id="tabs">
            <div class="tab [current|IDLE|ACTIVE|COMPLETED|HIT]">
                <img class="tab-image" src="...">
                <span class="tab-name">Game Name</span>
                <span class="tab-hits">0 hits</span>
                <span class="tab-status">ACTIVE</span>
            </div>
        </div>
    </div>
</div>
```

### CSS Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `--tab-size` | `40px` | Tab image size |
| `--tab-gap` | `8px` | Gap between tabs |
| `--tab-bg` | `rgba(50, 50, 50, 0.8)` | Tab background |
| `--tab-opacity` | `1` | Tab opacity |
| `--tab-border-width` | `2px` | Border width |
| `--border-radius` | `6px` | Tab corner radius |
| `--layout` | `row` | `row` or `column` |
| `--idle-border` | `#555555` | Idle state border |
| `--active-border` | `#00bcd4` | Active state border |
| `--hit-border` | `#dc3232` | Hit state border |
| `--completed-border` | `#32b450` | Completed border |
| `--tab-active-bg` | `rgba(0, 188, 212, 0.3)` | Active background |
| `--tab-completed-bg` | `rgba(50, 180, 80, 0.3)` | Completed background |
| `--tab-hit-bg` | `rgba(220, 50, 50, 0.3)` | Hit background |
| `--text-color` | `#ffffff` | Tab text color |
| `--hits-color` | `#ff5555` | Hits text color |

### CSS Classes

| Class | Description |
|-------|-------------|
| `.tabs` | Tabs container |
| `.tab` | Individual tab |
| `.tab.current` | Currently selected |
| `.tab.IDLE` | Idle state |
| `.tab.ACTIVE` | Active state |
| `.tab.COMPLETED` | Completed state |
| `.tab.HIT` | Hit state (pulses) |
| `.tab-image` | Game image |
| `.tab-name` | Game name |
| `.tab-hits` | Hit counter |
| `.tab-status` | Status label |

---

## Animation Classes

These classes can be applied to trigger animations:

| Class | Effect |
|-------|--------|
| `.animate-pulse` | Scale pulse |
| `.animate-flash` | Opacity flash |
| `.animate-bounce` | Vertical bounce |
| `.animate-shake` | Horizontal shake |
| `.animate-fade` | Fade in |
| `.animate-slide` | Slide in from left |

---

## Tips & Tricks

### Force Override Styles
Use `!important` to ensure your styles override defaults:
```css
.container {
    background: #000 !important;
}
```

### Custom Fonts
Import Google Fonts at the top of your CSS:
```css
@import url('https://fonts.googleapis.com/css2?family=Roboto&display=swap');

:root {
    --font-family: 'Roboto', sans-serif;
}
```

### Transparent Background
For a fully transparent overlay:
```css
.container {
    background: transparent !important;
}
```

### Glow Effects
Add glow to elements:
```css
.split.current {
    box-shadow: 0 0 20px rgba(0, 188, 212, 0.5);
}
```

### Gradient Backgrounds
```css
.container {
    background: linear-gradient(180deg, #1a1a2e 0%, #16213e 100%) !important;
}
```

### Hide Elements
```css
.timer { display: none !important; }
.header { display: none !important; }
.pb { display: none !important; }
```

---

## Example Themes

See the [Theme Gallery](Theme-Gallery.md) for complete example themes you can copy and customize.
