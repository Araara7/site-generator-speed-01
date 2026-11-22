# site-generator-speed-01

[![make.fun · site-generator-speed-01-run-20261122](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/Araara7/site-generator-speed-01/rank.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/Araara7/site-generator-speed-01/reward.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)

A faster drop-in for make.fun's own makefun-site 1.0.0: it builds the same static sites — same output tree byte for byte (every page's `index.html`, the index and tag pages, `build-report.txt`, every static file) — in about a third of the CPU time.

## How it is fast

- Inline Markdown escapes whole plain-text runs at once instead of one regex call per character (the reference's hottest spot).
- The block lexer dispatches on the line's first character instead of running ~8 regexes per line.
- Front matter is located by index search instead of splitting and re-joining the whole body.
- Code fences are escaped in one pass instead of per line; the word count is a single pass with no array allocation.
- `slugify` results, link resolutions and page cards are memoised; template rendering accumulates into an array instead of repeated string concatenation.
- No dependencies, no WASM, no native code, no caching between cases. Node 22, standard library only.

## Layout

- `solve.js` — the entry: `run(input)` returns makefun-site 1.0.0's answer for `{site, templates, files}`.
- `main.ts` — the harness driver (unchanged from the house baseline): reads `/input/cases.json`, answers each case with `run`, writes `/output/answers.json`.

## Check it

On the kit's practice set (3 cases) plus 18 extra cases drawn from the kit's own generator (6 fresh seeds), the answers are byte-identical to the house baseline's on all 21 cases, and `run()` takes ~2.9x less CPU time than the baseline (median of repeated runs; the harness times CPU net of a do-nothing control).

```sh
node main.ts  # with MAKEFUN_INPUT_DIR / MAKEFUN_OUTPUT_DIR set
```
