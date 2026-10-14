# Music Visualizer

A beat-reactive music visualizer in a single HTML file: no build step, no libraries. It listens to your computer's audio, splits it into bass, mids and highs, detects beats and drops, and drives a set of swappable visual looks with them.

**Live:** https://taguiar-ctrl.github.io/beatscape/ (open in Chrome)

## Run it

- Open the live link above in **Chrome**, or open `visualizer.html` from a local copy.
- Under **Controls → Sound**, pick a source:
  - **System audio:** choose "Entire screen" and turn on "Share system audio". This needs Chrome.
  - **Tab audio:** share a tab that's playing music.
  - **Input device:** a microphone, or a loopback device like BlackHole.
  - **Audio file**, or **Demo** (a built-in synthesized track).
- Press **F** for fullscreen.

## What's in it

- **Looks** are built from parts: Form, Render, Ripple, Wiring (which sound drives which effect), Post FX, Sparks, Palette, Symmetry, Camera, Feedback echo, Background and Media. Built-in looks are templates: tweak one, then Save, Save as new, or Snapshot.
- **Sets** are playlists of looks that rotate on the beat or on drops, with stylish transitions (zoom punch, iris, glitch, burn, swirl). Each set has its own theme and settings.
- **Themes** restyle every look in a set: Neon trails, Noir grain, Ultra trippy, Warm, Cold, Vaporwave and more.
- **Background moments** briefly texture the video or background at high points in the music: VHS, halftone, pixelate, thermal, light leak, mirror.
- **Words:** a look of poetic words about music and togetherness in nine languages (English, Portuguese, German, Japanese, Mandarin, Spanish, Hindi, French, Arabic), set in each script's own typefaces and changing on the beat. Toggle languages and add your own words under **Controls → Words**. The **Kanji** look shows kanji only.
- **Video clips:** pick a folder of your own clips to cut on the beat, or capture a browser tab's video.
- **Library:** looks and sets save in the browser. In Chrome they can also be linked to a `looks.json` file.

## Keys

| Key | Action |
|---|---|
| ← → | Previous / next look |
| 1–9 | Jump to a look in the set |
| Space | Pause / resume rotation |
| S | Snapshot the current look |
| ⌘S | Save |
| T | Trigger a background moment |
| C | Cycle themes |
| R | Random look |
| F | Fullscreen |
| H | Hide the controls |

Design decisions and change history are in [implementation-notes.md](implementation-notes.md).
