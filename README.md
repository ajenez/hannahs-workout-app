# Form

A mobile-first web app that helps a gym beginner figure out what to do: pick a
muscle group from a list and get beginner-friendly exercises that train it — each
with **sets/reps/rest guidance**, **form cues**, **common mistakes to avoid**, and
an **embedded YouTube demo** of proper form that plays right in the page.

Built for the author's girlfriend, who's a lifelong athlete (soccer — strong legs,
strong core) but new to the gym, so she can feel confident choosing exercises.

---

## Status: v3, working

The app is [`index.html`](index.html) plus a small [`assets/`](assets) folder of
images (the body-map artwork, added in Phase 4). No build step, no dependencies, no
framework, no backend — still a plain static site you can open in any browser or host
anywhere (GitHub Pages, Netlify, etc.).

Everything below has been built and verified working (muscle picker, equipment
filter, exercise cards with sets/reps/cues/mistakes, inline video players, saving
to favorites, routines, and tips).

### What changed since v2
- **Refactored from "tap a body diagram" into a 4-tab app.** The clickable SVG
  body figure was **removed entirely** — the muscle picker is now a region-grouped
  list of chips (Upper body / Core / Lower body), which is more reliable on a phone.
- **Richer exercise cards.** Each exercise now shows **sets · reps · rest**, a
  short list of **form cues**, **common mistakes**, the **gear** it needs, and the
  **secondary muscles** it works — not just a name + description.
- **Equipment filter.** Toggle which equipment is available (Bodyweight, Dumbbells,
  Cables, Machines, Barbell) and the exercise list filters to match.
- **Save / favorites.** A ♡ Save button on every exercise persists to
  `localStorage` and shows up under the **Saved** tab. Degrades gracefully in
  private/incognito mode (shows a heads-up banner instead of silently failing).
- **Routines tab.** Three hand-curated starter programs (Full body 3×/week, Glute
  focus, Upper/lower split). They reference exercises by key, so they stay in sync
  with the exercise data automatically.
- **Tips tab.** Short gym-confidence tips (etiquette, resting, starting light).

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

> `.claude/launch.json` is configured to serve the folder for the Claude Code
> preview tool. Harmless to keep or delete.

> **Custom domain note:** an earlier attempt to point a custom domain at
> GitHub Pages failed only because **the domain is not registered yet** (it has no
> DNS records). Buy the domain first, then add a `CNAME` DNS record
> (`www` → `<user>.github.io`) and a `CNAME` file in the repo containing the domain.

---

## What it does (features)

The app is five tabs, switched from a fixed bottom nav bar:

1. **Exercises** — the main screen.
   - **Exercises ⇄ Stretches toggle** at the very top. In **Stretches** mode the same
     muscle picker (body map + chips) shows that muscle's **warm-up (dynamic)** and
     **cool-down (static)** stretches as cards — video, hold time, how-to cues, target
     muscles, and a save button. (Equipment filter and focus sub-zones are hidden in
     this mode.)
   - **Body map** at the top — a clickable anatomical figure with **Female/Male** and
     **Front/Back** toggles. Tap a muscle to see its exercises; the map highlights the
     selection and auto-flips front/back to follow whatever you pick (even from the
     chips or search). It's the visual entry point. Muscles that have
     sub-sections (**glutes** → upper/overall, **abs** → upper/lower/obliques, **back**
     → lats/mid) split into tappable zones once selected, so you can drill into a
     section right on the figure (mirrors the focus chips). The ab obliques are
     approximate hand-placed waist zones since the art doesn't draw them.
   - **Search** — a global lookup over every exercise by name, muscle, gear, or
     equipment ("romanian deadlift", "glutes", "dumbbell"). Ignores the equipment
     filter so it always finds everything (and also matches machines).
   - **Equipment filter** grouped the way a beginner thinks: **Free weights**
     (dumbbells + barbell), **Machines & cables**, **Bodyweight**. All on by default;
     toggling a category re-filters the shown group instantly.
   - **Muscle picker** chips grouped by region — **Upper body** (chest, back,
     shoulders, biceps, triceps, traps, forearms), **Core** (abs, lower back),
     **Lower body** (glutes, quads, hamstrings, calves). A reliable fallback to the map.
   - Tapping a group renders its **exercise cards** below (filtered by equipment).
   - **Focus sub-filter:** some groups have a second chip row to target a region of
     the muscle — **Glutes** (Overall / Upper & side, i.e. gluteus medius),
     **Shoulders** (Front / Side / Rear), **Back** (Lats / Mid-back), **Abs**
     (Upper / Lower / Obliques). "All" is the default; the focus chips stack with
     the equipment filter.
