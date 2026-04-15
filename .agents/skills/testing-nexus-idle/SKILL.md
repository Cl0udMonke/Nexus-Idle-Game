# Testing NEXUS // IDLE Game

## Project Overview
This is a static HTML idle/clicker game — a single `index.html` file with inline CSS and JS. No build tools, no package manager, no dependencies to install.

## How to Serve Locally
```bash
cd /home/ubuntu/repos/Nexus-Idle-Game
python3 -m http.server 8080
```
Then open `http://localhost:8080` in Chrome.

## Known CSS Issues

### Em-Dash in CSS Custom Properties
The `<style>` block in `index.html` may use **em-dash characters (U+2014 `—`)** instead of **double-hyphens (`--`)** for CSS custom properties (e.g., `var(—bg)` instead of `var(--bg)`). The browser silently drops these as invalid, causing:
- Transparent/white backgrounds instead of dark theme
- Missing colors, borders, glows
- Invisible toggle switches and other styled elements

The JS `applyTheme()` function sets correct `--` properties via inline styles, but the CSS rules reference the em-dash versions, so the stylesheet declarations never resolve.

**How to verify**: Run in browser console:
```js
getComputedStyle(document.body).backgroundColor
// If "rgba(0, 0, 0, 0)" → CSS vars are broken
// If a real color → CSS vars are working
```

### Font Override Bug
The rule `* { font-family: inherit !important; }` may override all font-family declarations, causing everything to render in the browser's default font (Times New Roman) instead of Orbitron/Share Tech Mono.

**How to verify**: Run in browser console:
```js
document.fonts.forEach(f => console.log(f.family, f.status));
// If all show "unloaded" → fonts are broken
```

### Missing Tab Click Handlers
The drawer tab buttons (PPS, PPC, OFFLINE, QUANTUM, STATS, SETTINGS) may not have click handlers attached. The `switchTab()` function exists but might not be bound to the buttons.

**Workaround**: Call `switchTab('settings')` directly from the console.

## Testing Checklist

1. **Page Load**: Verify background is dark (not white), title is "NEXUS // IDLE"
2. **Fonts**: Check Orbitron on headers (geometric, wide-spaced) and Share Tech Mono on body text (monospaced). Inspect via DevTools computed font-family.
3. **Favicon**: Check browser tab for icon.png. Note: the `type` attribute may say `image.png` instead of `image/png` — Chrome still loads it.
4. **Tab Switching**: Click each drawer tab to verify they switch content. If clicking doesn't work, use `switchTab('tabname')` in console.
5. **Settings Panel**: Verify Dark Mode toggle, Particles toggle, theme buttons (NEXUS, CRIMSON, VOID, EMERALD, SUNSET, OCEAN)
6. **Light/Dark Mode**: Toggle dark mode and verify background/text colors change
7. **Tap Interaction**: Click the center TAP circle and verify credit count increments
8. **Responsiveness**: Test at 1024x768 (desktop), 768x1024 (tablet), 375x667 (mobile) using DevTools device emulation

## Game State
- Game saves to `localStorage` key `nexus_v5`
- To reset: run `localStorage.removeItem('nexus_v5'); location.reload()` in console
- Admin menu: Click CRIMSON theme button 5 times in Settings to unlock

## Devin Secrets Needed
None — this is a fully static site with no authentication or API keys.
