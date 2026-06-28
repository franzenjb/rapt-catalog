# RAPT Resource Catalog — Handoff

A discovery-first catalog of FEMA RAPT's operational layers — find, filter, preview on a map, and add to your own map. Presented as a full-screen modal inside a faithful RAPT shell (a concept/demo of what's possible beyond Experience Builder's toggle list). Also pitched to FEMA's EB team as "host one HTML file, embed the link."

## Protected route note

- The FEMA version at `/` and `/options` is the original client/demo surface. Do not re-theme or repurpose those files for Red Cross.
- The Red Cross Experience Builder version lives separately at `/redcross` (`redcross.html`). Use `/redcross?embed=1&open=1` for an ArcGIS Experience Builder Embed widget so the catalog fills the iframe without the stand-alone shell.
- Domain split: `catalog.jbf.com` remains the FEMA/client-demo URL. `raptrc.jbf.com` is the Red Cross URL and Vercel redirects that host root to `/redcross`.

## Where it lives
- **FEMA live (canonical):** https://catalog.jbf.com  — also https://rapt-catalog.vercel.app
- **Red Cross live:** https://raptrc.jbf.com
- **Design-options page (client/EB review):** https://catalog.jbf.com/options — opens the catalog full-screen; the format switcher (Master-Detail / Cards / Table) is hidden behind a toggle on the **FEMA logo**.
- **Repo:** https://github.com/franzenjb/rapt-catalog (public). On the M4: `git clone` it (or pull) — GitHub is fully up to date.
- **Vercel:** project `rapt-catalog` (`prj_82yXOqDTWkXDXvcsEQkwF9dD9PmA`, team `jbf-2539-e1ec6bfb`).
- **Files:** `index.html` (the FEMA app — no build, no framework; fonts + ArcGIS Maps SDK from CDN). `options.html` (the FEMA embed-comparison wrapper). `redcross.html` (separate Red Cross-themed Experience Builder embed version).

