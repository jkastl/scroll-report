# Scroll Report

A rolling 3D history of what your phone feels. The record head sits near the top
of the screen and the last 20 seconds of motion scroll down away from it, like
paper out of a seismograph.

**[jkastl.github.io/scroll-report](https://jkastl.github.io/scroll-report/)**

One file, no build step, no dependencies, no network calls.

## Reading it

Each moment is a 3D point drawn in an oblique projection:

- **X** (red axis): left/right, drawn across the screen
- **Y** (green axis): forward/back, drawn receding up and to the right
- **Z** (blue axis): up/down, lifts the ribbon off the floor

The ribbon's grey **shadow** on the floor is the same history with Z removed, and
the thin **stalks** join the two, so height and depth can be told apart. The lane
edges mark ±0.5G, and the chart rules every 2s scroll with the data. Older history
narrows and fades as it drops toward −20s.

## Views

**View** switches between two ways of drawing the same history. The choice is remembered.

- **Drop**: an oblique projection. The record head sits near the top and
  history falls away down a narrowing lane.
- **Road** (default): a perspective camera sitting behind and a little to the left of the record
  head. Recording happens at almost full width near the top, and history travels toward
  you, widening as it comes, like watching a road roll by. Forward/back (Y) moves a sample
  along the road, so its gizmo arrow points up the road rather than being drawn to
  scale.

## Controls

| Button | What it does |
| --- | --- |
| Pause / Resume | Freezes the display and stops recording. Resuming carries on from the frozen history with no gap |
| Invert | Flips all three axes to show the felt force (the push you feel) instead of the device's own movement. Off by default, applies to the existing history too, and is remembered |
| Keep Awake | Holds a screen wake lock while the page is visible |
| Smoothing | 0.15s low-pass on all three axes. On by default |
| View | Switches between Drop and Road |
| Theme | Cycles Auto → Light → Dark. Auto follows the OS, and the choice is remembered |

## Signal

- Uses `acceleration` (gravity removed). Devices that only report
  `accelerationIncludingGravity` get a 1s gravity high-pass, and the status reads
  `Live · est. gravity`.
- Sign convention is detected, not assumed from the user agent. The spec's gravity
  reaction points up out of a screen someone is looking at, and iOS reports it (and
  everything else) negated, so its sign decides whether to flip the event.
- X/Y are rotated from the hardware frame into the screen frame. The angle comes from
  `window.orientation` first, because iPadOS leaves `screen.orientation.angle` at 0.
- There's a soft 0.01G noise floor on the 3D magnitude. History is binned by absolute
  time (the mean per 2px row), so it scrolls smoothly at any sample rate.

## Development

Edit `index.html` and test on a real device over HTTPS (iOS needs a tap for motion
permission). A desktop browser renders the empty scene only. Push to `main` to
deploy through GitHub Pages.

Versioning is semver, shown in the header next to the release date.
