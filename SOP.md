# SOP — Producing On-Brand Video with HyperFrames + Descript

This is the standing operating procedure for the media-production-kit. Open this file at
the start of any new video session. It covers: the per-session workflow, how to keep each
brand on-brand, the evergreen slide library (short + long form), and what Descript can and
can't do so you know where the tool's edges are.

The model: **Descript is the editor. HyperFrames makes the graphics.** You record and cut
in Descript; HyperFrames renders the branded inserts (intro, lower-thirds, callouts,
takeovers, outro) that you drag onto the Descript timeline. Captions are done natively in
Descript.

**This SOP is the reference manual; the `youtube-animations` skill is the automation spine.**
For a YouTube (16:9) video, don't run this SOP by hand — trigger the **`youtube-animations`** skill
(in the Internyl workspace). It orchestrates the whole arc end-to-end: script overview → chapter
markers → graphic-moment list → build → render review → placement on the Descript timeline with
sound effects. It reads *this file* for the mechanics (render commands, folder org, the libraries,
the Descript MCP placement flow), so the two aren't redundant — the skill is the *sequence*, this
SOP is the *how*. Use this SOP directly for short-form (9:16) or one-off graphics work that the
skill doesn't cover. See §8 for the exact division of labor.

---

## 0. The desk setup (every session)

Three windows open:

1. **Claude Code** — pointed at `media-production-kit/`. This is where graphics get built.
2. **Finder** — at `media-production-kit/renders/`. Cmd+Shift+G → paste the path. This is
   where finished `.mp4` (and `.mov`, once upgraded) inserts land for you to drag out.
3. **Descript** — your recording + edit project for this video.

---

## 1. Start-of-session checklist

1. **Pick the brand.** Internyl? Means & Meaning? A client? → see §2 (Brand switching).
2. **Pick the format.** Short (9:16) or long-form/YouTube (16:9)? → see §4.
3. **Confirm the visual identity is loaded.** The active `DESIGN.md` must match the brand
   you're producing for. HyperFrames will NOT write a composition without one.
4. **Have the content.** Two options — use whichever fits how you worked:
   - **Scripted (optional markup):** paste the script with inline cues in `{curly braces}`
     where you want graphics — e.g. `{intro slide}`, `{callout: "the key term"}`, `{outro}`.
   - **Raw transcript (no markup needed):** paste the full transcript straight from your
     recording, retakes and all. Claude Code will identify graphic moments from context,
     flag retake boundaries, and output a cut guide + composition list. No pre-tagging required.

---

## 2. Staying on-brand (the part that keeps everything consistent)

Two files govern everything that ends up on screen, and they're the single source of truth for
their halves:

- **`DESIGN.md`** — the *look*: colors, fonts, motion rules, logo usage, and a "What NOT to Do"
  list. HyperFrames reads it before writing any composition.
- **`VOICE.md`** — the *words*: how any on-screen text reads (chapter titles, callouts, list and
  quote-card lines, lower-third labels). Check copy against it before rendering.

To restyle or re-word, edit these files and re-render. **Don't hand-edit the individual composition
files** — they read the look from `DESIGN.md` and their copy should follow `VOICE.md`, so a change
made in one composition doesn't propagate and drifts out of sync. One file changed, everything
re-renders on-brand.

---

## 3. The per-video workflow (step by step)

1. **Record** the talking-head (Descript or Riverside → Descript).
2. **Rough-cut** in Descript: remove filler, tighten pacing, get the spoken timeline right.
3. **Hand off the content to Claude Code.** Before any cutting, Claude reads the **raw
   transcript end-to-end and writes a coherence pass — `videos/<slug>/output/story.md`** — a
   clean, blog-style retelling of the talk in narrative order. This does two jobs:
   - **(a) Edit safety / continuity.** It's the narrative map Claude and you both hold while
     cutting, so collapsing retakes or stripping filler never instructs Underlord (or you) to
     drop a beat the story actually needs. If a proposed cut would break the `story.md` thread,
     that's the signal to keep it. (This is the guard against the "something I needed got
     removed" misses — see the note Graham flagged 2026-06-10.)
   - **(b) Reusable content.** The same `story.md` is a draft blog post / newsletter / LinkedIn
     piece. Run it against **`VOICE.md`** (root of this kit) and touch it up before publishing.

   Only after `story.md` exists does Claude extract graphic cues, strip verbal direction from the
   spoken text, and produce the cut guide showing retake segments to delete in Descript.
