# AM / Sideband Phase Lab v0.4

A standalone Web Audio research lab for comparing low-frequency carrier and sideband structures while keeping frequency components and spectral amplitudes controlled.

## Research question

When the component frequencies and levels are held constant, how do the number of sidebands and their relative phases change the audible and vibrotactile perception?

The core comparison is:

```text
same frequencies
same spectral amplitudes
different phase relation
```

This lab is separate from Subharmonic Beat Lab. It intentionally excludes stereo detune, sway, subharmonics, harmonic-grid selection, automatic carrier choice, and multiple modulators.

## Signal components

```text
lower   = fc − fm
carrier = fc
upper   = fc + fm
```

Where:

- `fc` is the carrier frequency, adjustable from 20 to 80 Hz.
- `fm` is the modulation or difference frequency, adjustable from 1 to 30 Hz.

All output is mono duplicated to both output channels.

## Signal modes

### 1. Carrier only

```text
x(t) = Ac cos(2πfc t + φc)
```

Only the carrier component is active.

### 2. Carrier + Lower

```text
x(t) =
Ac cos(2πfc t + φc)
+
As cos(2π(fc − fm)t + φl)
```

This contains two spectral components and corresponds to the basic Tactile Difference structure.

It is not pure amplitude modulation.

### 3. Pure AM

```text
x(t) = A[1 + m cos(2πfm t)] cos(2πfc t)
```

Expanded into three sinusoidal components:

```text
x(t) =
A cos(2πfc t)
+
Am/2 cos(2π(fc − fm)t)
+
Am/2 cos(2π(fc + fm)t)
```

The lower and upper sidebands have equal amplitude and symmetric phase relative to the carrier.

### 4. Same Spectrum / Phase Shift

This mode uses the same three frequencies and the same component amplitudes as Pure AM, but changes the relative sideband phase.

The amplitude spectrum therefore remains unchanged before normalization, while the time-domain waveform and envelope may change substantially.

This mode is not labelled as pure FM because three fixed sinusoidal components do not generally form an exact FM signal.

## Phase model

The component phases are explicit:

```text
carrier phase = φc
lower phase   = φl
upper phase   = φu
```

The readout shows:

- carrier phase
- lower phase
- upper phase
- lower minus carrier phase
- upper minus carrier phase
- upper minus lower phase

The live FFT phase display is carrier-referenced. The complex FFT spectrum is rotated by the measured carrier phase:

```text
Xref(f) = X(f) · exp(−jφc)
```

This forces the carrier reference to 0° while preserving relative FFT phase relationships.

## Level controls

The lab provides:

- Carrier Level
- Sideband Level
- Master Volume
- Gate / Play

Levels begin conservatively. The normalization stage includes a peak-safety limit to prevent clipping.

## Normalization modes

### Equal RMS

Applies gain so the active mode matches a common RMS target.

### Equal peak

Applies gain so the active mode matches a common peak target.

### None

Uses the raw component amplitudes without comparison normalization.

The readout shows:

- raw RMS
- normalized RMS
- normalized peak
- normalization gain
- peak-safety status

Pure AM and Same Spectrum / Phase Shift use the same component amplitudes before normalization.

## Visualizations

### Waveform

Shows approximately 500 ms of the output waveform. This is long enough to display the carrier oscillation and several modulation cycles at common test frequencies.

### Envelope

Shows the instantaneous analytic-envelope magnitude derived from the active sinusoidal components.

Phase-shifted sidebands can produce an envelope shape that differs significantly from pure sinusoidal AM.

### One-second pulse history

Shows the most recent one second of envelope data captured from the live AudioWorklet signal path.

When the gate is closed, the completed history remains visible instead of being regenerated from current control settings.

### Component Spectrum

Shows the exact analytical frequencies, amplitudes, and phases of the lower sideband, carrier, and upper sideband.

The component markers are interactive:

```text
drag carrier horizontally  → change fc
drag sideband horizontally → change symmetric fm
drag carrier vertically    → change carrier level
drag sideband vertically   → change shared sideband level
```

Dragging either sideband horizontally keeps the sidebands symmetric around the carrier.

### Live FFT

The live FFT uses:

```text
analysis duration: 500 ms
window: Hann
frequency axis: logarithmic
amplitude range: 0 to −100 dB
nominal resolution: approximately 2 Hz
```

The FFT is calculated from the generated output signal.

Exact theoretical component frequencies are overlaid as markers because components closer than approximately 2 Hz cannot be fully resolved by a 500 ms analysis window.

A dim carrier-referenced phase trace is drawn in a narrow strip near the bottom of the FFT display. The phase trace is intentionally allowed to move freely in low-magnitude bins; it is primarily useful at the three active component frequencies.

## A/B comparison

The four mode buttons allow rapid switching while preserving:

- carrier frequency
- modulation frequency
- carrier level
- sideband level
- normalization mode

Mode changes use a short click-safe crossfade. Long transitions are intentionally avoided because they make tactile comparison less precise.

## Suggested listening and tactile test

1. Start with low master volume.
2. Use `fc = 50 Hz` and `fm = 10 Hz`.
3. Compare Carrier only and Carrier + Lower.
4. Compare Pure AM and Same Spectrum / Phase Shift using equal RMS.
5. Adjust Sideband Phase slowly while monitoring:
   - pulse strength
   - pulse sharpness
   - apparent smoothness
   - audible roughness
   - tactile versus audible differences
6. Repeat at `fm = 5, 10, 20, and 30 Hz`.
7. Repeat using equal peak to check whether an apparent difference is mainly caused by peak level.
8. Use the component-spectrum handles for rapid frequency and level exploration.

Avoid assuming that equal RMS, equal peak, or equal amplitude spectrum implies equal perceived intensity.

## Known limitations

- A 500 ms FFT has approximately 2 Hz true frequency resolution. Zero-padding improves display smoothness but does not improve physical resolution.
- FFT phase is highly sensitive in bins with very low magnitude.
- The phase trace is carrier-referenced, not absolute phase relative to an external clock.
- Browser audio hardware, operating-system processing, amplifiers, and tactile actuators may alter amplitude and phase.
- The displayed analytical envelope describes the generated signal, not the mechanical envelope after the actuator and body response.
- Mono duplication does not test binaural, stereo-beat, or spatial phase effects.
- The lab does not yet provide harmonic-grid locking.
- The signal generator is intended for controlled experiments, not calibrated hearing or vibration measurement.

## Safety

Low-frequency signals can drive amplifiers and tactile actuators strongly even when they do not sound loud.

Use conservative volume, avoid sustained high levels, and check that the audio chain is not clipping or overheating.

## Files

```text
am-sideband-phase-lab-v0.4.html
README-am-sideband-phase-lab-v0.4.md
```
