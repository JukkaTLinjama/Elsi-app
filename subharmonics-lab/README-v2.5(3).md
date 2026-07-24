# Subharmonic Beat Lab / Tactile Grid Carrier

Author: Jukka Linjama, 2026

Current working version: `subharmonic-beat-lab-v2.5-actuator-motion.html`

## Purpose

This lab explores how low-frequency carrier structures can add tactile support to music.

The current prototype is deliberately simple:

- the keyboard selects the source note and a 0...+12 semitone slider transposes the body carrier
- Stereo Beat Solo crossfeeds the close carrier pair to create a controlled slow beat in both outputs
- a complementary equal-power gain sway moves emphasis slowly between the outputs
- a separate lower layer creates a directly chosen tactile difference frequency below the base pair
- the earlier ratio-derived subharmonic and harmonic-grid layers remain available in Layered mode
- a short mono attack crossfades into the stereo body stack
- live Lissajous views show the modeled left/right field
- a compact Actuator Motion panel shows the modeled Base drive, electrical L+R sum, and real-time Tactile Difference pulse

The lab is a research instrument, not a finished music processor.

## Current signal model

The pressed key and transpose setting drive the carrier:

```text
carrier = pressed note × 2^(transposeSemitones/12)
```

At zero transpose, the relation is direct 1:1. There is currently no octave fold, frequency compression, target scoring, or automatic 30–45 Hz carrier selection.

The loop stores the original keyboard frequencies. Transposition is applied only at the audio-engine boundary, so the same loop can be moved in equal-tempered semitone steps up to one octave higher and returned exactly to its original range.

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

`Slow Beat Δf` is also the relative phase-rotation rate of the carrier pair. A 0.7 Hz difference produces one full relative phase cycle in approximately 1/0.7 seconds.

The working hypothesis is that slow stereo phase motion may feel more organic than applying the same motion as ordinary amplitude tremolo.

### Stereo Beat Solo

Stereo Beat Solo is the v2.0 default. It adds normalized crossfeed between the close carrier oscillators:

```text
outputL = direct × baseL + cross × baseR
outputR = direct × baseR + cross × baseL
```

`Stereo Beat Depth` sets the cross/direct ratio. The gains are normalized by `1 + depth`, so increasing beat depth does not simply increase the maximum base level.

In Solo mode:

- the Base pair and crossfeed remain active
- Sub and Grid 10 are bypassed
- stored Sub and Grid settings are preserved
- turning Solo off restores the earlier layered field

### Stereo gain sway

v2.2 replaces the inaudible common carrier drift experiment with complementary equal-power gain movement:

```text
sway = sin(2πt / period)
gainL = sqrt(1 + depth × sway)
gainR = sqrt(1 − depth × sway)
```

This keeps `gainL² + gainR² = 2` while emphasis moves between the outputs. Carrier frequencies and Slow Beat Δf remain unchanged. The default sway is 20% with a 12-second period.

Stereo gain sway is bypassed in Layered mode but its settings are preserved.

### Tactile Difference layer

v2.4 replaces the second-harmonic color experiment with an independent lower pair:

```text
lowerL = baseL − tactile difference
lowerR = baseR − tactile difference
```

The selected Tactile Difference is therefore the direct base−lower difference in both channels. It has no ratio, crossfeed, or sway of its own. This makes it a clean test of the mechanism that was strongest in the earlier Base + Sub field.

The layer is separate from the stored ratio-derived Sub layer and stays available in both Solo and Layered modes.

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

- `Stereo Beat Solo` switches between the isolated slow stereo beat and the stored Base + Sub + Grid field.
- `Slow Beat Δf` controls both the local slow-beat rate and carrier-pair phase-rotation rate.
- `Stereo Beat Depth` controls normalized crossfeed and therefore slow-beat depth.
- `Stereo Sway Depth` controls complementary equal-power gain movement from 0 to 50%.
- `Stereo Sway Period` controls its cycle from 1 to 30 seconds.
- `Sub ratio` selects the rational subharmonic relationship.
- `Sub R Detune` shifts only the right sub frequency and intentionally breaks the right-channel harmonic lock.

Sub-related controls are visibly inactive in Stereo Beat Solo mode but retain their values.

### Levels

- `Base level` controls the carrier pair.
- `Sub level` controls the ratio-derived sub pair.
- `Grid 10 level` controls the optional `10 × grid` oscillator pair. Its default is zero.
- `Tactile Difference` sets the base−lower separation from 1 to 20 Hz; its default is 10 Hz.
- `Tactile Difference Level` controls the separate lower pair.

### Keyboard

- `Transpose` moves the carrier from 0 to +12 semitones in whole-semitone steps.
- Changing transpose affects a running loop immediately without retriggering the attack.
- The existing `Glide` value controls the transition time.

## Readout

The readout begins with a motion hierarchy and continues as two detail columns.

### Motion hierarchy

- `Slow stereo beat` shows Δf, crossfeed depth, and cycle duration.
- `Stereo gain sway` shows its depth and period while confirming constant summed power.
- `Tactile difference` shows the direct base−lower rate and the lower L/R frequencies.
- `Tactile beat` is the average of the audible base−sub difference frequencies in the left and right channels.
- `Sway` is the difference between the left and right tactile-beat rates.
- `Stereo phase rotation` shows the signed base and sub L/R frequency differences.

