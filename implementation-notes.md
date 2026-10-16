# Music Visualizer: implementation notes

Deliverable: `visualizer.html`, a single file with no dependencies that works offline. The target browser is Chrome on macOS, the only browser that can capture system audio. In other browsers, only the input device, demo and file sources work.
`prototype-gallery.html` is a throwaway sketch, to be deleted once the build is accepted.

## Decision log

| # | Decision | Choice | Why |
|---|---|---|---|
| D1 | Audio source | Picker: system audio (whole-screen share), tab audio, input device (mic or BlackHole), demo signal, audio file | If one capture path fails, another still works |
| D2 | Signal handling | Browser voice processing off (echo cancellation, noise suppression, auto-gain); each band scaled against its own recent peak; beat = sudden jump in kick-range energy against an adaptive threshold with a 250 ms minimum gap | Browsers strip music out with that processing on; volume varies wildly by source |
| D3 | Styles | All six prototypes kept | User choice |
| D4 | Playback | Rotate through the looks the user ticks | User choice |
| D5 | Customization | Settings for every part of a look | User choice |
| D6 | Mix and match | One look on screen at a time, built from swappable parts. No layering. | User picked options 2 and 3 |
| D7 | Slots | Form, Render, Ripple, Palette, Wiring, Symmetry, Camera, Feedback echo, Background | User picked all the extra slots |
| D8 | Rotation trigger | Every N beats (timer fallback when no beats are detected), plus switch early on a drop; minimum gap between switches | User picked options 1 and 3 |
| D9 | Persistence | Saved in the browser on this machine, plus JSON export/import | Default |
| D10 | Safety | No full-screen strobe; beat effects move rather than flash; one overall intensity control | Photosensitivity |
| D12 | Tunnel motion | Rings fly toward the viewer with perspective: spaced evenly in depth, sized by 1/z, fading in at the vanishing point. Speed is set by the Form setting "Fly-through (rings)" (negative flies backwards), with an optional boost from the bass. | User: "as if we are entering the tunnel" |
| D13 | Centre wander | Camera settings: "Centre wander" (amount), "Wander speed", and "Centre kick" (a beat knocks the centre off course, then it springs back). In the tunnel, far rings follow the centre more than near ones, so the tunnel bends. The beat shockwave starts from the moved centre. | User: "let the center stray" |
| D15 | Camera feel | All motion eases toward its target instead of jumping: wander, kicks, zoom pulse, fly speed and spin. Kicks shift where the centre is heading and then fade over ~2.5 s, passing through two lags so movement never starts abruptly. Everything is about 2–3× slower by default. A "Smoothness" camera setting controls how lazily the centre follows (0.3–2 s). | User: the camera was too fast and jerky |
| D18 | Saving model | Built-ins are read-only templates that live in the code and are never stored. Edits create a draft (●) with Save, Save as new… and Revert. Snapshot (S) saves what's on screen as a new look in the active set. Saving a template replaces it in the set with your own version. Unsaved edits trigger a warning when leaving the page. | User chose the full option |
| D19 | Sets | Named, ordered playlists of looks (↑↓✕ to reorder or remove, keys 1–9 to jump). Each set stores its own rotation settings, theme and background-moment settings. The look ticks now mean "in the active set". | So each playlist has a consistent flow |
| D20 | Library file | In Chrome, a linked `looks.json` that every save writes to. It merges with browser storage (newer wins per look/set). After a restart Chrome asks for one click (Reconnect). A file that isn't a library is never written to. Browser storage stays as a backup; Export/Import use the same format. | Browser storage alone is fragile |
| D21 | Themes | 10 themes set per Set (C cycles them) that override palette, glow, grain, trails, colour shift and so on, on top of any look: Neon trails, Noir grain, Ultra trippy, Colourful, Warm, Cold, Vaporwave, Red alert, Terminal green, Ethereal. | User: keep the flow of visuals consistent |
| D22 | Background moments | On drops, energy surges, every 32 beats, or the T key, a texture covers everything behind the shapes for 4–32 beats with fades: VHS, Halftone print, Pixelate, Thermal colour, Light leak, Mirror. "Theme's texture" picks the one that suits the theme. | User: textures on the video background at high points, lasting longer than transitions |
| D16 | Transitions | Transitions run in the GPU pass, which reads both looks: Zoom punch, Iris, Glitch, Burn, Swirl, Crossfade, Cut, plus Random mix (a different style per switch). Transition style and length are separate settings. Glow and shockwaves still apply during transitions. Without WebGL2, every transition falls back to a crossfade. | User: transitions should be "cooler and stylish" |
| D17 | Ridgelines centring | The line block is symmetric about the screen centre (±50% of half-height) | User: the bottom line sat too close to the bottom edge |
| D14 | Ridgelines width | Lines run edge to edge (slightly past the edges, so a wandering centre doesn't reveal the ends); peaks keep their old width | User request |
| D23 | Settings panel UX | Bigger type, a value readout on every slider with the track filled up to the value, toggle switches, chevron sections. A wired effect set to "None" dims its slider. Every setting, button and section has a tooltip (`data-tip`; editor tips are the 4th field of each `SCHEMA` entry). Tooltips appear to the left of the panel, or below the control on narrow screens, and hide while you drag. The editor sits under an "Edit this look" heading, with only Form and Render open by default. | User: settings were hard to read and hard to understand at a glance |
| D24 | Mandala | New form "Arch vortex", after a hand-drawn reference the user liked: rings of rainbow arches (5 nested half-circle bands each) shrinking towards a 3-arm spiral, seigaiha-style. Each ring sits in the valleys of the one outside it and turns faster the deeper it is, so the vortex winds up with the mids. Lines thin out towards the centre. "Arches per ring" uses the Sides setting; Detail sets the number of rings. The Mandala built-in now uses it. Petals stays for Supernova. | User: "I would like the mandala also to get even better design" |
| D25 | Words | New form "Words" with two built-ins, **Words** and **Kanji**. One big word changes every 1–8 beats (Form › New word every). It walks one feeling through up to 4 of the enabled languages (music → música → 音楽 → موسيقى), then moves to the next feeling, with a small caption giving the language and meaning. Smaller words fade in on beats, in bands above and below the big word, or as vertical CJK columns down the sides. They drift towards you (Fly-through), breathe with the mids, and never overlap or repeat. Wiring maps onto type: Push swells the word and its outline halo with the bass, Ripple fires echo outlines that ring out (the reverb), Roughness adds colour-split ghosts on the highs, and Sparks fly off the word. Render: Filled draws gradient ink, Lines draws neon outlines, Dots draws an LED dot matrix that a shockwave runs through. The default collection is 30 feelings × 9 languages (en, pt, de, ja, zh, es, hi, fr, ar), plus 24 extra kanji with meanings. Each script gets its own typefaces from the macOS system fonts: Didot, Avenir Next, Hiragino, Songti/PingFang, Kohinoor Devanagari, Diwan Thuluth/Geeza Pro. Arabic and Devanagari never get letter spacing, because it breaks their joins. **Kanji only** (Form › Words) shows Japanese words written only in kanji, larger, with 3+ characters set vertically. A Words panel section has language toggles (kept per browser) and your own word list, one per line, which is saved in the library and merged as a union. Your words take every other turn as the big word. | User request: multilingual words about music and togetherness, own word list, language toggle, kanji-only mode |
| D11 | Video | Media slot: local clip folder or file picker, plus capturing a tab's video as a live source. Clips cut every N beats, stutter on a trigger, and change speed with energy. Video can be the background, appear inside the shape, or blend over it, and it still goes through symmetry and feedback. Built last. | YouTube and Instagram embeds can't be drawn into a canvas, YouTube embeds fail from `file://`, and its policies forbid overlays |

## Look model (plain JSON, everything editable)

```
{ name,
  form:       { type: blob|rings|ridges|cloud|grid|petals|arches|words, size, detail, sides, fly, script: all|kanji, pace },
  render:     { type: lines|filled|dots, width, trails },
  ripple:     { type: rim|shockwave|stone|roll|bloom, speed, size },
  palette:    name,
  symmetry:   { type: none|mirror|kaleido, segments },
  feedback:   { amount, zoom, twist },
  background: { type: solid|gradient|noise|stars },
  wiring:     { push, rough, spin, hue, ripple, zoom, shake, drift, bgpulse }
              // each one is { src: bass|mids|highs|beat|none, amt }
}
```
The six prototypes become built-in looks. Users can save their own looks; built-in looks can't be overwritten.

## Drawing pipeline (each frame)
1. Analysis produces bass, mids, highs, the beat pulse, recent ripples, the beat count and a drop flag.
2. Each look draws onto its own offscreen canvas. Order: background → feedback (the previous frame, slightly zoomed, twisted and faded) → camera transform → form points (base shape cached) → modifiers that push each point along its direction (push, roughness, ripple) → render → symmetry (copy the image into N mirrored or rotated slices).
3. The visible canvas shows the current look, or crossfades between two looks during a switch.
- Every form outputs a list of point paths. Each point carries a position, a push direction, its position along the path (0–1), its layer (0–1) and a weight. That shared format is what lets any part combine with any other.

## Rotation
- Switch after N beats; if no beat has been detected for 8 s, switch on a timer instead.
- Drop detection: short-term bass energy compared with long-term. A dip below 50% for more than 2 s, followed by a recovery, counts as a drop. It has a sensitivity setting and a minimum gap between switches.
- Crossfade or cut; in order or shuffled; ←/→ skip; space pauses; editing a look pauses the rotation.

## Deviations
1. **Built-in looks are editable.** Edits save automatically; there's a Reset button for built-ins and a Duplicate button to branch off. This replaces the planned "Save as" with built-ins locked. It means one fewer step before you can play with a look.
2. **Built-ins refresh when the code changes.** A `REV` number is saved with the looks. When it changes, saved copies of built-ins are replaced with the new code versions, keeping their rotation tick. Edits to a built-in are lost on such an upgrade, so use Duplicate to keep them.
3. **Visual upgrade, requested mid-build ("bolder, more explosive, stylish"):**
   - Added a **Post FX** slot: one WebGL2 pass providing glow, colour split, a shockwave that warps the whole picture, grain and vignette. WebGL is built into the browser, so there are still no libraries. Without WebGL2, the page shows plain output without the effects.
   - Added a **Sparks** slot (bursts on the beat).
   - Added iridescent outlines and 3 new palettes.
   - Added an 8th built-in look, *Supernova*.
   - Re-tuned all built-ins to be punchier.
4. **Tuning changes:**
   - Highs react more strongly (curve 0.7, release 0.3 s), because hi-hats are too short to register otherwise.
   - Colour shift reduced to 50°; 120° destroyed the palettes' identity.
   - The glow uses screen blending, so large bright areas don't clip to white.
5. **The clip folder isn't remembered between sessions.** Pick it once each session. Skipped storing the folder handle in IndexedDB; add it if picking the folder gets tedious.
6. **The screen-share video track keeps running while capturing system audio**, because stopping it can end the share in some Chrome versions. It costs a little CPU.
7. **Background video no longer goes through Symmetry.** It moved behind the shapes so background moments can texture it without touching the shapes. The kaleidoscope still applies to the shapes, the Mirror moment mirrors the background, and video set to "inside" or "blend" still goes through symmetry.
8. **Saved data from earlier versions is migrated automatically.** Your own looks are kept, and the ticks become a set called "My set". A built-in you had edited becomes your own look named "… (my edit)". Deletions don't carry across merges, so a look deleted here can come back if you import an older file that still contains it.
9. **Drafts last for the session only.** They aren't stored; the browser warns before you leave with unsaved edits.
10. **Palette hue ranges now wrap forward.** sunset and blood had been interpolating through cyan and green. This also removes the lime tint from Mandala and Swarm.
11. **Ridges and arches now erase what's behind them instead of painting the palette background over it**, so a video or textured background shows through the gaps. Side effects: with Use video set to "Inside shape", the video now appears only on the lines, not in the filled ridges. With Additive glow, nothing is hidden, as before.
12. **Burn transition fixed.** Its blend ran backwards: it started on the new look, burned towards the old one, then snapped to the new one when it ended.
13. **A zero-size window no longer freezes the visuals.** If the page loaded hidden (0×0), `drawForm` threw on a non-finite gradient, and the animation loop stopped for good. Frames are now skipped until the window has a size.
14. **Words: the new built-ins are not added to existing sets.** A fresh install puts them in the Main set; on an existing library, tick Words and Kanji to add them.
15. **The visuals recover from a lost GPU context.** A GPU reset, sleep/wake or a GPU switch can drop the WebGL context. Before, the screen then stayed black until a reload. Now the page asks for the context back, shows black while it is gone, and rebuilds the post-FX pass when the browser restores it. Tested by forcing a loss and a restore with `WEBGL_lose_context`. Limit: if Chrome refuses to restore it, for example after repeated GPU crashes, the page still needs a reload.

## Verification (2026-10-09, in the built-in browser using the demo signal)
- **Self-test passes** (`selfTest()` in the console, or open with `#test`). It checks:
  - every editor field exists in the base look;
  - built-ins are valid;
  - junk or prototype-pollution input is rejected;
  - every form builds without errors.
- **Beat detection:** a beat every ~0.49 s, matching the demo's 122 BPM, with no false beats during the breakdown.
- **Drop detection** fires after the breakdown.
- **Rotation** switches every N beats, in order.
- **All 8 looks render** at about 0.6 ms per frame (CPU + GPU) at 600×1674.
- **Video clips:** loading, cutting on the beat, and the inside-shape and background modes all work, tested with generated clips.
- **The page keeps working when browser storage is disabled.**

**Not verifiable here** (needs your Chrome and real music):
- capturing system audio, tab audio and microphone;
- capturing a tab's video;
- beat and drop accuracy on real tracks;
- frame rate fullscreen on a 4K/5K display.
