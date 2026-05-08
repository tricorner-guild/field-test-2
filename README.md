# AMBRÔUGH

> *The land that remembers.*

A location-based mobile adventure game built on real-world GPS. Walk your actual neighborhood. Uncover a layered fantasy world beneath it.

---

## What It Is

AMBRÔUGH is a GPS-driven exploration game with no AR — just a handcrafted pixel-art map that reveals itself as you move through the real world. Think Pokémon Go's movement loop crossed with the depth of an old-school RPG. Your city is the dungeon. Your walk to the corner store is the expedition.

The map is generated from real OpenStreetMap data and rendered with a custom Bayer-dithered pixel engine — every tree, road, and water feature drawn procedurally in a dark fantasy palette. Fog of war covers everything you haven't walked yet. The world unfolds behind you.

---

## Core Loop

1. **Walk** — GPS moves your character on the pixel map in real time
2. **Discover** — Quest points spawn along roads near you; fog of war lifts as you explore
3. **Encounter** — Reach a quest point to trigger a narrative encounter (a fallen deer, an abandoned campsite, a book left in the road)
4. **Earn Gold** — Complete mini-games to collect gold and progress
5. **Repeat under pressure** — Taxes and debts create urgency. Grind solo, or group up for faster rewards

---

## World Structure

Player identity unfolds in four layers:

| Layer | What It Is |
|---|---|
| **Occupation** | Your first identity — Quarryman, Scholar, Shepherd, Servant, or Courier |
| **Oath** | Your calling. Shapes how you act and what you notice |
| **Faction** | Your community. Group identity that drives cooperative play |
| **Dragon** | The endgame revelation — which of the five dragons your path has always served |

Dragons are hidden until late game. The world is older than the player knows.

---

## Technical Overview

The field test is a single-file web app (`field-test/index.html`) deployed via Netlify. No backend. No native app. Just GPS, canvas, and OpenStreetMap.

**Stack:**
- [Leaflet.js](https://leafletjs.com/) — map engine and GPS layer
- [Overpass API](https://overpass-api.de/) — live OSM data (roads, buildings, water, parks)
- Canvas 2D — custom pixel renderer with Bayer 4×4 ordered dithering
- Vanilla JS — no framework dependencies

**Renderer highlights:**
- Fourier harmonic blob generation for organic forest shapes
- Bayer ordered dithering for pixel-cluster edges (three density zones: core / mid / edge)
- Zoom-scaled pixel size — cell size adjusts with zoom so the footprint stays constant in real-world metres
- Road polygons rasterized on the same canvas as the forest, dithered edges let tree pixels bleed through for an organic fringe
- Fog of war canvas overlay with circular punch-outs per visited position, persisted to `localStorage`

**Quest generation:**
- Candidates sampled every 15m along walkable road centrelines (no major roads)
- Three proximity zones: Immediate (0–100m, ≥15m spacing), Near (100–400m), Far (400–1600m)
- All quest points are on accessible streets — never in private land or water

---

## Field Test Files

| File | Purpose |
|---|---|
| `field-test/index.html` | Main app — live GPS, full game loop |
| `field-test/pixel-test/` | Renderer sandbox — tune dither, blob shape, pixel size |
| `field-test/road-test/` | Road shape sandbox — width wobble, node spacing, variety |
| `field-test/neighborhood-test/` | Hardcoded location test — no GPS required (serves via HTTP) |

---

## Running Locally

The field test fetches from the Overpass API and requires HTTP — `file://` will block cross-origin requests.

```bash
cd field-test
python3 -m http.server 8743
# open http://localhost:8743
```

Grant location access when prompted. The map loads around your real position.

---

## Status

Active alpha development. The renderer, quest system, fog of war, and basic game loop are functional. Mini-games, faction mechanics, and the full identity arc are designed but not yet implemented in the field test.

---

## Design Philosophy

No AR. No real-world recreation. Every landmark becomes a narrative object — the church becomes an abandoned well, the park becomes a clearing where something happened. The game is a layer over reality, not a mirror of it.

The pixel aesthetic isn't nostalgia — it's intentional abstraction. The world should feel discovered, not displayed.