When Solo bypasses the Sub layer, Tactile beat and Sway show `INACTIVE` instead of theoretical values that are not present in the audio.

For each motion rate, the UI also shows the corresponding cycle duration. This separates fast tactile pulse, slow sway, and stereo phase motion.

### Frequencies

- pressed note
- transposed carrier
- body center
- base L/R
- tactile difference L/R
- sub L/R
- grid
- grid point

### Structure & mismatch

- carrier-to-note relation
- selected ratio
- harmonic structure and mismatch
- right-sub detune and applied shift
- harmonic mismatch
- nominal grid repetition rates
- sub-frequency limiter status

The columns collapse into one column below a 680 px viewport width.

## Lissajous visualization

The lab shows four live views:

- Base L/R
- Sub L/R
- Tactile Difference L/R
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

## Actuator Motion visualization

v2.5 adds a compact phase-oriented Actuator Motion panel below the Lissajous views.

The panel no longer repeats the instantaneous L/R values already visible in the scopes. Instead, each actuator is shown as a circular phase indicator:

- the large pointer angle shows the corresponding Base oscillator phase
- a faint dashed pointer shows the fixed shared Base reference
- the left and right pointers rotate symmetrically in opposite directions around that reference
- their relative angle advances at the real `Slow Beat Δf` rate
- pointer length follows the real complementary equal-power Stereo Sway gain
- a thinner pointer shows the lower Difference oscillator phase
- its angle relative to the Base pointer rotates at the real Tactile Difference rate
- the halo and one-second Tactile Pulse trace continue to show the Base–Lower interference envelope

The shared Base reference is fixed. Only the real relative stereo phase motion is shown:

```text
common visual phase = 0°
base phase L/R = ∓ π × Slow Beat Δf × time
lower phase L/R = base phase L/R − 2π × Tactile Difference × time
```

The one-second Tactile Pulse trace freezes when the key is released, so the latest pulse pattern remains available for inspection. A new played note starts updating the trace again.

Therefore a 0.7 Hz Slow Beat still produces one full L/R relative phase cycle in approximately 1.43 seconds, and a 10 Hz Tactile Difference produces ten Base–Lower phase rotations and pulse cycles per second.

Crossfeed makes the complete routed channel multicomponent, so the pointer angle is explicitly the Base oscillator phase, not a claimed single phase angle for the entire channel output. Crossfeed remains visible in the Lissajous views and in the interference-envelope calculation.

The panel is an electrical control-signal model. Pointer size, halo size, and brightness are illustrative. It does not model actuator resonance, mounting, furniture, body loading, or measured mechanical displacement.

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

The v1.7 baseline restored direct 1:1 keyboard-to-carrier control. v1.8 added manual transposition after the stored source note. v1.9 restricts this to 0...+12 equal-tempered semitones and adds an explicit motion hierarchy. v2.0 adds Stereo Beat Solo with normalized crossfeed while retaining the layered engine for A/B comparison. The v2.1 common carrier drift experiment had no clear perceptual effect. v2.2 replaces it with complementary equal-power stereo gain sway. v2.3 tested normalized second-harmonic color, but its physical effect was too subtle to justify keeping it in the active test. v2.4 replaces it with a direct, separately adjustable Base−Lower tactile-difference layer. v2.5 adds the compact Actuator Motion electrical model without changing the audio engine.

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

The desired slow movement is approximately 0.3–1 Hz. The v2.0 Slow Beat Δf control covers 0–2 Hz and defaults to 0.7 Hz.

## Known limitations

- The source keyboard spans A0–A1, approximately 27.5–55 Hz. Manual transposition extends the carrier to 55–110 Hz at +12 semitones, but it does not automatically keep the carrier in the preferred tactile 30–45 Hz region.
- The current `Grid 10` oscillator can enter an audible upper-bass or low-midrange region and may sound too tonal.
- The label and internal variable name `grid5` are historical even though the current completion index is 10.
- The attack and offline WAV renderer are not fully identical: the offline render currently omits the short attack layer.
- The Lissajous and Actuator Motion displays are mathematical electrical-signal models, not physical measurements.
- Perceived results depend strongly on the transducer, mounting, furniture, listening level, and body coupling.

## Suggested next experiments

Change only one mechanism at a time.

1. Evaluate Stereo Beat Solo at Δf values around 0.3–1 Hz.
2. Set Beat Depth to 0% to separate physical L/R coupling from electrical crossfeed beat.
3. Compare stereo sway depths around 10–40% with periods around 8–20 seconds.
4. Toggle Solo off for direct comparison with the stored Base + Sub + Grid field.
5. In Solo, compare Tactile Difference values around 4, 8, 10, 12, and 16 Hz while changing only its level.

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


### v2.5 visualization refinement

- The Base phase pointer now has a fixed length.
- Equal-power sway is shown by the brightness and thickness of each actuator outer ring.
- Tactile Difference is shown as a blue marker on a local phase ring referenced to that channel's own Base pointer, rather than as a second absolute phase pointer.
- The Stereo Sway Period control now spans 1–30 seconds; its default remains 12 seconds.
- The latest one-second Tactile Pulse trace remains visible after key release.
