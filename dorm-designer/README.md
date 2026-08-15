# Dorm Studio

A dorm room you can design before you buy anything. Describe the vibe you're
going for in plain English, and a fictional dorm room fills itself in — walls,
floor, bedding, rug, lamps, plants, the lot. Change your mind, and it changes.

One self-contained file: `index.html`. Open it in a browser, or serve it:

```
python3 -m http.server 8000
```

No build step, no dependencies, no backend. Fonts come from Google Fonts; if the
CDN is unreachable the page falls back to system fonts and works the same.

## The three parts

**1. The vibe engine.** Your words are tokenized and scored against eleven vibe
profiles (cozy, dark academia, y2k, coastal, cottagecore, minimal, plant jungle,
gamer, pastel, mid-century, boho). It handles negation — "no neon", "nothing on
the walls" — by banning the three words after a negation cue and subtracting
hard from anything matching. Every pick shows the words that earned it a spot,
so the matching is never a black box. Chips, accent color and budget feed the
same scoring pass.

The winning vibe sets the room itself, not just the shopping list: wall paint,
floor type (carpet / wood / seagrass), and the color of the light. If the top
two vibes are close, the wall and floor colors are mixed between them.

**2. The room.** A cutaway dorm — back wall, window, radiator, twin bed frame,
desk — drawn as SVG, with thirteen slots things can live in (wall over the bed,
wall middle, window, ceiling, bed, under-bed, rug, three desk spots, chair, two
floor corners). Every product is drawn procedurally from its colors and a
variant name, so the room is stylistically coherent instead of being a collage
of product photos on white backgrounds. Drag anything to move it, click to
select, arrow keys to nudge, `+`/`−` to resize. Night mode dims the room and
lights up every lamp, candle and screen that's actually in it.

**3. The products.** ~65 items across nine zones, each with a typical price and
a real store to search. Prices are ballpark figures for that *kind* of item —
"Find it online" runs a live search at Target / IKEA / Urban Outfitters / etc.
so you get what's actually in stock rather than a stale fake listing.

## Pulling in real products

The left panel takes any product URL. The page's title, price and photo are read
through a public metadata reader (microlink, falling back to r.jina.ai), and the
result drops into the room as a photo card that stands in the scene. Plenty of
big retailers block cross-origin readers — when that happens the panel says so
and you can type the name and price yourself, or paste the image address
directly (right-click the product photo → "Copy image address"), which always
works.

Anything you pull in is pinned: re-designing around a new vibe rearranges
everything else but leaves your own finds where they are.

## Saving

- **Share** copies a URL with the whole room encoded in the hash.
- **PNG** exports the room as a 2400×1640 image. Photos pulled from other sites
  won't survive the export — SVG rendered to canvas can't fetch remote images —
  but everything drawn by the app does.
- The room autosaves to `localStorage` and comes back on reload.

## Iteration knobs

| What | Where |
|---|---|
| Vibes, their keywords, and what they do to the room | `VIBES` |
| Where things sit in the room | `SLOTS` (`x`/`y` is the bottom-centre anchor) |
| The catalog | `CATALOG` — `z` zone, `d` draw routine, `v` variant, `t` tags |
| How a product is drawn | the `D.*` routines; each returns `{w,h,s}` |
| The room shell | `shellSVG`, `floorSVG`, `windowSVG`, `furnitureSVG` |
| Matching weights | `scoreProduct` |
| Which slots get filled first when money is tight | `FILL_ORDER` |

Adding a product is one line in `CATALOG` if an existing draw routine fits.
Nudging a single item's default position is `ox` / `oy` on the product.

## Known edges

- One product, one colorway. A rust bean bag stays rust in a pastel room; swap
  it or set an accent color.
- Tall floor pieces can overlap wall art in the middle of the room. Drag them,
  or hit **Re-center**.
- The catalog is hand-written, not a live feed. It's a styling tool, not a
  shopping cart.
