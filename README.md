# Workout Finder

A simple web app that helps a gym beginner figure out what to do: tap a muscle
group on a diagram of the human body and get a list of beginner-friendly
exercises that train it, each with an **embedded YouTube demo** of proper form
that plays right in the page.

Built for the author's girlfriend, who recently started going to the gym and is
sometimes unsure which exercises to do.

---

## Status: v2, working

The whole app is a **single self-contained file: [`index.html`](index.html)**.
No build step, no dependencies, no framework. Open it in any browser, or host it
anywhere static.

Everything below has been built and verified working on desktop **and** mobile
(front/back view switching, clicking muscle regions, the quick-pick chips, the
exercise panel, and the inline video players).

### What changed since v1
- **Embedded videos.** Tapping **▶ Watch** now expands a YouTube player inline
  instead of opening a search page. Each exercise has a hand-picked video ID.
  (See design decision #1 — this replaces the old "open a search query" behavior.)
- **Female figure.** The body diagram was redrawn as an average-build female
  figure (hourglass silhouette, hair, anatomically-placed muscle bellies with
  separation detail lines), replacing the original stylized unisex dummy.
- **Mobile-first sizing.** On phones the figure is height-capped so the figure
  **and** all the quick-pick chips fit on one screen without scrolling.

---

## How to run / share

It's a static file, so any of these work:

- **Locally:** double-click `index.html`, or run a static server in the folder:
  ```bash
  python3 -m http.server 4173
  # then open http://localhost:4173
  ```
- **GitHub Pages (how the author plans to share it):** push the folder to a repo,
  go to Settings → Pages, set source to the `main` branch root. The app will be
  served at `https://<user>.github.io/<repo>/`.
- Or drop it on Netlify / any static host.

> `.claude/launch.json` is configured to serve the folder with
> `python3 -m http.server 4173` for the Claude Code preview tool. Harmless to
> keep or delete.

> **Custom domain note:** an earlier attempt to point `www.hannahsworkout.com` at
> GitHub Pages failed only because **the domain is not registered yet** (it has no
> DNS records). Buy the domain first, then add a `CNAME` DNS record
> (`www` → `<user>.github.io`) and a `CNAME` file in the repo containing the domain.

---

## What it does (features)

- **Body diagram** with a **Front / Back** toggle (two inline SVG figures, female
  form). Major muscle regions are clickable and highlight when selected.
- **Quick-pick chips**: a labeled button for every muscle group below the figure.
  These exist because (a) tapping small SVG regions on a phone is fiddly and
  (b) a beginner may not know where a given muscle is. The chips are the most
  reliable way to navigate; the SVG is the nice-to-have visual.
- **Exercise panel**: when a group is selected it shows that group's exercises,
  each with a plain-English description, a **Beginner / Intermediate** badge, and
  a **▶ Watch** button.
- **Inline video**: **▶ Watch** expands a YouTube player directly under the
  exercise (one open at a time; tap again or pick another to switch). Each player
  also has a "Not the right video? Search YouTube ↗" fallback link.
- **Mobile-first** dark UI (she uses it on her phone at the gym). On narrow
  screens the figure is height-capped and results auto-scroll into view.

### Covered muscle groups (13)
chest, back (lats), shoulders, biceps, triceps, forearms, core (abs),
lower back, traps, glutes, quads, hamstrings, calves — 56 exercises total,
weighted toward machines / dumbbells / bodyweight that are easy to learn.

---

## Key design decisions (please preserve unless asked otherwise)

1. **YouTube "Watch" buttons embed a curated, hard-coded video per exercise.**
   Each exercise in `DATA` has a `yt` field holding an 11-character YouTube video
   ID, rendered as a `youtube-nocookie.com/embed/<id>` iframe. The IDs were picked
   as popular "proper form" tutorials and each was checked to be embeddable.
   - **Why curated and not the live YouTube API:** to keep the single-file,
     zero-setup, zero-API-key design (chosen over a "fetch the highest-view video
     live" approach that would need a Google API key exposed in the public file).
   - **Dead-link safety:** every player keeps a fallback link that opens a YouTube
     **search** (`...youtube.com/results?search_query=...`), so if a hard-coded
     video is ever removed the user can still find a replacement. The old v1
     behavior was search-only links; that lives on as this fallback.
2. **Major muscle groups only** (not individual muscles like medial vs lateral
   head) — kept simple because the user is a beginner.
3. **Single self-contained HTML file**, no build tooling — chosen for trivial
   sharing and GitHub Pages compatibility. Don't introduce a framework/build
   step unless the user wants ongoing feature growth.
4. **Chips + SVG both drive the same `show(group)` function** and stay in sync
   (selecting via either highlights both the chip and the muscle region).
5. **Female, average-build figure** in a dark theme with tap-to-highlight (gray
   muscle → red when selected). It deliberately is **not** a color-coded medical
   chart; keep the interaction model unless asked otherwise.

---

## Code map (all inside `index.html`)

- **`<style>`** — all CSS. Theme variables are at the top under `:root`
  (`--accent` is the red/coral brand color). Layout is a 2-column grid that
  collapses to 1 column under 760px.
  - `@media (max-width: 760px)` caps the figure with
    `svg.body.on { height: 46vh; width: auto; }` so the chips stay on-screen.
  - `.detail` = thin separation lines (pec split, ab grid, spine). `.hair` = the
    hair shape. `.ex-wrap` / `.player` = the inline video container (`.player.open`
    reveals it; the `iframe` is `aspect-ratio: 16/9`).
- **Two `<svg>` figures** — `#svg-front` and `#svg-back`, `viewBox="0 0 300 560"`.
  Clickable parts have `class="muscle"` and a `data-group="..."` attribute.
  Non-interactive body outline parts use `class="body-base"` (or `class="hair"`).
  Thin `class="detail"` paths add muscle separation lines. The SVGs are stylized
  (hand-authored bezier paths, anatomically placed), not anatomically precise.
- **`<script>`**:
  - **`DATA`** — the single source of truth for content. An object keyed by
    group id; each entry has `title`, `sub` (subtitle), and `ex` (array of
    `{ name, desc, level, yt }`, where `yt` is the YouTube video ID).
    **Edit this object to add/remove/change exercises or swap a video.**
  - **`show(group)`** — renders the exercise panel (each row wrapped in
    `.ex-wrap` with a hidden `.player`), highlights the matching muscle region(s)
    and chip.
  - **`toggleVid(btn)`** — expands/collapses the inline player for a row, builds
    the embed iframe + fallback link, and enforces "only one open at a time."
  - **`YT`** — base search URL, now used only for the per-player fallback link.
  - **`setView('front'|'back')`** — toggles which SVG and toggle button is active.
  - **`CHIP_ORDER`** — array controlling the order chips appear; chips are built
    from `DATA` at the bottom of the script.

### To add a new exercise
Find the group in `DATA` and add an object to its `ex` array. It needs a `yt`
video ID:
```js
{ name: "Incline Push-up", desc: "Hands on a bench, easier than the floor.",
  level: "Beginner", yt: "VIDEO_ID_HERE" }
```
Rendering, the embed, and the fallback link are all automatic.

### To find / swap a video ID
1. Find a good "proper form" tutorial on YouTube; the ID is the `v=` part of
   `https://www.youtube.com/watch?v=VIDEO_ID`.
2. **Verify it's embeddable** before trusting it (some videos disable embedding,
   and IDs can 404). Quick check via the oEmbed endpoint — a `200` with JSON title
   means it exists and is embeddable:
   ```bash
   curl -s "https://www.youtube.com/oembed?format=json&url=https://www.youtube.com/watch?v=VIDEO_ID"
   ```
3. Paste the ID into the exercise's `yt` field.

> ⚠️ Never invent video IDs. Every `yt` value must be a real ID you verified with
> the oEmbed check above — a wrong ID silently embeds the wrong video or a dead
> player. (All current IDs were validated this way.)

### To add a whole new muscle group
1. Add an entry to `DATA` with a new `group` id (with `yt` IDs on each exercise).
2. Add a clickable shape with `class="muscle" data-group="<id>"` to the front
   and/or back `<svg>`.
3. Add the `<id>` to `CHIP_ORDER`.

### To edit the body figure
Both SVGs share the same hand-authored coordinate system (`viewBox 0 0 300 560`,
centered on x=150). The base silhouette (`body-base` / `hair`) is drawn first,
then `muscle` regions overlay it, then thin `detail` lines on top. If you reshape
the silhouette, move the overlapping `muscle` paths to match. Verify visually at
both desktop and mobile widths (the mobile height cap changes how it reads).

---

## Ideas / backlog (not built)
- "Workout of the day" or a beginner full-body routine.
- Favorite / save exercises (localStorage).
- Set/rep guidance per exercise.
- Personalize (e.g. her name in the title).
- (Done in v2: embedded curated demo videos; female figure; mobile sizing.)