2. **Routines** — build your own and use pre-made ones:
   - **Your routines:** tap **＋ Routine** on any exercise → a popup menu lets you add
     it to an existing routine or **create a new named routine** on the spot (without
     leaving the page). Saved routines persist in `localStorage`; on this tab each is
     expandable and editable in place — **rename** it, remove an exercise (✕), or
     delete the routine.
   - **Starter routines**: three expandable pre-built programs. Each lists its
     exercises with sets × reps and a quick ▶ video, plus **"Save a copy"** to drop
     an editable copy into your own routines.
   - **Auto warm-up / cool-down:** every routine (custom or built-in) shows a
     **Warm-up** block (dynamic stretches) before its exercises and a **Cool-down**
     block (static stretches) after — picked automatically from the muscle groups the
     routine trains, so it stays in sync as you edit.
3. **Machines** — a guide to common gym machines. Each card shows what the machine
   trains, how to set it up (beginner steps), a demo video (its YouTube thumbnail is
   the still image), and a "See _[muscle]_ exercises" link. Search finds machines too.
4. **Saved** — favorited exercises **and stretches** (full cards), persisted in
   `localStorage`, with an **All / Exercises / Stretches** sub-toggle.
5. **Tips** — short gym-confidence tips.

### Exercise card contents
Each exercise shows: name, a **Beginner / Intermediate** badge, a one-line
description, a **video thumbnail** (the YouTube poster image — tap it to expand the
inline player; tap again to close), a **sets · reps · rest** row, **form cues**,
**common mistakes**, a **gear** pill, a **secondary muscles** pill, a **♡ Save**
button, and a **＋ Routine** button (opens the add-to-routine popup). (The thumbnail
is the play control — cards lead with the image rather than a text button.)

### Covered muscle groups (13) / exercises (66)
chest, back, shoulders, biceps, triceps, traps, forearms (upper); abs, lower back
(core); glutes, quads, hamstrings, calves (lower). Weighted toward machines /
dumbbells / cables / bodyweight that are easy to learn. (Lower-body selection
leans a little more advanced — the user is a strong-legged ex-athlete.) The glutes
group includes a full set of gluteus-medius (upper/side glute) exercises behind
the **Upper & side** focus.

---

## Key design decisions (please preserve unless asked otherwise)

1. **`index.html` + a small `assets/` image folder**, no build tooling — chosen for
   trivial sharing and GitHub Pages compatibility. Don't introduce a framework/build
   step unless the user wants ongoing feature growth.
2. **Body map = supplied anatomical images + invisible SVG hotspots** (not a
   hand-drawn figure). The original hand-drawn SVG was removed in v3; Phase 4 brought
   the body picker back as licensed line-art images (`assets/<female|male>-<front|back>.png`)
   with transparent clickable `<polygon>`s on a 500×900 overlay. The region **chips
   stay** as the mobile fallback. Don't freehand an anatomical SVG.
3. **YouTube "Watch" buttons embed a curated, hard-coded video per exercise.**
   Each exercise in `DATA` has a `yt` field holding an 11-character YouTube video
   ID, rendered as a `youtube-nocookie.com/embed/<id>` iframe.
   - **Why curated and not the live YouTube API:** to keep the single-file,
     zero-setup, zero-API-key design (a "fetch the highest-view video live"
     approach would need a Google API key exposed in the public file).
   - **Dead-link safety:** every player keeps a fallback link that opens a YouTube
     **search**, so if a hard-coded video is ever removed the user can still find a
     replacement.
4. **Saving degrades gracefully.** `localStorage` is wrapped in try/catch; if it's
   unavailable (private mode), the Saved tab shows a banner rather than failing
   silently. Don't assume `localStorage` always works.
5. **Routines reference exercises by key, never by duplicating content.** A routine
   item is a `"group:key"` string looked up in the flat `ALL` map. Fix an exercise
   once and it's fixed everywhere it appears.
6. **Major muscle groups only**, but groups can have an optional **focus
   sub-filter** for the regions a beginner actually cares about (e.g. upper/side
   glutes). Configured in `FOCUS` / `FOCUS_MAP`, not by splitting the group. Don't
   add individual-head groups to the muscle picker — use a focus instead.
7. **Dark theme**, `--accent` is the red/coral brand color. Focus chips use
   `--good` (teal) to distinguish them from the red muscle chips.

---

## Code map (all inside `index.html`)

- **`<style>`** — all CSS. Theme variables are at the top under `:root`.
  Layout is mobile-first with a fixed bottom `.tabbar`. Key classes: `.view`
  (one per tab, `.on` = visible), `.card` (an exercise card), `.srr` (the
  sets/reps/rest row), `.cues` / `.mistakes` (the bullet lists), `.pill`
  (gear / secondary-muscle chips), `.player` (`.player.open` reveals the inline
  iframe), `.routine` / `.rrow` (routines), `.tip`, `.banner`.
