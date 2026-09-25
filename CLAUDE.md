# scroll-report

Scroll Report: a single-file rolling 3D motion history for iOS Safari and Android Chrome.
`index.html` is the whole app. No build step, no dependencies. Hosted on GitHub Pages
from `main`. See README.md for behavior.

## Instructions for Claude

- Keep everything in `index.html`.
- Semver: bump `vMAJOR.MINOR.PATCH` in the `#version` span with every change (patch for
  fixes, minor for features) and set the date next to it (`YYYY-MM-DD`).
- Keep README.md in sync with behavior changes.
- Theme colors live in CSS custom properties. The dark palette is declared twice (for the
  `data-theme="dark"` override and the `prefers-color-scheme` media query), so edit both.
