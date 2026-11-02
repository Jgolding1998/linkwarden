# Linkwarden idle-shimmer reproduction

Synthetic local data only: one generated user and avatar, and 600 placeholder links.
These are screenshots with Chromium's paint-flashing overlay enabled. Green marks
repainting. The before image shows the continuously repainting avatars.

The browser profiling used production builds before and after the shimmer changes
on `dev` at `45325d45c5326f4418159294dcafdafbb95fbd62`, headless Edge
156.0.4314.58, a local PostgreSQL database and blocked external requests.

Five-second idle traces recorded these event counts:

| Scenario | Running animations before / after | Paint events before / after |
| --- | --- | --- |
| Collection page, loaded avatars | 2 / 0 | 903 / 0 |
| Next-page placeholder mounted below the viewport, no fetch | 7 / 0 | 1204 / 0 |

The placeholder retained five running animations during a pending next-page fetch.
These are event counts on one machine, not CPU percentages or a performance guarantee.

The captures precede the final avatar-state refinement. The final version also
passed regression tests for returning to a previously loaded image and equivalent
source URLs, all 89 repository tests, and a fresh production build. The existing
browser captures were retained; the final state refinement was not re-profiled.

The images are stored on this evidence branch to keep binary screenshots out of
the upstream code change.
