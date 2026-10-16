# Backlog

## To do

- [ ] **Words & Kanji: slower big word, livelier small words.** The main word changes too often by default. It should stay on screen longer, and the small words in the background should carry the beat instead, popping in and out with the beat or sound.
  - Big word: defaults are `pace: 2` beats for Words and `pace: 4` for Kanji (`visualizer.html`, `def('words', …)` / `def('kanji', …)`; the base default is in `form` near line 397). Raise both, likely to 8, and consider adding a 16-beat option to the **New word every** control. Also check the no-beat fallback timer in `drawWords()` (`f.pace * 0.5` seconds), so quiet passages slow down too.
  - Small words: right now one fades in per beat and lives 4–8 beats. Make them snappier, with shorter lives and a pop on beats or loud moments, so the motion comes from the background while the big word holds.
- [ ] **Mandala: fill the empty black space.** The arch vortex sits in the middle and leaves a lot of black around it. Add stylish circles around it in the look's own style, such as smaller arch rings or seigaiha-style medallions in the same palette, that react to the music. Built-in: `def('mandala', …)` with form `arches` in `visualizer.html`.

## Japanese-inspired visuals (unsorted)

These are the mockups from `japanese-mockups.html` (untracked, in the main checkout) that haven't been built yet. Seigaiha sea is already done. They still need sorting: keep the ones worth building and drop the rest.

- [ ] **Torii tunnel:** vermilion gates rush past, faster on the bass, with lanterns and a dusk glow at the end. A new gate shape for the existing ring tunnel.
- [ ] **Sakura storm:** blossoms drift on a swirling wind and burst outward on every beat, under a moon and a dark branch silhouette.
- [ ] **Sumi-e ensō:** one ink brush stroke per 16-beat phrase that swells with the bass, with ink spatters on the beat and a red seal stamp. Comes with an ink-on-paper theme.
- [ ] **Kintsugi drop:** gold cracks spread across dark glaze on a drop. A background moment or transition over any look.
- [ ] **Ukiyo-e theme:** Prussian blue, cream paper, a red sun, print dots and a title cartouche. A theme for any look, shown on Ridgelines in the mockup.
