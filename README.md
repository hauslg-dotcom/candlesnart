# Candlesnart

A single-page scroll-narrative website for a hand-painted candles & art brand.

**The concept:** a candle has to fall apart to become art. The first half of the
page is a near-monochrome world (bone, wax, shadow, one amber flame). As you
scroll, the candle lights, melts, tips, and falls off the table — and at the
moment of impact the wax detonates into a paint splatter and the whole site
inverts into full technicolor. Everything after lives in that color world:
the shop, draggable wax blobs, a paintbrush cursor, and a relightable candle.

## Running it

It's one self-contained file. Open `index.html` in a browser, or serve it:

```
python3 -m http.server 8000
```

Libraries (GSAP, ScrollTrigger, Lenis, Matter.js) and fonts load from CDN.
If the CDN is unreachable, the site degrades to the static storyboard mode.

## Structure

- **Acts I–VI** — one pinned, scrub-driven GSAP timeline (`tl`, 0–100 units)
  over a sticky stage. All SVG vector art, no photos, no 3D.
- **Act VI impact** — `detonate()` / `undetonate()`, driven with hysteresis
  from the master ScrollTrigger's `onUpdate` so it replays cleanly in reverse.
- **Act VII** — normal scrolling sections; the splatter-to-grid transition,
  Matter.js blob playpen, shop, about, contact, finale.
- **Reduced motion / no-JS / CDN failure** — `#storyboard`: the seven acts as
  captioned still frames, zero animation, followed by the working shop.

## Iteration knobs

| What | Where |
|---|---|
| Act timings | timeline positions in section 4 of the script (0–100 scale) |
| Scroll length of the story | `#stage-wrap` height (820vh desktop / 560vh mobile) |
| Tip resistance strength/zone | `REZ` object + the `onUpdate` block (section 5) |
| Particle counts | `MAXP`, debris `n` in `spawnSplatter` |
| Palette | CSS custom properties + `PALETTE` array |
| Impact trigger point | the `0.72` / `0.695` progress checks in `onUpdate` |

## TODO for launch

- Replace the vector product art with **real photos of the real candles**
  (see `.photo-note` in the shop section).
- Swap the placeholder tagline/copy for the brand's real voice if it differs.
- Point the contact form at a real endpoint (currently a `mailto:` placeholder).

## Also in this repo

- [`dorm-designer/`](dorm-designer/) — **Dorm Studio**, a separate single-page
  app. Describe the vibe you want and a fictional dorm room fills itself in with
  matching products you can drag around, swap, and price out. See
  [its README](dorm-designer/README.md).
