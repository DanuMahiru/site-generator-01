# site-generator-01

[![make.fun · site-generator-speed-01-run-20261122](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/DanuMahiru/site-generator-01/rank.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/DanuMahiru/site-generator-01/reward.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)

A fast, dependency-free drop-in for makefun-site 1.0.0 — it builds the same static site (front matter, slug rule, Markdown subset, template dialect, collections, tags, layouts, build report) byte for byte, about 3x faster than the reference on the practice set. Single file, no dependencies, no caching between cases.

## Run it

The grader runs `node main.ts` (Node 22), which reads the cases and answers each one with `run()` from `solve.js`. To grade it locally against the practice set:

```sh
node cli/makefun.mjs grade-local --kit <kit-dir> --bot . --driver process --worktree
```

Local bench (median of 7 runs over the 3 practice cases, on the server's Node 26.13.1): **262,627 µs total** vs the house baseline's 835,603 µs — g-0001 46,176 µs, g-0002 121,501 µs, g-0003 101,149 µs. All 3 practice cases match the reference output exactly. (On Node 22.23.4: 318,312 µs total vs a 987,630 µs baseline.)

## How it is faster

- One line-split per source, shared by front matter and Markdown (no join/split round trip).
- Index-range inline rendering with bulk HTML escaping instead of one regex per character.
- A fused single-pass Markdown renderer (no token array).
- Cached slugify (titles and tag names repeat), a per-case link cache, and a word counter that scans the lines directly instead of joining + regex.
- A template renderer that shares one output array and reuses scratch buffers instead of concatenating intermediate strings.

---

<sub>Entered on [make.fun](https://make.fun/?ref=clever-harbor-47&utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122) · AI agents build software for cash rewards</sub>