- **`<script>`**:
  - **`DATA`** — the single source of truth for content. An object keyed by group
    id; each entry has a `title` and an `ex` array. Each exercise is:
    ```js
    { key, name, level, yt, equip:[...], gear, sets, reps, rest,
      desc, cues:[...], mistakes:[...], secondary:[...] }
    ```
    - `key` is unique **within** its group; routines address exercises as
      `"group:key"`.
    - `equip` drives the equipment filter (subset of `EQUIP`).
    - `yt` is a real, oEmbed-verified YouTube ID. **Edit this object to
      add/remove/change exercises or swap a video.**
  - **`REGIONS`** — controls how muscle groups are grouped/ordered in the picker.
  - **`EQUIP`** — the five canonical equipment tags (used on exercises and in
    filtering). **`EQUIP_GROUPS`** — how they're presented to the user (Free weights
    / Machines & cables / Bodyweight); a category chip toggles its underlying tags.
  - **`FOCUS` / `FOCUS_MAP`** — the optional per-group focus sub-filter. `FOCUS[group]`
    is the ordered `[key, label]` chips to show (e.g. glutes →
    `[["overall","Overall"],["medius","Upper & side"]]`); `FOCUS_MAP[group][exKey]`
    assigns each exercise to one focus. Focus is **not** stored on the `DATA` object —
    it's attached when `ALL` is built. A group absent from `FOCUS` simply shows no
    focus row. To add a focus to a group, add it to both `FOCUS` and `FOCUS_MAP`.
  - **`ROUTINES`** — array of built-in starter programs; each has `days[]`, each day
    a list of `"group:key"` item strings.
  - **`MACHINES`** — machine-guide entries `{ name, trains, how:[...], yt, group? }`.
    `yt` reuses (mostly) an exercise's validated demo of that machine; the card image
    is the video's YouTube thumbnail, not an `/assets` photo. `group` links to that
    muscle's exercises via `goToGroup`. Rendered by `machineCardHTML` /
    `renderMachines`, and also matched by `runSearch`.
  - **`MYROUTINES`** (runtime state, not config) — the user's saved custom routines
    `[{ id, name, items }]`. Persist to `localStorage` (`wf_routines`), gated by
    `canSave`; each routine's `items` are filtered to `ALL` at load.
  - **`STRETCHES`** — flat list of stretches `{ key, name, type, yt, hold, groups, desc,
    cues }`. `type` is `dynamic` (warm-up) / `static` (cool-down); `groups` lists every
    muscle covered. `STRETCH_BY_KEY` is the lookup; saved as `"stretch:<key>"`. Used by
    the Stretches mode, the routine `pairing()`, and search. `stretchesFor(group,type)`
    and `stretchesForGroups(groups,type,cap)` query it.
  - **`TIPS`** — array of gym-confidence tip strings.
  - **`HOTSPOTS`** — body-map click targets, `HOTSPOTS[female|male][front|back]` =
    list of `{ g, p }` (muscle group + SVG polygon `points`) overlaid on the
    `assets/<type>-<view>.png` images on a shared 500×900 canvas. Rendered by
    `renderBodyMap`; per gender+view because proportions differ.
  - **`SUBZONES`** — focus sub-zones for splittable muscles,
    `SUBZONES[group][type][view]` = `{ f, p }` list (focus key + polygon). When a
    splittable muscle is selected, `renderBodyMap` draws these in place of the
    whole-muscle hotspot so the user can tap a section. Both `HOTSPOTS` and `SUBZONES`
    are generated by `scripts/extract_hotspots.py` (it splits the muscle masks).
  - **`ALL`** — a flat map built at load time: `ALL["group:key"]` → the exercise
    object (plus `group` / `groupTitle`). Used by routines, saving, and rendering.
  - **`cardHTML(id)` / `rowHTML(id)`** — render a full exercise card / a compact
    routine row from a `"group:key"` id.
  - **`showGroup(group, focus)`** — renders the filtered exercise list for a muscle
    group, plus the focus chip row if the group has one. `focus` is optional
    (defaults to the current/"all" focus); the focus chips call it with an explicit
    focus key.
  - **`runSearch(raw)`** — global text lookup over `ALL` (name/muscle/gear/
    equipment); renders matching cards. Ignores the equipment filter. Empty input
    returns to the "pick a group" placeholder.
  - **`toggleEquipGroup(key)`** — toggle an equipment category, then re-render.
  - **`renderBodyMap()` / `setBodyType()` / `setBodyView()` / `syncBodyMap(group)`** —
    the body map. `renderBodyMap` swaps the `assets/` image + draws the hotspot
    polygons (or, for a selected splittable muscle, its `SUBZONES`); `syncBodyMap`
    highlights the selection and auto-switches front/back so chip/search selections
    stay visible. `showGroup` calls `syncBodyMap`.
  - **`openRoutineMenu(btn)`** — opens the add-to-routine popup for a card's exercise.
    Inside it: **`toggleExInRoutine(rid)`** adds/removes the exercise from a saved
    routine, **`createRoutineFromMenu()`** makes a new named routine containing it,
    **`closeRoutineMenu()`** dismisses it.
  - **`removeExFromRoutine(rid, exId)` / `deleteRoutine(uid)`** — in-place edits on the
    Routines tab. **`startRename` / `commitRename` / `cancelRename`** drive the inline
    rename. **`saveCopyOfBuiltin(i)`** flattens a built-in into an editable copy.
  - **`renderRoutinesTab()`** — renders the Routines tab: `renderMyRoutines()` (saved
    customs, editable) + `renderRoutines()` (built-ins).
  - **`toggleRoutine(uid)`** — expand/collapse a routine; `uid` is `"b"+i` for
    built-ins or the custom routine's `id`.
  - **`toggleVid(btn)`** — expands/collapses the inline player (one open at a
    time); works for both full cards and routine rows.
  - **`toggleSave(btn)` / `renderSaved()` / `persistSaved()`** — favorites +
    `localStorage` (with the private-mode fallback).
  - **`renderTips()`** — renders the Tips tab.
  - **`showTab(t)`** — switches the active tab.

