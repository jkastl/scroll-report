# Scroll Report

A rolling 3D history of what your phone feels. Motion is recorded at the head of a
road and the last 20 seconds roll back along it toward you, like paper out of a
seismograph. Drag to look at it from any angle.

**[jkastl.github.io/scroll-report](https://jkastl.github.io/scroll-report/)**

One file, no build step, no dependencies, no network calls.

## Reading it

The history is drawn as a ribbon on a road, with time running back along it:

- **X** (red axis): left/right, across the road
- **Z** (blue axis): up/down, lifts the ribbon off the road

Forward/back isn't recorded. The road already runs that way as time, and giving it a
second meaning made the picture ambiguous.

A small **airplane** rides at the record head, nose up the road, and the ribbon trails
behind it like a contrail. It slides with X and lifts with Z. It also gets two "how it felt" cues, which aren't
the aircraft's real attitude:

- **Lean:** a bank of up to 12° toward the sideways push, following a 0.4s average of X
  and reaching the cap at 0.3G.
- **Wobble:** up to 10° of shake, sized by how rough the last half second was. Only
  quick changes in X and Z count; steady pushes don't.

Invert flips the lean. It's red
while live, with a faint pulse, and grey before you start or when the sensor stalls.

The ribbon's grey **shadow** on the road is the same history with Z removed, and the
thin **stalks** join the two, so height can be read from any angle. The lane edges mark
±0.5G for X, a tick on the Z axis marks +0.5G, and the chart rules every 2s scroll with
the data. Older history fades as it
travels toward −20s.

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

## Controls

| Button | What it does |
| --- | --- |
| Pause / Resume | Freezes the display and stops recording. Resuming carries on from the frozen history with no gap |
| Invert | Flips both axes to show the felt force (the push you feel) instead of the device's own movement. Off by default, applies to the existing history too, and is remembered |
| Peak | Draws one dashed line per axis along the whole road at that axis's biggest value on the road right now, on the side it happened, labelled at the −20s end. The lines update as moments scroll off. Off by default, and remembered |
| Axes | Shows or hides the X/Z arrows at the record head. On by default, and remembered |
| Keep Awake | Holds a screen wake lock while the page is visible |
| Smoothing | 0.15s low-pass on both axes. On by default |
| View | Glides to the next camera preset, or back to the last one after a drag |
| Theme | Cycles Auto → Light → Dark. Auto follows the OS, and the choice is remembered |

## Signal

- Uses `acceleration` (gravity removed). Devices that only report
  `accelerationIncludingGravity` get a 1s gravity high-pass, and the status reads
  `Live · est. gravity`.
- Sign convention is detected, not assumed from the user agent. The spec's gravity
  reaction points up out of a screen someone is looking at, and iOS reports it (and
  everything else) negated, so its sign decides whether to flip the event.
- Screen left/right is rotated out of the hardware X/Y axes, so it stays right in
  landscape. The hardware Y is read for that and for the sign check, but isn't recorded.
  The angle comes from `window.orientation` first, because iPadOS leaves
  `screen.orientation.angle` at 0.
- There's a soft 0.01G noise floor on the X/Z magnitude. History is binned by absolute
  time (the mean per 2px row), so it scrolls smoothly at any sample rate.

## Development

Edit `index.html` and test on a real device over HTTPS (iOS needs a tap for motion
permission). A desktop browser renders the empty scene only. Push to `main` to
deploy through GitHub Pages.

Versioning is semver, shown in the header next to the release date.

## Open checks

Things to verify on a real ride. Tick them off or remove them once settled.

- [ ] **Wobble at cruise.** If the plane rocks noticeably in smooth cruise, the wobble
  threshold is too low. Raise `ROUGH_FULL_G` in `index.html` (currently 0.15G, the
  roughness that gives the full 10° wobble).
