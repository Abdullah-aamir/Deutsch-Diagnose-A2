# Deutsch-Diagnose — installierbare Web-App (PWA)

A 100-question diagnostic quiz for German. It scores 16 grammar areas separately and
ranks them weakest first. That is all it does.

The content targets the A2 foundations that tend to go rusty by the time someone is
working at B1 — cases and endings, prepositions, pronouns, participles, clause order —
rather than the upper end of B1.

## Files

| File | Purpose |
|---|---|
| `index.html` | The whole app: questions, scoring, results |
| `manifest.webmanifest` | App name, icons, colours, standalone display |
| `sw.js` | Service worker: caches everything for offline use |
| `icons/` | App icons (192, 512, maskable 512, Apple touch, favicon) |
| `.nojekyll` | Tells GitHub Pages to serve the files directly, skipping the Jekyll build |

All paths are relative, so it works from any folder of any static host.

## Publishing and installing

Upload the **contents** of this folder to a repo root (GitHub Pages → Settings → Pages →
deploy from `main`, `/ (root)`), or drag the folder onto app.netlify.com/drop. Then open
the URL in Chrome on Android → ⋮ → **App installieren**. Works offline after the first load.

## How the diagnosis works

- 100 items across 16 areas, 4–9 items each, weighted by how much trouble each area
  usually causes rather than spread evenly.
- Questions rotate through the areas, never in topic blocks — consecutive questions from
  one area prime the answers and inflate the score.
- Option order is shuffled at load, so the correct answer isn't in a fixed slot.
- No feedback during the test. Feedback mid-test turns a measurement into a lesson and
  the later answers stop being diagnostic.
- Thresholds: **under 60 %** = eingerostet, **60–79 %** = wackelig, **80 %+** = sitzt.

An unfinished run is saved in the browser, so closing the tab doesn't lose it — reopening
offers **Fortsetzen**. Finishing the test clears it, so the next visit starts clean.

## Editing

The question bank is the `Q` array near the top of the `<script>` block. One item:

```js
{c:'adj', s:['Ich habe gestern ',' Mantel gekauft.'], h:'ein · warm',
 a:'einen warmen', o:['einen warmen','einen warme','ein warmen','einem warmen'],
 w:'Akkusativ maskulin nach ein.'}
```

`c` is the category id from `CATS`, `s` is the sentence split around the gap, `h` is the
hint under it, `a` the correct answer, `o` the four options (must include `a`), `w` a note
on the rule. `w` is not shown anywhere — it is there so the answer key is documented when
you edit. Add or remove items freely; scoring and ordering adapt to whatever is in the array.

After editing, bump `diagnose-v1` in `sw.js` to `diagnose-v2`, or installed phones keep
serving the old copy.