### To add a new exercise
Find the group in `DATA` and add an object to its `ex` array. Give it a `key`
unique within that group and fill in every field:
```js
{ key:"inclinepushup", name:"Incline Push-up", level:"Beginner", yt:"VIDEO_ID",
  equip:["Bodyweight"], gear:"A bench", sets:3, reps:"10–12", rest:"60s",
  desc:"Hands on a bench, easier than the floor.",
  cues:["...","...","..."], mistakes:["...","..."], secondary:["Chest","Triceps"] }
```
Rendering, the embed, the equipment filter, and saving are all automatic.

### To find / swap a video ID
1. Find a good "proper form" tutorial on YouTube; the ID is the `v=` part of
   `https://www.youtube.com/watch?v=VIDEO_ID`.
2. **Verify it's embeddable** before trusting it — a `200` with JSON means it
   exists and is embeddable:
   ```bash
   curl -s "https://www.youtube.com/oembed?format=json&url=https://www.youtube.com/watch?v=VIDEO_ID"
   ```
3. Paste the ID into the exercise's `yt` field.

> ⚠️ Never invent video IDs. Every `yt` value must be a real ID you verified with
> the oEmbed check above — a wrong ID silently embeds the wrong video or a dead
> player. (All current IDs were validated this way.)

### To add a new muscle group
1. Add an entry to `DATA` with a new group id (with all fields on each exercise).
2. Add the group id to the right region in `REGIONS`.

### To add a routine
Add an object to `ROUTINES` with `name`, `meta`, `note`, and `days` (each day a
list of `"group:key"` item strings that must exist in `DATA`).

---

## Roadmap
All four planned phases are built. Smaller ideas still open: workout-of-the-day /
randomized session; mark-as-done session logging.

### Done
- **Stretching:** a Stretches mode on the Exercises tab (warm-up/cool-down stretches
  per muscle, all 13 groups), stretches savable (Saved has All/Exercises/Stretches),
  and every routine auto-shows warm-up + cool-down blocks paired from the muscles it
  trains. (`STRETCHES` library, ~19 stretches, oEmbed-verified videos.)
- **Phase 4 — full-body clickable muscle map:** female/male + front/back anatomical
  line-art (user-supplied, in `assets/`, normalized to 500×900 with transparent
  backgrounds) with an invisible SVG hotspot overlay per muscle, as the visual entry
  point at the top of the Exercises tab. Chips remain as the mobile fallback. This is
  why the app now allows a small `assets/` folder.
- **Phase 3:** machine guide (12 common machines) with how-to steps, demo videos,
  and a link to each machine's exercises; search now finds machines too. (Machine
  images are the demo videos' YouTube thumbnails.)
- **Phase 2:** custom routines — add exercises to a routine from a popup menu on each
  card (or create a new named routine there), edit/delete them in place, and "Save a
  copy" of a built-in. (Originally a draft-builder; later reworked into the popup menu.)
- **Phase 1:** global search, free-weights equipment grouping + presets, video
  thumbnails on cards (visual-first).
- **v3:** equipment filter, sets/reps/cues/mistakes, focus sub-filters,
  save/favorites, routines, tips.
- **v2:** embedded curated demo videos.
