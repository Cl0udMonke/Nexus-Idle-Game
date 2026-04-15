# NEXUS // IDLE

A cyberpunk-themed idle/clicker game built as a single-page Progressive Web App (PWA).

## How to Play

Tap the central **TAP** core to earn **credits**. Use credits to purchase upgrades that increase your earnings automatically or boost your tap power. Progress through prestige and quantum resets to unlock powerful multipliers.

## Game Mechanics

### Currency

- **Credits** — The primary currency, earned by tapping or passively via CPS (Credits Per Second).

### Upgrades

The game has four upgrade categories, accessible via the drawer tabs at the bottom of the screen:

#### PPS (Production Per Second)

Generators that produce credits automatically each second. Each generator has a base cost that scales by **×1.15** per level owned.

| Generator       | Icon | Base Cost   | Base CPS   | Description                   |
| --------------- | ---- | ----------- | ---------- | ----------------------------- |
| Nano Miner      | ⚙️   | 10          | 0.1/s      | Basic credit generator        |
| Data Stream     | 📡   | 100         | 1/s        | Harvests data flows           |
| Crypto Farm     | 💎   | 1,100       | 8/s        | Mines digital currency        |
| AI Core         | 🧠   | 12,000      | 47/s       | Intelligent processing        |
| Quantum Bank    | 🏦   | 130,000     | 260/s      | Quantum computation           |
| Black Hole      | ⚫   | 1,400,000   | 1,400/s    | Infinite compression          |
| Dimension Rift  | 🌀   | 20,000,000  | 7,800/s    | Extracts from other realities |
| Universe Forge  | 🌌   | 330,000,000 | 44,000/s   | Creates new universes         |

#### PPC (Production Per Click)

Upgrades that increase the credits earned per tap. Cost also scales by **×1.15** per level.

| Upgrade         | Icon | Base Cost   | Base PPC   | Description          |
| --------------- | ---- | ----------- | ---------- | -------------------- |
| Reinforced Tap  | 👆   | 15          | +1         | Stronger clicks      |
| Multi-Touch     | ✌️   | 100         | +5         | Double tap power     |
| Laser Pointer   | 🔴   | 1,200       | +25        | Precision tapping    |
| Tap Amplifier   | 🔊   | 14,000      | +125       | Amplified input      |
| Gravity Tap     | 🌍   | 170,000     | +625       | Gravitational force  |
| Nuclear Tap     | ☢️   | 2,100,000   | +3,125     | Atomic energy        |

#### Offline Earnings

One-time purchases that allow credits to accumulate while the app is closed. Each tier requires the previous tier to be owned.

| Upgrade       | Icon | Cost    | Effect                          |
| ------------- | ---- | ------- | ------------------------------- |
| Idle Core I   | 🔋   | 1,000   | Earn 25% of CPS offline         |
| Idle Core II  | 🔋   | 10,000  | Earn 50% of CPS offline         |
| Idle Core III | 🔋   | 100,000 | Earn 75% of CPS offline         |
| Time Bank I   | ⏰   | 5,000   | +1 hour max offline time        |
| Time Bank II  | ⏰   | 50,000  | +2 hours max offline time       |
| Time Bank III | ⏰   | 500,000 | +4 hours max offline time       |

Base offline time cap is **30 minutes**. Time Bank upgrades stack additively.

#### Quantum Upgrades

Purchased with **Quantum Points (QP)** earned from Quantum Resets.

| Upgrade           | Icon | Cost (QP) | Effect              |
| ----------------- | ---- | --------- | ------------------- |
| Quantum Boost I   | ⚡   | 1         | +10% all production |
| Quantum Boost II  | ⚡   | 5         | +25% all production |
| Quantum Boost III | ⚡   | 15        | +50% all production |
| Prestige Power    | 💫   | 10        | +20% prestige gain  |
| Click Surge       | 👆   | 8         | 2× tap power        |

### Prestige System

Prestige resets your **credits**, **total clicks**, and all **PPS/PPC upgrades** in exchange for **Prestige points** that provide a permanent **+10% production multiplier** per point.

- **Threshold**: 100,000 × 10^(current prestige level) credits
- **Gain formula**: floor(log₁₀(credits / 100,000)) × prestige boost multiplier
- Found on the **STATS** tab

### Quantum Reset

A higher-tier reset that also clears your **Prestige points** and **Offline upgrades** in exchange for **Quantum Points**.

- **Threshold**: 1,000,000,000 × 100^(current quantum level) credits
- **Gain formula**: floor(log₁₀(credits / 1,000,000,000))
- Found on the **QUANTUM** tab

### Multiplier Stack

All production is affected by a chain of multipliers:

```
Effective CPS = base CPS × Prestige Mult × Quantum Mult
Effective Tap = base Tap × Prestige Mult × Quantum Mult × Click Mult
```

- **Prestige Mult** = 1 + (prestige points × 0.1)
- **Quantum Mult** = 1 + sum of (Quantum Boost level × bonus) for each boost
- **Click Mult** = product of (clickMult ^ owned) for Click Surge upgrades

## UI Elements

### Header

Displays the current **credit count**, plus stat boxes for **CPS**, **TAP** power, **Prestige** level, and **Quantum** level.

### Tap Zone

Central pulsing circle — tap to earn credits. Visual feedback includes:
- Scale animation on tap
- Floating "+amount" text
- Particle effects (canvas-based)
- Flash overlay

### Drawer (Bottom Panel)

Tabbed panel with six tabs: **PPS**, **PPC**, **OFFLINE**, **QUANTUM**, **STATS**, **SETTINGS**.

### Settings

| Setting        | Description                                             |
| -------------- | ------------------------------------------------------- |
| Dark Mode      | Toggle dark/light theme (default: dark)                 |
| Particles      | Toggle tap particle effects                             |
| Tap Feedback   | Toggle floating "+credits" text on tap                  |
| Sound Effects  | Toggle synthesized audio feedback                       |
| Auto-Save      | Toggle periodic auto-save (default: every 30 seconds)   |
| Save Interval  | Configurable 5–300 seconds                              |
| Theme          | NEXUS, CRIMSON, VOID, EMERALD, SUNSET, OCEAN            |
| Wipe All Data  | Permanently deletes all save data                       |

### Themes

Six color themes available: **Nexus** (cyan/default), **Crimson** (red), **Void** (purple), **Emerald** (green), **Sunset** (orange), **Ocean** (blue). Both dark and light modes are supported.

### Admin Menu

Hidden developer menu unlocked by clicking the **CRIMSON** theme button 5 times. Provides:
- Infinite Coins toggle
- Free Buys toggle
- +1000 CPS / +100 Tap Power
- Force Prestige / Force Quantum
- Custom coin amounts (+1K to +100M quick buttons)

## Technical Details

- **Single-file app**: All HTML, CSS, and JavaScript in `index.html`
- **PWA support**: `manifest.json` + `service-worker.js` for offline play and install-to-homescreen
- **Save system**: Uses `localStorage` (key: `nexus_v5`) with configurable auto-save
- **Fonts**: Orbitron (headings) and Share Tech Mono (body) via Google Fonts
- **No external dependencies**: Pure vanilla HTML/CSS/JS

## Files

| File                | Description                              |
| ------------------- | ---------------------------------------- |
| `index.html`        | Main game (current version)              |
| `manifest.json`     | PWA manifest                             |
| `service-worker.js` | Service worker for offline caching       |
| `icon.png`          | App icon (512×512)                       |
| `v1`–`v8`           | Archived earlier (broken) versions       |
