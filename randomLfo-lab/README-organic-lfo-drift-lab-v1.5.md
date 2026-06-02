# Organic LFO Drift Lab v1.5

Standalone test application for developing an organic modulation / drift LFO for the Elsi Sleep Sound App.

## Purpose

This lab tests a smooth random modulation model before integration into the main Elsi app.

The goal is to replace a clearly periodic or stepped LFO feel with a more organic low-frequency drift that can control pitch and amplitude without obvious up/down repetition.

## File

`organic-lfo-drift-lab-v1.5.html`

Single standalone HTML file. No external libraries.

## Version

v1.5  
Date: 2026-06-02

## Main idea

The modulation source is a spring-mass-damper style stochastic process:

- `value` stays roughly within `-1..+1`
- `velocity` is internal state
- random force drives the velocity
- damping smooths the motion
- center force pulls the value back toward zero
- optional sine mix can be blended in for comparison
- amplitude can use a delayed sample of the same drift

This creates band-limited low-frequency noise rather than a strictly periodic sine LFO.

## Audio test

The lab uses:

- mono sine oscillator
- base frequency slider, default 60 Hz
- modulation target:
  - frequency
  - gain
  - both
- master volume

The oscillator is not restarted during parameter changes, avoiding phase-reset clicks.

Gain and frequency changes are smoothed through Web Audio parameter ramps / target updates.

## v1.5 changes

Added a user-facing macro parameter:

### Liveliness

`Liveliness` controls several internal modulation parameters together:

- drift speed
- motion depth
- pitch drift amount
- amp drift amount

The tech sliders act as maximum limits. The effective values are derived from the Liveliness macro and shown in the debug readout.

This gives a possible Elsi UI model:

```text
Scene = what sound
Volume = how much
Liveliness = how organic / alive
```

## Current recommended Elsi mapping

User UI:

- `Liveliness`

Tech UI:

- `Pitch drift max octaves`
- `Amp drift max dB`
- `Drift speed max`
- `Amp delay`
- optionally `Motion depth`

Internal / fixed initially:

- damping
- center force
- random force
- simulation Hz

## Suggested Elsi integration path

Do not integrate the whole lab UI.

Instead, extract the drift model into an internal module/function with an API like:

```js
organicDrift.update(dt);
const mainValue = organicDrift.getValue();
const delayedValue = organicDrift.getDelayedValue(ampDelaySec);
```

Then convert to audio parameters:

```js
pitchRatio = Math.pow(2, mainValue * effectivePitchDepthOct);
ampGain = Math.pow(10, delayedValue * effectiveAmpDepthDb / 20);
```

The existing Elsi app should receive the same type of high-level modulation outputs as before, so scene logic does not need to change.

## Test notes

Useful test settings:

- base frequency: 60 Hz, 90 Hz, 120 Hz
- pitch drift max: up to 0.5 oct for stress testing
- amp drift max: up to 6 dB for stress testing
- practical sleep-sound values will likely be smaller:
  - pitch drift: about 0.05–0.20 oct
  - amp drift: about 1–3 dB

## Open questions

- Whether variable amp delay is useful enough to keep
- Whether the LFO spectrum should be plotted in a later version
- Whether a fixed amp delay is enough for Elsi v1 integration
- How strongly Liveliness should scale pitch versus amplitude
