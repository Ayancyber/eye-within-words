# The Eye Within Words — by ayan

> A living, blinking human eye rendered entirely out of poetry.

**[▶ Live Demo](https://ayancyber.github.io/eye-within-words/)**

Every pixel of the iris, pupil, sclera, and eyelids is real English text — quotes, philosophical fragments, and poetic words packed edge-to-edge across the screen. No images. No vectors. Just typography, laid out with geometry, and animated with a continuous cinematic camera zoom.

The eye blinks. The camera drifts. The iris breathes. And if you look closely enough, you'll find the artist's signature hiding in the pupil.

---

## Preview

```text
╭─────────────────────────────────────────╮
│    light remembers where it has been    │
│      every thought leaves a trace       │
│  ╭─────────────────────────────────╮   │
│  │   ╭───────────────────╮         │   │
│  │   │     ◉ ayan ◉      │         │   │
│  │   │   consciousness   │         │   │
│  │   ╰───────────────────╯         │   │
│  ╰─────────────────────────────────╯   │
│    what you seek is seeking you         │
│    the universe lives within us         │
╰─────────────────────────────────────────╯
```

---

## Controls

| Key | Action |
| --- | --- |
| `SPACE` | Trigger an immediate blink |
| `B` | Enable / disable automatic blinking |
| `Z` | Toggle automatic camera zooming |
| `C` | Cycle through color palettes |
| `R` | Regenerate a new arrangement of quotes |
| `H` | Show / hide keyboard instructions |
| `ESC` | Close the animation / exit fullscreen |
| **Mouse** | Subtly influences the gaze and camera focus |

---

## Features

- **100% typography** — every shape you see is a real, readable word or sentence
- **Living iris** — 15 concentric word rings + 84 radial spokes that cross-fade and breathe
- **Biological blink** — 3-phase state machine (close → hold → open) with eased curves, randomized durations, and occasional double-blinks
- **Cinematic camera** — continuous smooth zoom from wide view ($1.0\times$) to intimate close-up ($2.55\times$), over a $1.55\times$ virtual canvas so no blank edges ever show
- **Animated eyelids** — rim words ride the eyelid edge in real time, arc-length spaced so they never overlap on the curve
- **Mouse gaze steering** — the eye follows your cursor, then returns to its idle drift after 4.5 seconds
- **Three palettes** — Midnight Poetry, Bioluminescent Verse, Astral Manuscript
- **Signature throughout** — `ayan` appears at the pupil center, on the upper eyelid rim, in the iris highlights, and as a persistent corner watermark

---

## Tech

**Pure HTML + Canvas 2D + vanilla JavaScript.** No dependencies, no build step, no frameworks, no external fonts (uses the system font stack).

| Technique | Where it's used |
| --- | --- |
| Parametric eyelid curves | `evalEyelid(u, openness)` — upper/lower lid shape as a function of normalized horizontal position $u \in [-1, 1]$ |
| Arc-length parameterization | Rim words spaced along a *sloped* curve via $\cos \theta = \frac{1}{\sqrt{1 + \text{slope}^2}}$ correction |
| Virtual canvas overscan | $1.55\times$ the screen size, camera crops a sub-rect so zoom never exposes blank space |
| `destination-in` / `destination-out` | Eyeball polygon clipping + iris cutout mask on sclera |
| `lighter` composite | Additive iris glow |
| Pre-rendered sprites | Rim words rasterized once per angle (closed/mid/open), cached |
| Seeded PRNG | mulberry32 — deterministic regeneration via `R` |
| Layered cross-fade | Two full iris ring layers blended by $\text{iris\_mix} = 0.5 + 0.5 \cdot \sin(t \cdot 0.6)$ |

All typography layers are built **once** at startup (~200–400ms). The frame loop only blits pre-rendered canvases, so steady-state is a smooth 60fps.

---

## Run Locally

**Option 1 — just open it**

```bash
open index.html        # macOS
start index.html       # Windows
xdg-open index.html    # Linux
```

**Option 2 — local server (recommended if the `file://` origin misbehaves)**

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```