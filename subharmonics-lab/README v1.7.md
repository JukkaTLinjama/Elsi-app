# Subharmonic Beat Lab / Tactile Grid Carrier

Author: Jukka Linjama, 2026

Current working version: `subharmonic-beat-lab-v1.7-two-column-readout.html`

## Purpose

This lab explores how low-frequency carrier structures can add tactile support to music.

The current prototype is deliberately simple:

- the keyboard directly selects the body carrier frequency
- a ratio-derived subharmonic is added below the carrier
- small left/right frequency differences create stereo phase rotation
- an optional harmonic-grid point can add upper color
- a short mono attack crossfades into the stereo body stack
- live Lissajous views show the modeled left/right field

The lab is a research instrument, not a finished music processor.

## Current signal model

The pressed key drives the carrier directly:

```text
carrier = pressed note
```

There is currently no octave fold, frequency compression, target scoring, or automatic 30–45 Hz carrier selection.

For a selected sub ratio `m/n`:

```text
sub = carrier × m/n
grid = carrier / n
grid point = 10 × grid
```

Example with a 55 Hz carrier and ratio 2/3:

```text
carrier = 55.00 Hz
sub = 36.67 Hz
grid = 18.33 Hz
grid point = 183.33 Hz
```

Important: `Grid 10` means the tenth point of the inferred grid. It is not the tenth harmonic of the carrier.

## Stereo field

The carrier is split symmetrically around its center frequency:

```text
baseL = carrier − Δf/2
baseR = carrier + Δf/2
```

The left sub remains ratio-locked to the left base:

```text
subL = baseL × m/n
```

The right sub can be shifted independently:

```text
subR = baseR × m/n + subRDetune
```

`Stereo Δf` is also the relative phase-rotation rate of the carrier pair. A 0.7 Hz difference produces one full relative phase cycle in approximately 1/0.7 seconds.

The working hypothesis is that slow stereo phase motion may feel more organic than applying the same motion as ordinary amplitude tremolo.

## Attack and body crossfade

Each key press starts with a short mono sine attack at the pressed frequency. The attack then crossfades into the stereo body stack.

Current internal timing:

```text
attack hold = 0.15 s
crossfade = 0.60 s
```

The carrier frequency no longer changes during this transition. The crossfade changes the structure from a centered attack to the stereo base + sub + optional grid field.

This replaced pitch glide because an audible glide between the key and a computed target sounded musically incorrect.

## Controls

### Transport

- `Rec loop` records monophonic gate and pitch events.
- `Play loop` repeats the recorded phrase.
- `Render WAV` renders the current loop, or a five-second static sound when no loop exists.
- `Master Volume` controls the output level.
- `Glide` smooths frequency changes used by applicable parameter and loop paths. The attack-to-body transition itself is a crossfade.

### Structure

- `Sub ratio` selects the rational subharmonic relationship.
- `Stereo Δf` separates the left and right carrier frequencies.
- `Sub R Detune` shifts only the right sub frequency and intentionally breaks the right-channel harmonic lock.

### Levels

- `Base level` controls the carrier pair.
- `Sub level` controls the ratio-derived sub pair.
- `Grid 10 level` controls the optional `10 × grid` oscillator pair. Its default is zero.

## Readout

The readout is divided into two columns.

### Frequencies

- pressed note
- direct carrier
- body center
- base L/R
- sub L/R
- grid
- grid point

### Relations & motion

- carrier-to-note relation
- selected ratio
- base and sub Δf
- second-order beat
- right-sub detune and applied shift
- harmonic mismatch
- common repetition rates
- sub-frequency limiter status

The columns collapse into one column below a 680 px viewport width.

## Lissajous visualization

The lab shows three live views:

- Base L/R
- Sub L/R
- Sum L/R

The scopes use:

```text
x = modeled left-channel signal
y = modeled right-channel signal
```

They are generated from the engine state. They do not measure the Web Audio output, transducers, furniture, or the body.

Current visualization behavior:

- approximately 30 fps refresh
- fixed 3000 Hz visual sampling grid
- maximum 200 new points per frame
- drawing only while the audio gate is open
- fading point history after release

The fixed visual sample grid keeps the display tied to absolute time instead of changing its sampling density with oscillator frequency.

## Development since v0.7

The v0.7 line established the simplified base + sub engine and absolute-time Lissajous sampling.

Later experiments added:

- an optional upper harmonic-grid point
- independent right-sub detune
- harmonic mismatch and repetition-rate observability
- a short played-note attack followed by body-stack crossfade
- smaller Lissajous panels below the keyboard
- grouped controls and a two-column readout

Several automatic carrier-selection experiments were also tested:

- harmonic-grid target scoring in the 30–45 Hz range
- preference for targets near 36 Hz
- continuity weighting from the previous target
- an untouched zone

These experiments were rejected. With many rational candidates available, the center-frequency term caused most notes to collapse near 35–36 Hz. The keyboard then stopped producing a meaningful carrier contour.

The untouched-zone experiment did not solve the underlying scoring problem and was removed.

Current v1.7 therefore uses direct 1:1 keyboard-to-carrier control. This is a clean baseline for evaluating the attack, stereo motion, subharmonic ratio, and grid color independently of carrier selection.

## Main theoretical ideas

### Harmonic grid

A frequency set can be described as integer points on a shared temporal grid:

```text
frequencies = indices × grid
```

For 40 Hz and 60 Hz:

```text
40 = 2 × 20
60 = 3 × 20
grid = 20 Hz
```

The grid does not need to exist as an audible oscillator. It may describe a shared repetition structure or virtual periodicity.

Working hypothesis:

```text
Perceived coherence may reflect how strongly a frequency set supports
a stable shared temporal grid.
```

This is a research hypothesis, not an established result of the lab.

### Phase motion

The current conceptual separation is:

```text
grid / ratio  → what frequencies form the field
stereo Δf     → how the field rotates in phase
sway          → the slower time scale on which movement breathes
```

The desired slow movement is approximately 0.3–1 Hz. The current UI still allows a wider 0–8 Hz Δf range for comparison.

## Known limitations

- Direct keyboard control spans A0–A1, approximately 27.5–55 Hz, so the carrier is not restricted to the preferred tactile 30–45 Hz region.
- The current `Grid 10` oscillator can enter an audible upper-bass or low-midrange region and may sound too tonal.
- The label and internal variable name `grid5` are historical even though the current completion index is 10.
- The attack and offline WAV renderer are not fully identical: the offline render currently omits the short attack layer.
- The Lissajous display is a mathematical model, not a physical measurement.
- Perceived results depend strongly on the transducer, mounting, furniture, listening level, and body coupling.

## Suggested next experiments

Change only one mechanism at a time.

1. Evaluate direct carrier control with `Grid 10 level = 0`.
2. Test slow `Stereo Δf` values around 0.3–1 Hz.
3. Add a dedicated low-level harmonic-color control only after the base + sub field is understood.
4. Compare `10 × grid` with a simpler candidate such as `base + grid`.
5. Revisit automatic carrier selection only after defining what the target should optimize: tactile strength, musical relation, continuity, or user preference.

Do not yet combine carrier scoring, sway modulation, multiple upper algorithms, and chord detection in the same experiment.

## Experimental discipline

Keep these layers conceptually separate:

```text
harmonic-grid logic
stereo renderer
attack / transition
physical transducer and body response
```

Recommended workflow:

```text
change one mechanism
listen and feel
observe the modeled field
record the result
then decide
```