4. **Claude Code builds the inserts** — reuses existing slides from the library (§4),
   creates only what's new, and renders them to `renders/` as `.mp4` (opaque takeovers) or
   `.mov` (transparent overlays — lower-thirds, logo pops — once you've upgraded Descript).
   **Before finalizing, draft every piece of on-screen text (chapter titles, callout copy,
   list/quote-card lines, chapter markers) and check it against `VOICE.md`.** This has been a
   real miss — chapter slides and generated copy have read as LLM-flavored (title case
   headers, inflated framing like "X vs. Y", generic AI vocabulary). `VOICE.md`'s banned-list
   and formatting rules (section case, no em dashes, no negative-parallelism constructions)
   apply here just as much as to prose.
5. **Lint passes** (the agent runs `npx hyperframes lint`); it reports the Finder path and
   which file maps to which script line.
6. **Drag inserts onto the Descript timeline** at each cue. Full-screen takeovers go on top
   as their own clips; transparent overlays (`.mov`) layer over the face-cam.
7. **Captions** — generate natively in Descript.
8. **Export** the finished video from Descript → post to LinkedIn / YouTube.

**Insert types and file format:**

| Insert | Covers face-cam? | Format | Needs Descript upgrade? |
|--------|------------------|--------|--------------------------|
| Intro / outro / full-screen takeover | Yes (opaque) | `.mp4` | No |
| Lower-third, callout overlay, logo pop | No (transparent) | `.mov` (ProRes 4444 / alpha) | Yes |

> Alternative to upgrading: HyperFrames can **flatten** the whole edit — your recording as
> the base video layer with graphics composited on top, rendered to one finished `.mp4`. No
> transparency needed. You've chosen the Descript path, so the upgrade is the cleaner route;
> keep flatten in your back pocket for complex composited pieces.

---

## 4. The evergreen library

Two kinds of reusable material, so you rebuild as little as possible:

- **Source compositions** (`compositions/*.html`) — the editable slides below. Same structure,
  re-skins from `DESIGN.md`. Each is a self-contained insert with its own entrance + exit, so it
  drops between talking-head segments cleanly (no slide-to-slide transitions needed).
- **Finished effects** (`renders/_library/`) — already-rendered `.mov`/`.mp4` inserts good enough to
  drag onto a new timeline as-is, no rebuild, no render cost. See §4.5.

### Short-form (9:16) — already built

| Slide | File | Use |
|-------|------|-----|
| Intro | `experiment-intro-9x16.html` | Opening hook / title card |
| Topic takeover | `claude-code-takeover-9x16.html`, `capabilities-takeover-9x16.html` | Full-screen point with icon/logo |
| Callout | `internyl-callout-9x16.html` | Emphasize a key term mid-sentence |
| Lower-third | `internyl-lower-third-9x16.html` | Name + role (transparent overlay) |
| Follow / subscribe | `internyl-follow-9x16.html` | CTA card |
| Outro | `experiment-outro-9x16.html` | Closing CTA + URL |

### Long-form / YouTube (16:9) — evergreen elements to build when you start a YouTube video

These don't exist yet; they're the standing set worth creating once and reusing every
episode:

- **Intro / title card** — episode title + series branding (the most reused element).
- **Lower-third** — your name/title; guest name/title for interviews.
- **Chapter / section divider** — "Part 1 — …" full-screen card between segments.
- **Callout / key-term card** — same idea as the 9:16 callout, 16:9 layout.
- **List / steps overlay** — for "here are 3 things…" segments.
- **Quote / pull-stat card** — emphasize a number or a line.
- **Lower-third URL / CTA bug** — small persistent corner branding.
- **End screen / outro** — subscribe + next-video CTA, sized for YouTube's end-card zone.

When you start your first YouTube video, tell Claude Code "build the 16:9 evergreen set" and it'll
create these from `DESIGN.md`. After that they're drop-in for every episode.

### 4.5 The finished-effects library (`renders/_library/`)

When an animation turns out well, it goes in `renders/_library/` as a finished render so the next
video reuses it with zero rebuild. This is the fastest path — no composition edit, no render time.
**Check `renders/_library/README.md` before building any new insert;** if something there fits (or
nearly fits), reuse it. Each entry maps back to its source composition, so you can re-render with a
tweak and copy the new output back.

Seeded with two proven effects:

| Effect | What it is | Source composition |
|---|---|---|
| `voice-dictation-wave.mov` | Voice-dictation / waveform ("talking to it" beat) | `portal-voice-wave.html` |
| `lower-third-name.mov` | Name lower-third (transparent overlay) | `lower-third.html` |

To add one: render as usual, then `cp` it into `renders/_library/` with a stable descriptive name
(not the numbered per-video name) and add a row to that README. See the README for the full drill.

---

## 5. What's possible with Descript (and its edges)

**Strong fits:**
- Record talking-head directly; transcript-based editing (edit video by editing text).
- Remove filler words, "studio sound," and gaps automatically.
- Native captions/subtitles with styling — your caption layer lives here, not HyperFrames.
- Overlay/layer imported clips (your HyperFrames inserts) on the timeline.
- Multitrack, screen recording, basic motion/zoom.

**Known limits / edges:**
- **Transparent video import requires a higher tier** — this is the `.mov` (alpha) upload
  you're upgrading for. Free/lower tiers reject ProRes/alpha `.mov`; `.mp4` always uploads.
  Once upgraded, transparent overlays (lower-thirds, logo pops) work directly.
- Descript compresses on export; for max fidelity record externally (Riverside) and import.
- Heavy keyframe/compositing work is not Descript's strength — that's why complex branded
  motion is built in HyperFrames and dropped in as a clip.
- **Riverside vs Descript for recording:** Riverside records higher-quality local tracks
  (better for remote guests, separate audio/video, less compression). Descript's recorder
  is fine for solo talking-head. Either way the edit happens in Descript.

After the upgrade there's no blocker in this workflow — `.mp4` for takeovers, `.mov` for
overlays, captions native, inserts from HyperFrames.

---

## 6. Quick reference — commands

```bash
npx hyperframes preview          # studio editor in browser
npx hyperframes lint             # validate (run after every change)
./render-block.sh <block> [dur] [quality] [format] [project-slug]   # render one insert
                                                      # format: mp4 | mov | webm | png-sequence
                                                      # project-slug → renders/<slug>/ (always pass it)

# Find the next numbered project slug before creating a new folder:
ls renders/ | grep -v '^_' | sort | tail -1          # shows the highest existing NN- prefix
```

Renders land in `media-production-kit/renders/<project-slug>/` (see §7).

---

## 7. Per-video organization (folders, the Descript MCP, restyling)

This kit serves every video. Keep each one isolated so nothing piles up in one place.

### Folder structure

```
media-production-kit/
├── videos/<slug>/              # one folder per video
│   ├── START-HERE.md           # that video's brief + current state (read after this SOP)
│   ├── inputs/                 # script, transcript exports for that video
│   └── output/                 # story.md (raw-transcript coherence pass, written FIRST),
│                               #   placement-map.md (which insert goes on which line/timecode)
├── references/<slug>/          # style inspiration to drop in for a restyle (see its README)
│   └── _inbox/                 # unsorted inspiration
└── renders/
    ├── 01-export-your-automation/   # NN- prefix = chronological production order
    ├── 02-noloco-agency-portal/
    ├── 03-human-in-the-loop/
    ├── 04-managing-complexity/      # current newest project
    ├── _library/                    # reusable finished effects (drag-in, no rebuild) — see §4.5
    ├── _archive/                    # _ prefix = utility/meta folders, not numbered
    ├── _templates/
    └── _toolcheck/
```

**Numbered project slugs:** Production video folders in `renders/` always use a two-digit
prefix (`01-`, `02-`, …) so the folder sorts chronologically. Utility/meta folders that start
with `_` (e.g., `_templates`, `_archive`, `_toolcheck`) are excluded from numbering.

**Before creating any new render folder**, run:
```bash
ls renders/ | grep -v '^_' | sort | tail -1
```
This prints the highest existing prefix (e.g., `04-managing-complexity`). Increment by one and
use that as the project slug — e.g., `05-new-project-name`. Always pass this numbered slug as
the fifth argument to `render-block.sh` so output lands in the right folder automatically.

**Number the render filenames** so the `renders/<slug>/` folder sorts top-to-bottom in
timeline (drop) order — `01-`, `02-`, `03-`, … prefixes. The numeric order must match the
order of the asset table you hand back in chat and the rows of `output/placement-map.md`, so
Graham can drag them into Descript in sequence straight down the folder. Example:
`01-human-loop-intro.mp4`, `02-graham-lower-third.mov`, `03-human-loop-chapter.mp4`.
(`render-block.sh` names output from the block file; rename the render to its numbered name
after rendering, or pass a numbered output path.)

**Composition file naming.** The `renders/<slug>/` outputs are per-video-isolated and numbered,
so the composition **source** files in the shared `compositions/` folder don't need a video-slug
prefix — use bare descriptive names (`chapter-airtable.html`, `recap.html`, `scoreboard.html`,
`criteria-speed.html`). Keep names specific enough to avoid clobbering another video's block in
that shared folder (e.g. `result-airtable` not `result`); only add a prefix if a bare name would
actually collide with an existing composition. (Legacy sets like `noloco-*` and `human-loop-*`
predate this and can stay as-is.)

**ALWAYS include a verbatim transcript-cue column.** Graham navigates Descript by the actual
words, not timecodes — there are no visible timestamps to scrub to. So every asset table (both
in chat and in `output/placement-map.md`) MUST have a column quoting the **exact transcript
phrase** where the insert drops, copied word-for-word from the Descript transcript (e.g.
`"So what is a human in the loop?"`). Timecodes are a nice-to-have secondary reference; the
verbatim quote is the primary one. Pick a short, unambiguous phrase Graham can Cmd+F for.

`renders/` output is disposable — the `compositions/*.html` are the real source. Re-render
anytime. **Always pass the project slug to `render-block.sh`** so output lands in
`renders/<slug>/`, not loose in the root.

### Identifying graphic moments — three ways (use any/all)

1. **The script** — Graham's filming script with `{curly-brace}` cues marking where he wants graphics.
2. **The transcript** — infer beats from context (a definition, "as you can see," a 2×2, a list, the recap). Transcripts (raw + cleaned-with-timecodes) live in `videos/<slug>/inputs/`.
3. **Rewatch / request** — Graham names specific moments ("framework goes here", "lower-third when I say my name"). His call overrides inference.

### The Descript MCP (the editor side, now wired into Claude Code)

Descript is connected as an MCP — confirm with `list_projects` at session start. Useful tools:
- `list_projects` / `get_project` — find the project + its compositions (durations, media).
- `export_transcript` — pull the live transcript **with timecodes** (`{ "timecodes": { "on_paragraphs": true } }`) to time inserts precisely. This replaces hand-copying transcripts.
- `prompt_project_agent` → `wait_for_job` — let Underlord edit the project (collapse retakes, cut filler, layout switches). Always work on a **duplicated composition** as the rollback, and **verify the result with `get_project`** — the agent can report "success" on a no-op.

#### Placing inserts / SFX from Claude Code (proven 2026-06-17)

Claude can do the drag-into-Descript step itself, anchored to the spoken words — not just hand you a placement map. Two flows, both tested working on this project:

1. **Stock SFX at a transcript moment.** If the sound is already in the project's media
   (`get_project` → `Stock Media/...`), just prompt the agent: *"place the existing stock clip
   'Flash Whoosh Swoosh 3' right as I say 'My name is Graham', on its own audio track, don't
   touch the spoken track."* Landed at 0:40.29, full clip, ~9 credits.
2. **A rendered insert (video/audio) you have locally.** First `import_media` with
   `content_type` + `file_size` (direct upload) → `PUT` the file bytes to the returned
   `upload_url` with `Content-Type: application/octet-stream` → `wait_for_job`. Then prompt the
   agent to place that named clip as a **video overlay above the face-cam** at the target line.
   Tested: `debug-video-bumper-16x9.mp4` placed at 8:21.6, ~6 credits.

**How well it works:** it reliably finds the *words* and uses the *existing* clip without
disturbing the spoken track, and reports an exact timecode. The anchor timecode may differ from
your transcript notes (the edited comp has its own timing) — that's fine, it anchors to words,
not clocks. **Caveats:** (a) you cannot frame-verify from Claude — `get_project` shows media +
compositions, not track placements, so the real check is scrubbing to the reported timecode in
Descript; (b) frame-accurate sync of a SFX to a *visual hit* is the weak spot — expect small
offsets to nudge by hand; (c) prefer a **duplicated composition** for anything bigger than a
single isolated insert. Always tell the agent explicitly: *use only the existing named clip,
create nothing, don't cut/move the spoken track.*

### Restyling (revamping the look)

Drop inspiration in `references/<slug>/`, then change the look in `DESIGN.md`. Never hand-edit
composition files to restyle — they read from `DESIGN.md`. Re-render after.

### The 16:9 YouTube evergreen set — STATUS: started

The set listed in §4 now exists in first-draft form (built for "Human in the Loop"):
`human-loop-intro.html` (title card), `human-loop-chapter.html` (section divider),
`graham-lower-third.html` (name overlay), `human-loop-2x2.html` (decision-matrix overlay).
Generalize these into reusable evergreen blocks once the look is locked.

---

## 8. Division of labor — this SOP vs. the `youtube-animations` skill

They are not redundant and they should not be merged. One is the *sequence*, the other is the
*reference* it calls.

| | `youtube-animations` skill (Internyl workspace) | This SOP (in the kit) |
|---|---|---|
| **Role** | Orchestrator / automation spine | Reference manual |
| **Scope** | YouTube / 16:9 only | Every format (short + long), plus one-off graphics work |
| **What Graham triggers** | Yes — this is the front door | No — read by the skill (or by hand for short-form) |
| **Owns** | The end-to-end flow + its gates; SFX house rules; the intro recipe | Kit mechanics: render commands, folder org, the two libraries, the Descript MCP flows, Descript's edges |
| **Reads the other?** | Reads this SOP for mechanics | Points to the skill as the YouTube front door |

**The automated arc (what "trigger the skill" gets you), gated only where Graham's judgment is
required:**

1. **Script overview** — reads the filming script, summarizes it, flags a jumbled structure. *(auto)*
2. **Chapter markers** — sets them in Descript. *(auto, no approval — reversible metadata)*
3. **Graphic-moment list** — proposes every insert as a table. **← human gate 1: approve/cut/reorder.**
4. **Build + render** — reuses `renders/_library/` and evergreen compositions first, builds only
   what's new, checks all copy against `VOICE.md`, renders numbered inserts. *(auto)*
5. **Render review** — hands back the placement map. **← human gate 2: validate the renders.**
6. **Placement + SFX** — on approval, places every insert on a duplicated Descript composition at
   its verbatim cue and lays in the sound effects (the SFX pass Graham singled out as high-value).
   *(auto; hands back timecodes to scrub-check, since placement can't be frame-verified from here.)*

Two human gates, everything else runs. If you're changing *how a video gets made*, edit the skill.
If you're changing *how the tool works* (a render flag, a folder rule, a new library), edit this SOP.
