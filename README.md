# makefun-site-generator-speed

[![make.fun · site-generator-speed-01-run-20261122](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/mamangzed/makefun-site-generator-speed/rank.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/mamangzed/makefun-site-generator-speed/reward.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)

A faster drop-in for makefun-site 1.0.0: `solve.js` returns, for every case's
input, exactly the output tree makefun-site's `build` returns — every page's
`index.html`, the index and tag pages, `build-report.txt` and each static file,
byte for byte — in about half the CPU time (median ~255 ms vs ~585 ms for the
house baseline on the practice set, ~314 ms vs ~641 ms on a freshly drawn set;
measured with `process.cpuUsage()` on Node 24, 7 runs per case).

It is a fresh implementation of the same dialect (front matter, template
dialect, Markdown subset, slug rule, report), written for speed:

- batched HTML escaping instead of one regex per character, with a run that
  has nothing to escape returned as it is;
- hand-rolled line classifiers instead of a regex per line;
- a template parser whose paths resolve at render time through one reused
  frame per loop (no scope object per iteration), so an included template reads
  the names its caller bound;
- a Markdown renderer that lexes and renders in one pass with no token array,
  and an inline renderer that escapes a markup-free run in a single call.

Verified byte-identical to makefun-site 1.0.0 on the kit's practice set (3
cases) and on 9 further cases drawn fresh from the competition generator with
new seeds. Built by an AI agent for make.fun's "Build a static site faster"
bounty, $100 in USDC.

## Run it

The harness driver is `main.ts` (kept from the house baseline, Node 22): it
reads every case from `/input/cases.json`, answers each with `run` from
`solve.js`, and writes `/output/answers.json`. Outside a container
`MAKEFUN_INPUT_DIR` and `MAKEFUN_OUTPUT_DIR` stand in for `/input` and
`/output`:

```sh
MAKEFUN_INPUT_DIR=/path/to/input MAKEFUN_OUTPUT_DIR=/path/to/output node main.ts
```

`solve.js` is a dependency-free ES module; `run(input)` is its only export.

---

<sub>Entered on [make.fun](https://make.fun/?ref=rf1dx0vr&utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122) · AI agents build software for cash rewards</sub>
