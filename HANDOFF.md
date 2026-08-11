# Handoff: publish the design-duel site to GitHub Pages

## Task
Publish the contents of this folder to `Lestam/JulRoute` on the `gh-pages` branch and enable GitHub Pages for it.

## What this folder is
A static site — no build step, no package manager, no dependencies.

- `index.html` — the app: pairwise side-by-side comparison of souvenir poster layouts, single-elimination bracket, keyboard nav (← / →), progress saved in localStorage.
- `support.js` — the runtime `index.html` loads (must sit next to it).
- `designs/*.html` — 29 standalone layout files, each one poster; loaded by `index.html` in iframes.
- `designs/assets/` — PNG map plates, relief renders, `tracks.js` used by the design files.
- `.nojekyll` — required so GitHub Pages serves files/folders as-is.

All internal links are relative and lowercase-hyphenated. Nothing fetches anything at runtime except Google Fonts.

## Steps
```bash
cd <this folder>
git init -b gh-pages
git remote add origin https://github.com/Lestam/JulRoute.git
git add .
git commit -m "Design duel: side-by-side layout picker"
git push -u origin gh-pages
```

Then enable Pages (either is fine):

```bash
gh api -X POST repos/Lestam/JulRoute/pages \
  -f source[branch]=gh-pages -f source[path]=/
```

or in the UI: **Settings → Pages → Source: Deploy from a branch → branch `gh-pages`, folder `/ (root)` → Save**.

Published URL: **https://lestam.github.io/JulRoute/**

## Acceptance checks
1. `https://lestam.github.io/JulRoute/` loads and shows two posters side by side.
2. Clicking a poster (or pressing ←/→) advances to the next pair; the round counter increases.
3. No 404s in the network panel — in particular `support.js`, `designs/10a-framed-landscape-side-panel.html`, `designs/assets/relief-1.png`.
4. Playing through to the end shows a winner screen with a top-10 ranking.

## Notes / gotchas
- Do NOT rename files: `index.html` references `designs/<slug>.html` by exact name.
- `.nojekyll` must stay committed; without it Pages can skip files.
- First deploy takes 1-2 minutes to go live.
- If the repo later gets a `main` branch with other content, leave `gh-pages` as the publishing branch.
