# Testing: Nexus Idle Game

## Overview
Nexus Idle Game is a monolithic single-file web app (`index.html`) — all HTML, CSS, and JS live in one file. It's a browser-based idle/clicker game with themes, settings, and an admin cheat menu.

## Local Server Setup
```bash
cd /home/ubuntu/Nexus-Idle-Game
python3 -m http.server 8080 &
# Access at http://localhost:8080/index.html
```

No build step or dependencies required — it's a static HTML file.

## Game State
- **localStorage key**: `nexus_v5`
- **Clear state**: `localStorage.removeItem('nexus_v5')` then reload
- **Save/load**: Game auto-saves; state includes coins, upgrades, settings, theme, dark mode preference, admin unlock status

## Key UI Elements

### Drawer Tabs (bottom bar)
- 6 tabs: PPS, PPC, OFFLINE, QUANTUM, STATS, SETTINGS
- Click active tab → closes drawer (toggle behavior)
- Click different tab → opens that tab's content
- Default tab on fresh load: PPS

### Tap Zone
- The entire area between the header and the drawer tab bar is the tap zone
- Clicking anywhere in this zone increments credits (not just the center circle)
- Each tap adds `getTap()` amount (starts at 1)

### Settings Panel
Accessed via SETTINGS tab. Contains:
- **Dark Mode** toggle (tog-dark)
- **Particles** toggle (tog-part)
- **Tap Feedback** toggle (tog-pconf)
- **Sound Effects** toggle (tog-sound)
- **Auto-Save** toggle (tog-autosave)
- **Save Interval** number input
- **Theme grid**: 6 buttons — NEXUS, CRIMSON, VOID, EMERALD, SUNSET, OCEAN
- **WIPE ALL DATA** button

### Toggle Buttons
- ON state: `.toggle-btn.on` class, cyan glow border, knob on right, "ON" label
- OFF state: no `.on` class, gray background, knob on left, "OFF" label

### Themes
- **Dark mode**: Uses `THEMES` object (neon colors on dark backgrounds)
- **Light mode**: Uses `THEMES_LIGHT` object (muted colors on tinted light backgrounds)
- `applyTheme()` checks `S.darkMode` to pick the right palette
- Each theme has distinct: bg, bg2, c1 (accent), border, txt colors
- Light theme backgrounds: Nexus=#f0f4f8, Crimson=#fff0f3, Void=#f4f0ff, Emerald=#f0fff6, Sunset=#fff8f0, Ocean=#f0f8ff

## Known CSS Issues

### Smart Quotes and Em-Dashes
The file has historically had issues with smart/curly quotes (U+2018/U+2019) and em-dash characters (U+2014) breaking CSS custom properties and selectors. If editing, use a proper code editor (VS Code), not a word processor.

**How to verify CSS vars are working**: Run in browser console:
```js
getComputedStyle(document.body).backgroundColor
// If "rgba(0, 0, 0, 0)" → CSS vars are broken
// If a real color → CSS vars are working
```

## Admin Menu (Hidden)

### How to Unlock
1. Go to SETTINGS tab
2. Click the **CRIMSON** theme button **5 times**
3. Toast: "⚡ ADMIN MENU UNLOCKED"
4. ⚡ icon appears in top-right corner
5. Clicking CRIMSON again when admin is unlocked will **hide** the admin menu and reset the click counter

### Admin Menu Buttons
- **∞ Infinite Coins** — toggles infinite coins
- **🆓 Free Buys** — toggles free purchases
- **+1000 CPS** — adds 100 to PPS[0].owned, toast shows computed `getCPS()` delta
- **+100 Tap Power** — adds 100 to PPC[0].owned, toast shows computed `getTap()` delta
- **Force Prestige** / **Force Quantum** — trigger resets
- **Coin buttons** (+1K to +100M) — add coins directly

### Important: Admin toast values are computed
The +1000 CPS and +100 Tap Power buttons compute actual deltas using `getCPS()` / `getTap()` before and after the upgrade. The toast shows the real impact (e.g., "+11.74M CPS") not the hardcoded label.

## Common Testing Scenarios

### Fresh State Test
```javascript
// In browser console
localStorage.removeItem('nexus_v5');
location.reload();
```
Expected: 0 credits, dark mode, Nexus theme, PPS tab active with drawer open.

### Theme Testing in Light Mode
1. Open SETTINGS
2. Toggle Dark Mode OFF
3. Click each theme button — verify distinct background colors change
4. Toggle Dark Mode ON — verify dark neon theme restores

### Drawer Toggle Testing
1. On fresh load, PPS tab should be active with content visible
2. Click PPS again — drawer should close, no tab highlighted
3. Click any other tab — drawer reopens with that content
4. Click that same tab — drawer closes again

## Testing Checklist
1. **Page Load**: Verify background is dark (not white), title is "NEXUS // IDLE"
2. **Tab Switching**: Click each drawer tab to verify they switch content and toggle open/close
3. **Settings Panel**: Verify Dark Mode toggle, Particles toggle, theme buttons
4. **Light/Dark Mode**: Toggle dark mode and verify background/text colors change
5. **Light Mode Themes**: Switch between all 6 themes in light mode — each should show distinct colors
6. **Tap Interaction**: Click anywhere in the tap zone (above, below, or on the circle) and verify credit count increments
7. **Toggle Visibility**: Verify ON toggles show glow + right knob + "ON" label; OFF shows gray + left knob + "OFF" label
8. **Admin Menu**: Unlock via CRIMSON x5, verify CPS/Tap buttons show computed deltas

## Known Gotchas
- **`drawerOpen` initialization**: Must start as `false` so the first `switchTab()` call during init opens (not closes) the drawer.
- **Rapid CRIMSON clicks**: If you click CRIMSON too fast (e.g., sending multiple clicks in one action), the 5th click unlocks admin and the 6th immediately hides it. Click deliberately with pauses between.
- **No CI/build**: This is a static HTML file with no build pipeline. Testing is purely manual/browser-based.

## Devin Secrets Needed
None — no authentication or API keys required. The game runs entirely client-side.