## What's built
- **Three layouts** of the catalog: master-detail (list + detail), card grid, dense table. Toolbar toggle, plus a **Design Options** panel that drops down when you click the FEMA logo (`toggleDesign`/`pickView`). `?view=list|cards|table` + `?open=1` deep-links/auto-opens a view (used by the /options iframes).
- **Filters stack (intersection):** category segment + multiple topic facets + search all AND together (`curTags` Set). Removable filter chips in the result bar.
- **Map preview + Build-my-map** with 3 export paths (Map Viewer deep link · Add-by-URL stepper · Web Map JSON / recipe link). FEMA Option 3 now includes `Copy Web Map JSON`; if browser clipboard access is blocked, the JSON is shown and selected in the modal for normal copy/paste.
- **FEMA Tribal-layer popup proof** is implemented in `index.html` only. Selecting a Tribal layer and previewing the map disables Esri default popups, styles Tribal polygons, and opens a formatted, resizable/retractable right-side app panel on Tribal feature click. It currently covers the FEMA ACS2022 Tribal indicator service and the general Tribal boundaries layer, with raw/internal schema fields hidden.
- **Light/Dark** toggle (persists via localStorage). Full-screen modal over the RAPT shell; overlay is `top:56px;bottom:30px` to clear the fixed RAPT footer.
- **"How this was built" modal** (for the FEMA / Experience Builder team) — opened from the launch-card **"How this was built"** button and the catalog header **"How it's built"** button. It is now ONE flowing document (no accordions): 3 plain-English steps + a "try it yourself" callout → What it actually is → Adding it (Embed widget + your link) → Hosting it is easier than it sounds → Why it's safe & easy to keep. Core message: an AI wrote the HTML on a plain-English request (no hand-coding); host the file where you already host fema.gov-type content; embed the **link** (EB Embed widget, URL mode — not the paste-HTML/Code box). The custom-EB-widget / Developer-Edition / React route was removed as confusing and niche.
- **Red Cross version** (`/redcross`, and `https://raptrc.jbf.com`) keeps the catalog functions and source layer inventory but changes the visible shell to Red Cross operations language, red/charcoal styling, a staff-facing "How to use" guide, separate localStorage keys, and `?embed=1&open=1` mode for Experience Builder iframes.
- **Red Cross source/provenance UI** is explicit: the 106 FEMA RAPT records are labeled as the RAPT / wrapped seed set, and the normal Red Cross catalog also includes the seven public `Master_ARC_Geography_FY26_January_2026` sublayers from `data/red-cross-master-geography-2026.js`. The previous 586-item `Franzen` working lane is hidden by default because it overwhelms the catalog and mostly contains item-backed/private candidates. It is surfaced only with the hidden `?franzen=1` URL flag for Jeff/workbench review. Future Red Cross authoritative records remain separate, with pending buckets for the 414-item authoritative review queue. `Franzen` is a working-layer lane, not a claim of formal Red Cross authority.
- **Red Cross map preview controls** now follow Jeff's ArcGIS map standard: Home, Zoom, Search, ScaleBar, BasemapGallery in an Expand widget, Legend, and default Esri popups disabled at the view/layer level.
- **Red Cross Tribal-layer popup proof** is implemented in `redcross.html`. Selecting a Tribal layer and previewing the map disables Esri default popups, styles Tribal polygons in Red Cross colors, and opens a formatted, resizable/retractable right-side app panel on Tribal feature click. The Red Cross Web Map JSON export also writes curated `popupInfo` for Tribal layers so Map Viewer imports do not fall back to raw `OBJECTID`/schema-field tables. These are editable starter popups, not locked service-level changes.
- **Red Cross metric card styling/popups** are implemented in `redcross.html` for county/tract/Tribal indicator cards. Selecting a metric card applies a Red Cross color-gradient renderer to that metric field in map preview and writes the same renderer into Option 3 Web Map JSON. Clicking a rendered county/tract metric feature opens the app-owned right-side feature panel with a curated selected-metric card instead of Esri's raw field dump; the exported `popupInfo` also hides raw schema fields like `OBJECTID`. Non-metric feature layers now get a generic curated fallback card/popup so the Red Cross route avoids default gray/white Esri popups beyond just Tribal layers. The feature panel uses fixed light-surface contrast tokens so it stays readable even when the catalog modal is in dark mode.
- **Red Cross logo/header pass** replaced the hand-drawn approximation with a vendored `assets/american-red-cross-logo.svg` asset and title-cased the source filter labels. The mobile header uses the same logo asset at a smaller width and hides the center title to avoid wrapping.
- **Red Cross Option 3** includes `Copy Web Map JSON` as the primary action, plus `Download .json` and `Copy selected-layer link`. The copy button exists so users can paste directly into ArcGIS Assistant without opening a downloaded JSON file in macOS.
- **Red Cross scroll fix** keeps the embedded catalog panes inside the modal viewport. The filter rail, list, cards, table, and detail pane now own their own scroll areas instead of letting 692 rows expand the grid beyond the iframe height.

## Data (real)
106 layers pulled from RAPT's published web map item `7102d5a506514b55b880d7f62841f9a7` (FEMA org `XG15cJAlne2vxtgt`), inlined as `RAPT_DATA = [title, serviceURL, itemId, layerType, category, geography, tags[], realtimeBool]`. 31 are URL-only NWS/NOAA live feeds (no item id → export option 1 can't deep-link them; use 2/3). Re-pull: GET `https://www.arcgis.com/sharing/rest/content/items/7102d5a506514b55b880d7f62841f9a7/data?f=json` → `operationalLayers`.

The Red Cross default catalog includes seven public Master Geography FY26 layers from AGOL item `8c6753c3c5814d05be65723cd4615d7c` / service `https://services.arcgis.com/pGfbNJoYypmNq86F/arcgis/rest/services/Master_ARC_Geography_2022/FeatureServer`: Chapter HQ, Division, Region, Chapter, State, County, and Puerto Rico response territories.

The hidden Red Cross `Franzen` lane is generated from local project-inventory AGOL details into `data/franzen-recent-feature-services.js`. Current scope is owner `ARC_Jeff.Franzen_825009`, item type `Feature Service`, and `ageDays <= 365` as of 2026-06-28. The file stores item IDs but intentionally leaves service URLs blank because public REST checks returned token/permission errors for protected ARC services. Do not treat these as public URL feeds until an authenticated export supplies service URLs and sharing status. The normal Red Cross route does not merge these records into the catalog; use `?franzen=1` only when Jeff needs the private workbench lane.

