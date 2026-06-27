# RAPT Resource Catalog — Handoff

A discovery-first catalog of FEMA RAPT's operational layers — find, filter, preview on a map, and add to your own map. Presented as a full-screen modal inside a faithful RAPT shell (a concept/demo of what's possible beyond Experience Builder's toggle list). Also pitched to FEMA's EB team as "host one HTML file, embed the link."

## Protected route note

- The FEMA version at `/` and `/options` is the original client/demo surface. Do not re-theme or repurpose those files for Red Cross.
- The Red Cross Experience Builder version lives separately at `/redcross` (`redcross.html`). Use `/redcross?embed=1&open=1` for an ArcGIS Experience Builder Embed widget so the catalog fills the iframe without the stand-alone shell.

## Where it lives
- **Live (canonical):** https://catalog.jbf.com  — also https://rapt-catalog.vercel.app
- **Design-options page (client/EB review):** https://catalog.jbf.com/options — opens the catalog full-screen; the format switcher (Master-Detail / Cards / Table) is hidden behind a toggle on the **FEMA logo**.
- **Repo:** https://github.com/franzenjb/rapt-catalog (public). On the M4: `git clone` it (or pull) — GitHub is fully up to date.
- **Vercel:** project `rapt-catalog` (`prj_82yXOqDTWkXDXvcsEQkwF9dD9PmA`, team `jbf-2539-e1ec6bfb`).
- **Files:** `index.html` (the FEMA app — no build, no framework; fonts + ArcGIS Maps SDK from CDN). `options.html` (the FEMA embed-comparison wrapper). `redcross.html` (separate Red Cross-themed Experience Builder embed version).

## What's built
- **Three layouts** of the catalog: master-detail (list + detail), card grid, dense table. Toolbar toggle, plus a **Design Options** panel that drops down when you click the FEMA logo (`toggleDesign`/`pickView`). `?view=list|cards|table` + `?open=1` deep-links/auto-opens a view (used by the /options iframes).
- **Filters stack (intersection):** category segment + multiple topic facets + search all AND together (`curTags` Set). Removable filter chips in the result bar.
- **Map preview + Build-my-map** with 3 export paths (Map Viewer deep link · Add-by-URL stepper · Web Map JSON / recipe link).
- **Light/Dark** toggle (persists via localStorage). Full-screen modal over the RAPT shell; overlay is `top:56px;bottom:30px` to clear the fixed RAPT footer.
- **"How this was built" modal** (for the FEMA / Experience Builder team) — opened from the launch-card **"How this was built"** button and the catalog header **"How it's built"** button. It is now ONE flowing document (no accordions): 3 plain-English steps + a "try it yourself" callout → What it actually is → Adding it (Embed widget + your link) → Hosting it is easier than it sounds → Why it's safe & easy to keep. Core message: an AI wrote the HTML on a plain-English request (no hand-coding); host the file where you already host fema.gov-type content; embed the **link** (EB Embed widget, URL mode — not the paste-HTML/Code box). The custom-EB-widget / Developer-Edition / React route was removed as confusing and niche.
- **Red Cross version** (`/redcross`) keeps the catalog functions and source layer inventory but changes the visible shell to Red Cross operations language, red/charcoal styling, a staff-facing "How to use" guide, separate localStorage keys, and `?embed=1&open=1` mode for Experience Builder iframes.

## Data (real)
106 layers pulled from RAPT's published web map item `7102d5a506514b55b880d7f62841f9a7` (FEMA org `XG15cJAlne2vxtgt`), inlined as `RAPT_DATA = [title, serviceURL, itemId, layerType, category, geography, tags[], realtimeBool]`. 31 are URL-only NWS/NOAA live feeds (no item id → export option 1 can't deep-link them; use 2/3). Re-pull: GET `https://www.arcgis.com/sharing/rest/content/items/7102d5a506514b55b880d7f62841f9a7/data?f=json` → `operationalLayers`.

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

## Domain wiring (done — for reference)
- Cloudflare CNAME `catalog → cname.vercel-dns.com` (DNS-only) in zone jbf.com (`e3fbaac1c772786700d37440be8383d6`).
- Vercel: `catalog.jbf.com` attached to the `rapt-catalog` project (verified). Cert minted via `npx vercel certs issue catalog.jbf.com`.
- The old `maps.jbf.com/rapt-catalog` subpath was REMOVED from the `cvs` project (Jeff wanted it unrelated to maps). NOTE: `cvs` does NOT auto-deploy on git push — must `npx vercel --prod` from `~/dev/cvs`.
- `rapt.jbf.com` is TAKEN (the `rapt-4` "RAPT 4 Command Console"); `rapt4.jbf.com` is taken (answer-engine).

## Open items / next (for the M4 session)
1. **OPEN DECISION (Jeff to confirm):** should the app open in **dark mode + cards by default**? He asked for this; pending a yes/which. If yes: default `viewMode='cards'` + apply `.dark` on open. (He may instead just want a screenshot of dark+cards — the harness was down here, see #2.)
2. **Browser harness was DEAD the entire 2026-06-26 session** (Chrome disconnected — every `browser-harness` call printed its usage banner, never a screenshot). On the M4, confirm Chrome + the harness work so the app can actually be screenshotted/verified live.
3. Rebuild the M4's `~/dev/projects.json` registry to pick up this repo + catalog.jbf.com.

## Continue in Claude Code (on the M4)
Clone the repo, open it, run `claude`, then: *"Read HANDOFF.md and pick up the open items."* Everything needed is in `index.html`, `options.html`, and this file. (Prior local path was `~/DEV/rapt-catalog` on Mac.localdomain; use `~/dev/rapt-catalog` on the M4.)
