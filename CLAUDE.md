# HyperFrames Project

## Producing a video? READ THIS FIRST

If this session is to make graphics for a video, **read `SOP.md`** (the standing production
workflow: Descript = editor, HyperFrames = graphics; brand rules; per-video folders; the
Descript MCP) before anything else. Then, if you're working a specific video, read its
`videos/<slug>/START-HERE.md` for that video's current state and task.

**Any on-screen text you write — chapter titles, callout copy, list/quote-card lines, lower-third
labels — must follow `VOICE.md`** (root of this kit). Draft it, then check it against VOICE.md's
banned list and formatting rules before rendering. Feedback has flagged chapter slides reading as
LLM-generated (title case, inflated framing); this is the fix.

## Skills — USE THESE FIRST

**Always invoke the relevant skill before writing or modifying compositions.** Skills encode framework-specific patterns (e.g., `window.__timelines` registration, `data-*` attribute semantics, shader-compatible CSS rules) that are NOT in generic web docs. Skipping them produces broken compositions.

### HyperFrames

| Skill                      | Command                   | When to use                                                                                       |
| -------------------------- | ------------------------- | ------------------------------------------------------------------------------------------------- |
| **hyperframes**            | `/hyperframes`            | Creating or editing HTML compositions, captions, TTS, audio-reactive animation, marker highlights |
| **hyperframes-cli**        | `/hyperframes-cli`        | CLI commands: init, lint, preview, render, transcribe, tts                                        |
| **hyperframes-registry**   | `/hyperframes-registry`   | Installing blocks and components via `hyperframes add`                                            |
| **website-to-hyperframes** | `/website-to-hyperframes` | Capturing a URL and turning it into a video — full website-to-video pipeline                      |
| **gsap**                   | `/gsap`                   | GSAP animations for HyperFrames — tweens, timelines, easing, performance                          |

### Claude Design Bridge (custom skill — this repo)

| Skill                              | Command                              | When to use                                                              |
| ---------------------------------- | ------------------------------------ | ------------------------------------------------------------------------ |
| **claude-design-to-hyperframes**   | `/claude-design-to-hyperframes`      | Converting a Claude Design export (scenes.jsx + assets) into HyperFrames |

### Creator Plugins (Audio, Images, Copy)

| Skill                    | Command                    | When to use                                              |
| ------------------------ | -------------------------- | -------------------------------------------------------- |
| **create-song**          | `/create-song`             | Generate background music for a video                    |
| **create-sfx**           | `/create-sfx`              | Generate sound effects                                   |
| **create-soundscape**    | `/create-soundscape`       | Generate ambient soundscapes                             |
| **create-thumbnail**     | `/create-thumbnail`        | Generate a thumbnail image for the video                 |
| **transform-image**      | `/transform-image`         | Transform or style an existing image                     |
| **fetch-brand-assets**   | `/fetch-brand-assets`      | Pull brand colors, fonts, and logos from a URL           |
| **create-title**         | `/create-title`            | Generate video titles                                    |
| **create-description**   | `/create-description`      | Generate video descriptions                              |
| **create-hashtags**      | `/create-hashtags`         | Generate hashtags                                        |

> **Skills not installed?**
> ```bash
> npx skills add heygen-com/hyperframes --yes
> npx skills add SecondWindAI/creator-plugins --yes
> ```
> Then restart your agent session.

## Commands

\`\`\`bash
npx hyperframes preview          # preview in browser (studio editor)
npx hyperframes render           # render to MP4
npx hyperframes lint             # validate compositions (errors + warnings)
npx hyperframes lint --verbose   # include info-level findings
npx hyperframes lint --json      # machine-readable output for CI
npx hyperframes docs <topic>     # reference docs in terminal
\`\`\`

## Documentation

**Quick reference** (no network required):
\`\`\`bash
npx hyperframes docs <topic>
\`\`\`
Topics: \`data-attributes\`, \`gsap\`, \`compositions\`, \`rendering\`, \`examples\`, \`troubleshooting\`

**Full documentation** — use the machine-readable index, do NOT guess URLs:
\`\`\`
https://hyperframes.heygen.com/llms.txt
\`\`\`

## Project Structure

\`\`\`
.
├── index.html          # root composition (entry point)
├── compositions/       # sub-compositions (data-composition-src)
│   └── components/     # reusable components
├── assets/             # media files (video, audio, images)
├── meta.json           # project metadata (id, name)
├── hyperframes.json    # project config + registry
└── transcript.json     # word-level transcript (generated, gitignored)
\`\`\`

## Linting — ALWAYS RUN AFTER CHANGES

After creating or editing any \`.html\` composition, **always** run the linter before considering the task complete:

\`\`\`bash
npx hyperframes lint
\`\`\`

Fix all **errors** before presenting the result. Warnings are informational and usually safe to ignore.

## Key Rules

1. Every timed element needs \`data-start\`, \`data-duration\`, and \`data-track-index\`
2. Visible timed elements **MUST** have \`class="clip"\` — the framework uses this for visibility control
3. GSAP timelines must be paused and registered on \`window.__timelines\`:
   \`\`\`js
   window.__timelines = window.__timelines || {};
   window.__timelines["composition-id"] = gsap.timeline({ paused: true });
   \`\`\`
4. Videos use \`muted\` with a separate \`<audio>\` element for the audio track
5. Sub-compositions use \`data-composition-src="compositions/file.html"\` to reference other HTML files
6. Only deterministic logic — no \`Date.now()\`, no \`Math.random()\`, no network fetches
7. Default canvas is 1920×1080; set \`data-width\` and \`data-height\` on the root div to change it

## Workflow Best Practices

1. **Start with a skill** — \`/hyperframes\` for any composition work
2. **Lint early and often** — catch issues before they compound
3. **Preview before render** — \`npx hyperframes preview\` opens the studio editor
4. **Add blocks from the registry** — \`npx hyperframes add <block>\` rather than writing from scratch
5. **Keep compositions modular** — break complex videos into sub-compositions under \`compositions/\`
6. **Commit assets separately** — rendered \`.mp4\` files are gitignored; commit source HTML only
