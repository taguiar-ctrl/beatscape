# Backlog

## To do

- [ ] **Words & Kanji: slower big word, livelier small words.** The main word changes too often by default. It should stay on screen longer, and the small words in the background should carry the beat instead, popping in and out with the beat or sound.
  - Big word: defaults are `pace: 2` beats for Words and `pace: 4` for Kanji (`visualizer.html`, `def('words', …)` / `def('kanji', …)`; the base default is in `form` near line 397). Raise both, likely to 8, and consider adding a 16-beat option to the **New word every** control. Also check the no-beat fallback timer in `drawWords()` (`f.pace * 0.5` seconds), so quiet passages slow down too.
  - Small words: right now one fades in per beat and lives 4–8 beats. Make them snappier, with shorter lives and a pop on beats or loud moments, so the motion comes from the background while the big word holds.
