# 🌊 The Last Signal

> *A browser-based submarine survival game. Dive into the crushing dark. Find the signal. Don't run out of air.*

![Game Status](https://img.shields.io/badge/status-in%20development-00c8a0?style=flat-square)
![Tech](https://img.shields.io/badge/tech-vanilla%20JS%20%7C%20HTML5%20Canvas-0a3a48?style=flat-square)
![No Dependencies](https://img.shields.io/badge/dependencies-none-00ecd4?style=flat-square)

---

## 🎮 What Is This?

**The Last Signal** is a side-scrolling submarine survival game that runs entirely in the browser — no installs, no build steps, no libraries. Just open the HTML file and play.

You pilot a submarine through the crushing ocean depths, racing against a depleting oxygen supply, navigating jagged rock formations, swaying coral, and drifting wreckage — all to reach a mysterious distress beacon at the ocean floor.

---

## ▶️ How to Play

1. Download `the-last-signal.html`
2. Open it in any modern browser (Chrome, Firefox, Edge, Safari)
3. That's it — the game starts immediately

### Controls

| Key | Action |
|-----|--------|
| `W` / `↑` | Thrust up |
| `S` / `↓` | Thrust down |
| `A` / `←` | Thrust left |
| `D` / `→` | Thrust right |
| `Space` | Sonar ping *(coming soon)* |

---

## ✨ Features (Current Build)

### Foundation
- **Fixed logical resolution** (960×540) scaled to fit any screen — no coordinate math surprises
- **60fps game loop** with clean update/render separation
- **Physics-based movement** — velocity, drag, and inertia give the sub genuine underwater weight

### Visuals
- Deep ocean color palette — near-black backgrounds, deep navy blues, bioluminescent greens and teals
- **4-layer parallax particle system** — drifting plankton and micro-sediment at different depths
- **Animated caustic light** shimmer across the surface layer
- **Submarine drawn entirely in Canvas 2D** — hull, conning tower, spinning propeller, nose light, rivets, and live bubble trail
- Idle **bob animation** on the submarine
- Subtle **scanline overlay** for a pressure-cracked viewport feel

### World & Obstacles
- **Procedurally generated level** — 12,000px world with a seeded PRNG (same layout every run, fair deaths)
- **Organic floor heightmap** using layered sine waves — the ocean floor is jagged and alive
- **Rock formations** — ceiling and floor spikes with navigable gaps
- **Mid-water debris** — three variants: pipe sections, metal panels, pressure spheres
- **Coral clusters** — floor-anchored, gently swaying, bioluminescent tips (teal or purple)
- **AABB collision detection** with push-out resolution — no tunnelling, no sticky walls
- **World progress bar** — a 2px indicator at screen bottom with an amber beacon marker

### HUD
- **O₂ Supply bar** — colour shifts teal → amber → red as oxygen depletes
- **Depth meter** — derived from player Y position + world progress
- **Sonar indicator** — UI placeholder ready for Step 3 wiring
- Fonts: [Orbitron](https://fonts.google.com/specimen/Orbitron) + [Share Tech Mono](https://fonts.google.com/specimen/Share+Tech+Mono)

---

## 🗺️ Roadmap

This game is being built step by step. Here's the full plan:

- [x] **Step 1** — Canvas setup, game loop, submarine rendering, movement physics, particle atmosphere
- [x] **Step 2** — Procedural obstacles (rocks, debris, coral), floor heightmap, collision detection, world progress
- [ ] **Step 3** — Deep-sea creatures (patrolling anglerfish, drifting jellyfish)
- [ ] **Step 4** — Oxygen system (depletion, partial refill on signal contact)
- [ ] **Step 5** — Sonar ping mechanic (spacebar, illuminates nearby threats)
- [ ] **Step 6** — Visual pressure effects (vignette, screen flicker, distortion on low O₂)
- [ ] **Step 7** — UI polish pass
- [ ] **Step 8** — Win screen and death screen (cinematic, atmospheric)

---

## 🏗️ Architecture

Everything lives in a **single HTML file** — no build pipeline, no bundler, no dependencies.

```
the-last-signal.html
├── <style>          — CSS reset, canvas sizing, HUD layout (HTML overlay)
├── <canvas>         — All game rendering happens here
├── <div id="hud">   — Oxygen bar, depth meter, sonar indicator (HTML elements over canvas)
└── <script>
    ├── Canvas Setup          — Fixed 960×540 logical resolution, CSS scaling
    ├── CFG                   — All tunable constants in one object
    ├── state                 — Single source of truth for all mutable game data
    ├── Particle System       — 4 parallax layers of drifting bioluminescent motes
    ├── Player                — Physics, movement, submarine draw, bubble trail
    ├── Input Handling        — Key-state map (keydown/keyup)
    ├── Draw Helpers          — Background gradient, caustics, scanlines
    ├── Obstacle System       — PRNG, floor heightmap, rock/debris/coral generation & draw, AABB collision
    ├── update()              — Logic tick (world scroll, particles, player, collision, HUD)
    └── render()              — Draw tick (background → particles → obstacles → floor → player → UI)
```

### Key Design Decisions

**Why a fixed logical resolution?**
All game coordinates live in 960×540 space. CSS scales the canvas element to fit the viewport. This means zero screen-size-dependent bugs in game logic.

**Why a seeded PRNG?**
The level is procedurally generated but deterministic. Every player sees the same obstacle layout, making deaths learnable and the game fair.

**Why HTML elements for the HUD?**
Drawing text and bars on canvas works, but HTML gives you smooth CSS transitions, proper font rendering, and easier layout — so the O₂ bar colour-shifts are a single CSS change, not a repaint.

**Why AABB collision with push-out?**
Axis-aligned bounding boxes are fast enough for this game's obstacle density. The push-out resolver finds the shallowest overlap axis and ejects the player along it — preventing sticky walls and tunnelling without needing a physics engine.

---

## 🐛 Debug Mode

Open the file and set this flag near the top of the script:

```js
const CFG = {
  // ...
  DEBUG: true,   // ← flip this
};
```

This draws red bounding boxes around every obstacle and the player hitbox — useful for tuning collision feel.

---

## 🛠️ Tech Stack

| Layer | Choice | Reason |
|-------|--------|--------|
| Rendering | HTML5 Canvas 2D | No library overhead, full control |
| Language | Vanilla JavaScript (ES2020) | Zero dependencies, runs anywhere |
| Styling | Plain CSS | HUD layout, canvas scaling |
| Fonts | Google Fonts (Orbitron, Share Tech Mono) | Nautical/technical aesthetic |
| Physics | Custom (velocity + drag) | Tuned for underwater feel |
| RNG | Mulberry32 (seeded) | Fast, deterministic, no imports |

---

## 📄 License

MIT — do whatever you want with it.

---

*Built with vanilla JS and a love of deep ocean dread.*
