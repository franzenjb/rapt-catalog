# Deploy: RAPT Resource Catalog → maps.jbf.com/rapt-catalog

Single static file (index.html). No build, no assets — fonts + ArcGIS SDK load from CDN.
Scope: jbf-2539-e1ec6bfb · maps.jbf.com = Vercel project `cvs` · DNS Cloudflare (zone e3fbaac1c772786700d37440be8383d6)

## 1. Stand it up as its own Vercel project (gives a live URL you can open on your phone)
    mkdir -p ~/dev/rapt-catalog && cd ~/dev/rapt-catalog
    # drop index.html + vercel.json from this bundle into the folder, then:
    npx vercel --prod --scope jbf-2539-e1ec6bfb --yes

First run prompts: link existing? n · name? rapt-catalog · dir? ./ · override? n
→ You get https://rapt-catalog-<hash>.vercel.app  (open this on mobile immediately)

## 2. Wire the subpath into maps.jbf.com (edit the `cvs` project's vercel.json)
Add these two rewrites (replace the destination with your real .vercel.app URL from step 1):

    { "source": "/rapt-catalog",        "destination": "https://rapt-catalog-<hash>.vercel.app/" },
    { "source": "/rapt-catalog/:path*", "destination": "https://rapt-catalog-<hash>.vercel.app/:path*" }

Then in ~/dev/<cvs project folder>:
    git add vercel.json && git commit -m "Add /rapt-catalog rewrite" && git push
(auto-deploys cvs) → https://maps.jbf.com/rapt-catalog goes live.

## Notes
- Single-file app → subpath rewrites work with no asset-path rework.
- Register in projects.json + memory after it's live.
