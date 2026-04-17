# 3D Casino Games (three.js)

This directory contains a collection of browser-based 3D casino game prototypes and demos built with three.js.
Each page is self-contained and loads local assets (cards, chips, sounds, table textures) plus three.js modules from this folder.

## What Is Included

- Blackjack (main + beta)
- Roulette (main + beta)
- Poker (single-machine + P2P/multiplayer variants)
- Chemin de Fer / Baccarat-style table
- Keno (3D draw machine)
- Card mechanic sandboxes/demos
- Intro overlay demo

## Quick Start

These pages must be served over HTTP/HTTPS. Opening with file:// will usually break module imports, texture loads, and audio.

1. Start a static server from this folder:

```bash
cd public_html/Casino
python3 -m http.server 8000
```

2. Open a game in the browser, for example:

- http://localhost:8000/bj.html
- http://localhost:8000/roulette.html
- http://localhost:8000/poker.html
- http://localhost:8000/keno.html

## Game Pages

### Core Pages

| File | Title / Mode | Main Controls | Notes |
|---|---|---|---|
| `bj.html` | 3D Blackjack | Shuffle, Deal, Reset Deck, Reveal, Hit, Stand, Split, Double Down, chip spawners, speed slider | Main blackjack table flow with card and chip interaction. |
| `roulette.html` | 3D Roulette | Spin, Reset, chip denomination buttons, speed slider | 38-pocket wheel (`0` + `00` + 1-36), betting grid, hover and board overlays. |
| `poker.html` | 3D Poker | Shuffle, Deal, Reset Deck, Reveal, chip spawners, speed slider | Includes per-seat action panels (Fold/Check/Bet/Call/Raise). |
| `chemin.html` | 3D Chemin de Fer | Shuffle, Deal Round, Reset Deck, role toggle, Player/Banker stand-draw controls, chip spawners, speed slider | Baccarat-style round control with role switching UI. |
| `keno.html` | 3D Keno | Auto pick, Clear, Draw 20, bet input | 1-80 grid, max 10 selections, draw visualization and match/payout panel. |

### Variant / Experimental Pages

| File | Purpose |
|---|---|
| `bj_beta.html` | Blackjack variant with expanded table theming/audio choices. |
| `roulette_beta.html` | Roulette variant iteration with similar gameplay controls. |
| `poker_beta.html` | Multiplayer/P2P poker variant (room-based), chat/thread panel, confetti toggle, seat assignment UI. |
| `poker_p2p.html` | Multiplayer/P2P poker page similar to `poker_beta.html` (room URL parameter + live seat/player display). |
| `gruppebj.html` | Card table sandbox variant (deal/reveal/show-hand style controls). |
| `cards_deal_shuffle_reveal.html` | Standalone card mechanics demo (shuffle/deal/reveal/show-hand). |
| `control.html` | Intro overlay animation demo (non-game utility page). |

## Shared Controls and UX Patterns

Most pages follow a common interaction style:

- Orbit camera controls via `OrbitControls`
- Top-left button bar for game actions
- Bottom-left log/status panel
- Optional overlay labels for odds or state feedback
- Intro splash title animation on load (many pages)
- Chip spawners commonly mapped as:
  - Black: $100
  - Green: $25
  - Blue: $10
  - Red: $5

## Multiplayer Notes

P2P poker variants:

- `poker_beta.html`
- `poker_p2p.html`

Room usage example:

```text
http://localhost:8000/poker_p2p.html?room=my-room
```

Implementation details:

- Uses PeerJS from CDN (`peerjs@1.5.2`) in-page.
- Includes room info, player count, seat assignment display.
- Includes thread/chat panel and per-seat action UI.

Important:

- Reliable multiplayer depends on signaling and peer connectivity conditions.
- Browser/network/firewall constraints can affect peer discovery and connections.

## Asset and Folder Overview

### Core Assets

- `cards/`
  - `hearts/`, `spades/`, `diamonds/`, `clubs/`, `backs/`
  - Card faces are loaded as WebP textures (rank files such as `ace.webp`, `2.webp`, ..., `king.webp`)
- `chips/`
  - `black.webp`, `blue.webp`, `green.webp`, `red.webp`
- `sfx/`
  - `deal/` (`d1.wav` ... `d6.wav`)
  - `shuffle/` (`s1.wav`)
  - `theme/` (background music tracks)
- `surfaces/`
  - Table/board textures and layout resources (`1.webp`, `2.webp`, `r_surface.webp`, `r_board.svg`, `r_board.txt`)

### Engine and Libraries in Repo

- `three.module.js` and `three.core.js`
- `jsm/` three.js modules (controls, render helpers, etc.)
- `js/` utility folders (`controls`, `effects`, `networks`, `libs`)

Most HTML files use an import map like:

```html
<script type="importmap">
{
  "imports": {
    "three": "./three.module.js",
    "three/addons/": "./jsm/"
  }
}
</script>
```

## Audio Behavior

- Shuffle, deal, and theme audio are loaded in several pages with `THREE.AudioLoader`.
- Browsers may block autoplay until the first user interaction.
- If audio seems missing, click/tap the page once and trigger an action again.

## Browser/Runtime Requirements

- Modern browser with WebGL support (Chrome, Edge, Firefox, Safari)
- JavaScript enabled

## Troubleshooting

### Blank page or module errors

- Ensure you are running from a server and opening `http://...`, not `file://...`.

### Missing textures/chips/cards

- Confirm you started the server from `public_html/Casino` so relative paths resolve.
- Check browser DevTools Console/Network for 404s.

### No sound

- Interact with the page first (autoplay policy).
- Confirm audio files exist in `sfx/` and are not blocked by mixed content/CORS.

### Multiplayer not syncing

- Use the same room string on all peers.
- Check browser console for PeerJS/connectivity errors.
- Try another network or browser when peer connections are blocked.

## Development Notes

- This project is static and does not require `npm install` to run pages.
- Most game logic is embedded directly in each HTML file.
- The two poker P2P pages are very similar and may diverge over time; keep shared fixes aligned when updating multiplayer behavior.

## Suggested Maintenance Plan

1. Keep one page per game as canonical (`bj.html`, `roulette.html`, `poker.html`, `chemin.html`, `keno.html`).
2. Treat `*_beta.html` and `*_p2p.html` as experimental branches.
3. Extract shared code (card/chip/audio helpers) into reusable modules under `js/` to reduce duplication.
4. Add a small launcher `index.html` in this folder linking all game pages for easier navigation.
