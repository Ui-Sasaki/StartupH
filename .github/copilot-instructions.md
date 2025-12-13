# Copilot instructions for StartupH static site

This is a small single-page static website (HTML + CSS + assets). Below are concise, actionable instructions an AI coding agent should follow to be immediately productive editing or extending this repository.

1) Big-picture
- Single-page static site: `index.html` is the entrypoint and `style.css` contains all styling. No build system, no JS frameworks. The `images/` folder holds all media (jpg, png, mp4).
- Visual hero uses a video background (`images/background.mp4`) with a `poster` image (`images/background.jpg`) and decorative images (stickers, tapes). The hero layout and overlay are implemented with CSS classes like `.hero`, `.hero__video`, `.hero__vignette`, and `.hero__scanlines`.

2) Where to change common content (concrete examples)
- Headline and copy: edit the HTML inside `<div class="paperText">` in `index.html`. Example: the main sentence is inside `<h1><span class="rainbowText">…</span></h1>`.
- Replace/remove decorative images: files are in `images/` and referenced directly in `index.html` (e.g. `images/bear.PNG`, `images/kimono.png`). Keep filename case exactly as used — filenames are referenced with mixed case in the repo.
- To edit the gradient/rainbow effect: modify the `.rainbowText` rules in `style.css` (linear-gradient stops defined there).
- To change the hero behavior on mobile: see media queries at `@media (max-width: 820px)` and `@media (max-width: 480px)` in `style.css` — e.g. the video is hidden on small screens (`.hero__video { display:none; }`).
- Optional cards section: `div.projectsRow` contains placeholder cards. Remove it if unused.

3) Project-specific patterns & conventions
- No build step — changes are previewed by opening `index.html` or serving the folder locally.
- CSS variables: `:root{ --maxw: 1200px; }` used for layout width — prefer `var(--maxw)` when adding container rules.
- Decorative/assistive accessibility: many decorative images include `aria-hidden="true"` and empty `alt=""`. Preserve `aria-hidden` for purely decorative assets; provide meaningful `alt` for any content-critical images.
- Mixed filename casing: the repo includes `.PNG`, `.png`, `.jpg`, `.mp4` variations. Do not change file name case unless you update the corresponding reference in `index.html` (important on case-sensitive servers).
- Minimal JS: the site does not currently include a JS bundle — adding behavior should be done with a single vanilla JS file and added with a `<script>` tag at the end of `index.html` if needed.

4) Dev / preview commands (no CI/build present)
- Quick local preview (Python builtin server):

```bash
# from repo root
python -m http.server 8000
# then open http://localhost:8000
```

- Alternative: install and use an editor Live Server extension (VS Code Live Server) or `serve` (npm):

```bash
npm install -g serve
serve -s .
```

5) Media & performance notes discovered in repo
- Background video can be large. If editing or replacing it, consider providing a smaller-sizes fallback or using a compressed format for faster load.
- Poster image `images/background.jpg` is used for browsers that block autoplay. Keep it in sync when changing the video.
- Mobile: video is turned off in CSS media queries — test on small viewports to verify layout adjustments.

6) What to look for when changing visuals
- Check z-index stacking: hero video is behind overlays (`z-index:0`) and `.hero__vignette`/`.hero__scanlines` provide overlays (higher z-index). When adding new overlays, follow the current z-index conventions (video 0, overlays 1–2, content 10–20).
- Paper/card area: `.paperWrap` uses `aspect-ratio` with responsive adjustments — maintain aspect-ratio changes inside `@media` queries.

7) Contract (what automated edits should preserve)
- Input: small change description (e.g., "update headline text", "replace hero video", "add project card").
- Output: updated `index.html` and/or `style.css` and any new assets under `images/` with matching references.
- Error modes: missing asset files (404) and filename case mismatches; large media causing layout slowdown. If an asset is referenced but missing, add a note in the PR and do not remove the reference without replacing it.

8) Edge-cases & checks for PRs
- Verify all referenced images exist and casing matches.
- Confirm the hero still serves an accessible experience: poster image present and text still readable over overlays.
- Mobile checks: confirm `.hero__video` fallback and that decorative elements hidden via media queries do not overlap essential content.

9) Files to reference when making changes
- `index.html` — primary entry; where content and asset references live.
- `style.css` — layout, responsive rules, and visual effects (e.g., `.rainbowText`, `.hero__video`, media queries).
- `images/` — all visual assets (maintain file names and case).

If anything is missing, unclear, or you want additional automation (e.g., an npm preview script or a small test), tell me which parts to expand and I will update this file.
