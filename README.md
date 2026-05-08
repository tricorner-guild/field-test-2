# Field Test

A browser-based prototype that renders a pixel-art map of your real-world surroundings using OpenStreetMap data and live GPS.

**[Live demo →](https://tricorner-guild.github.io/field-test-2/)**

---

## What It Does

- Pulls live OpenStreetMap data (roads, buildings, water, parks) via the Overpass API around your current GPS position.
- Renders the surrounding area as a custom pixel-art map on a Canvas 2D layer using Bayer 4×4 ordered dithering.
- Tracks your GPS location and updates a player marker in real time.
- Lifts a fog-of-war overlay as you walk — visited tiles are persisted in `localStorage`.
- Spawns waypoints along walkable road centrelines, distributed across three proximity bands.

---

## Stack

- [Leaflet.js](https://leafletjs.com/) — map engine and GPS layer
- [Overpass API](https://overpass-api.de/) — live OSM data
- Canvas 2D — pixel renderer
- Vanilla JavaScript — no framework dependencies

---

## Renderer Notes

- Fourier harmonic blob generation for organic shapes
- Bayer ordered dithering with three density zones: core / mid / edge
- Zoom-scaled pixel size — cell size adjusts with zoom so the footprint stays constant in real-world metres
- Road polygons rasterized on the same canvas as fill regions, with dithered edges
- Fog of war drawn as a canvas overlay with circular punch-outs per visited position, persisted to `localStorage`

---

## Waypoint Generation

- Candidates sampled every 15m along walkable road centrelines (major roads excluded)
- Three proximity bands: Immediate (0–100m, ≥15m spacing), Near (100–400m), Far (400–1600m)
- All waypoints are on accessible streets — never in private land or water

---

## Files

| File | Purpose |
|---|---|
| `field-test-v1.html` | Main prototype |
| `index.html` | Redirect to the current version |

---

## Running Locally

The prototype fetches from the Overpass API and requires HTTP — opening the file directly via `file://` will block cross-origin requests.

```bash
python3 -m http.server 8743
# open http://localhost:8743
```

Grant location access when prompted.

Or just visit the [live demo](https://tricorner-guild.github.io/field-test-2/).

---

## Status

Active prototype.