Red Cross catalog growth policy:
- Keep Red Cross master geography first: divisions, regions, chapters, counties/county equivalents, and FIPS-keyed hierarchy from the governed Red Cross master geography spine.
- Treat Red Cross "authoritative" AGOL items/services as candidates, not automatic catalog records. The known candidate queue is ~414, but it can include feature services, map services, image services, web maps, dashboards, apps, reports, imagery, and old incident/state products. The Red Cross page has an empty but clickable "Red Cross authoritative" filter bucket ready for reviewed feature/map/image service records.
- Only map-usable feature/map/image services should graduate into this layer catalog. Dashboards, web maps, apps, and reports belong in a separate reference inventory unless they expose usable layers.
- Shelter candidates need special scrutiny: old Arizona/North Carolina or other state-specific shelter services may be stale or redundant. Compare every shelter candidate against the master shelter layer before promotion.

## Design tokens
- Navy `#1c4063` / gold `#bfa23f`; category colors Hazards `#c33d2e`, Infrastructure `#2766a3`, People `#0f8285`. Neutral gray canvas (cream removed). One topic-icon set (`svgIcon`) shared by filters + cards.
- Fonts: **Source Sans Pro** (UI/titles — the old Libre Baskerville serif was dropped) + IBM Plex Mono (small data).
- Dark overrides under `.bigmodal.dark` in the `<style>` block.

## Deploy
```
cd ~/dev/rapt-catalog          # wherever cloned on the M4
npx vercel --prod --scope jbf-2539-e1ec6bfb --yes
```
catalog.jbf.com follows the project's current production deployment, so a prod deploy updates it. (Custom domain already attached + verified; TLS cert already issued.)
raptrc.jbf.com is attached to the same Vercel project. `vercel.json` redirects that host root to `/redcross`, while `catalog.jbf.com` continues to serve the FEMA `index.html`.

## Domain wiring (done — for reference)
- Cloudflare CNAME `catalog → cname.vercel-dns.com` (DNS-only) in zone jbf.com (`e3fbaac1c772786700d37440be8383d6`).
- Vercel: `catalog.jbf.com` attached to the `rapt-catalog` project (verified). Cert minted via `npx vercel certs issue catalog.jbf.com`.
- Cloudflare CNAME `raptrc → cname.vercel-dns.com` (DNS-only) in zone jbf.com.
- Vercel: `raptrc.jbf.com` attached to the `rapt-catalog` project; cert entry issued via `npx vercel certs issue raptrc.jbf.com`; the host redirect in `vercel.json` sends the root URL to the Red Cross route.
- The old `maps.jbf.com/rapt-catalog` subpath was REMOVED from the `cvs` project (Jeff wanted it unrelated to maps). NOTE: `cvs` does NOT auto-deploy on git push — must `npx vercel --prod` from `~/dev/cvs`.
- `rapt.jbf.com` is TAKEN (the `rapt-4` "RAPT 4 Command Console"); `rapt4.jbf.com` is taken (answer-engine).

## Open items / next (for the M4 session)
1. **OPEN DECISION (Jeff to confirm):** should the app open in **dark mode + cards by default**? He asked for this; pending a yes/which. If yes: default `viewMode='cards'` + apply `.dark` on open. (He may instead just want a screenshot of dark+cards — the harness was down here, see #2.)
2. Browser QA worked on 2026-06-28 for the Red Cross route via the in-app browser/local server. Verified desktop Tribal click opens the custom panel and shows no Esri popup; mobile source-banner layout was fixed after the first mobile smoke check exposed a narrow-column wrap.
3. Rebuild the M4's `~/dev/projects.json` registry to pick up this repo + catalog.jbf.com.

## Continue in Claude Code (on the M4)
Clone the repo, open it, run `claude`, then: *"Read HANDOFF.md and pick up the open items."* Everything needed is in `index.html`, `options.html`, and this file. (Prior local path was `~/DEV/rapt-catalog` on Mac.localdomain; use `~/dev/rapt-catalog` on the M4.)
