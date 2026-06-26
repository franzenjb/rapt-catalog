# RAPT Resource Catalog — Handoff

A discovery-first layer catalog presented as a **modal inside FEMA's RAPT tool**. Concept/demo to show what the ArcGIS Maps SDK makes possible beyond Experience Builder's toggle list.

## Status
- **Live:** https://rapt-catalog.vercel.app  (Vercel project `rapt-catalog`, scope `jbf-2539-e1ec6bfb`)
- **Folder:** `~/Desktop/rapt-catalog` (move to `~/dev/rapt-catalog` for the standard pipeline)
- **Single file:** `index.html` — no build step, no framework. Fonts + ArcGIS SDK load from CDN.

## What's built
- Lands on a faithful **RAPT shell** (Contents Pane, Tools/Presets Pane, basemap) with a highlighted **Layer Catalog [NEW]** launcher. Three launch points wired: nav item, center card, footer "RAPT Resource Center".
- **Catalog tab:** 106 real RAPT layers as cards (circular FEMA badge, category top-stripe, LIVE flags). Full-text search + category pills + tag cloud. Checkbox-style "Add to map".
- **Map tab:** live ArcGIS `MapView`; selected layers render with visibility toggle, opacity, legend, basemap toggle (lower-right).
- **Selection tray** (docked, RAPT navy) → **Build my map** dialog with 3 export paths:
  1. Open in ArcGIS Map Viewer (deep link, item-backed layers only)
  2. Add by URL — one-at-a-time stepper (Map Viewer's Add→URL takes one endpoint per paste)
  3. Download Web Map JSON + shareable recipe link (selection encoded in URL hash)
- **Light/Dark toggle** (head, top-right; persists via localStorage).

## Data provenance (real, not mocked)
- Layers pulled from the live RAPT web map item **`7102d5a506514b55b880d7f62841f9a7`**
  (behind RAPT WAB app `90c0c996a5e242a79345cdbc5f758fc6`, FEMA org `XG15cJAlne2vxtgt`).
- 106 operational layers inlined as `const RAPT_DATA = [...]` near the bottom of `index.html`:
  `[title, serviceURL, itemId, layerType, category, geography, tags[], realtimeBool]`.
- 21 unique item IDs; 31 layers are URL-only (NWS/NOAA real-time feeds) — these have no item ID, so export option 1 can't deep-link them (use option 2/3). This is correct behavior, not a bug.
- Re-pull/refresh: GET `https://www.arcgis.com/sharing/rest/content/items/7102d5a506514b55b880d7f62841f9a7/data?f=json`, read `operationalLayers`.

## Design tokens (FEMA, not cream)
- Navy chrome `#1c4063`/gradient · gold accent `#bfa23f` · category colors: Hazards `#c33d2e`, Infrastructure `#2766a3`, People `#0f8285`.
- Fonts: Libre Baskerville (titles), Source Sans Pro (body), IBM Plex Mono (small data).
- Dark theme overrides live under `.bigmodal.dark` in the `<style>` block.

## Deploy
```
cd ~/Desktop/rapt-catalog   # or ~/dev/rapt-catalog
npx vercel --prod --scope jbf-2539-e1ec6bfb --yes
```
**Clean URL** `maps.jbf.com/rapt-catalog`: add to the `cvs` project's `vercel.json` (swap in the rapt-catalog.vercel.app host), then commit+push cvs:
```
{ "source": "/rapt-catalog",        "destination": "https://rapt-catalog.vercel.app/" },
{ "source": "/rapt-catalog/:path*", "destination": "https://rapt-catalog.vercel.app/:path*" },
```
Check existing cvs rewrites first; single-file app so no asset-path breakage.

## Open items / next
1. Dark theme contrast pass (currently a first cut — tune card vs. body separation).
2. Map preview uses default symbology; carry each layer's RAPT renderer from the web map JSON for fidelity.
3. Maximize item-ID coverage so export option 1 works for more selections.
4. Wire the `cvs` rewrite → `maps.jbf.com/rapt-catalog`.
5. Register in `~/dev/projects.json` + memory once the clean URL is live.

## Continue in Claude Code
Open this folder and run `claude`, then: *"Read HANDOFF.md and pick up the open items."*
Everything needed is in `index.html` + this file.
