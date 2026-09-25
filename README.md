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

The history is a real 3D scene: X runs across the road, time runs back along it, and
Z lifts off it. **Drag** anywhere on the plot to orbit the camera around it. Horizontal
drags spin it and vertical drags tilt it, stopping just above the floor. The camera
stays where you leave it, and the View button then reads **Free**.

**View** glides between the presets. After a drag, the first tap returns to the preset
you left; after that, taps move on to the next one. The camera is remembered on the
device.

| Preset | Camera |
| --- | --- |
| Road (default) | Behind and above the record head, a little to the left. Recording happens at almost full width near the top, and history rolls toward you, widening as it comes |
| Chase | High three-quarter view from behind and to the side |
| Side | Side-on. Time runs right to left and Z is a true vertical. It's the best one in landscape |
| Top | Straight down: a seismograph strip of X over time |
| Head-on | Standing past the record head looking back, so history recedes away from you |

Forward/back (Y) sets the **paper feed speed**. Accelerating forward feeds the road
faster, so that stretch of history is laid out longer; braking slows the feed and
squeezes it. The feed runs from ¼× to 3× (+0.4G hits the top). The time labels and 2s
rules ride along with the stretch, so a gap between rules that's wider than its
neighbours is a moment of forward push. The Y arrow points the way the road feeds.

## Controls

| Button | What it does |
| --- | --- |
| Pause / Resume | Freezes the display and stops recording. Resuming carries on from the frozen history with no gap |
| Invert | Flips all three axes to show the felt force (the push you feel) instead of the device's own movement. Off by default, applies to the existing history too, and is remembered |
| Keep Awake | Holds a screen wake lock while the page is visible |
| Smoothing | 0.15s low-pass on all three axes. On by default |
| View | Glides to the next camera preset, or back to the last one after a drag |
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
