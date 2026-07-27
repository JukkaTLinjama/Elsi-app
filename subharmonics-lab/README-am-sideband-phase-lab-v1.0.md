# AM / Sideband Phase Lab v1.0

Single-file Web Audio research lab for comparing mono and stereo phase structures built from the same three sinusoidal components.

```text
lower   = fc − fm
carrier = fc
upper   = fc + fm
```

The main research condition is:

```text
same frequencies
same per-channel component amplitudes
different L/R phase relation
```

The lab is intended for audible and vibrotactile A/B testing. It does not add L/R detune, binaural beat offsets, stereo sway, automatic panning, crossfeed, HRTF processing, subharmonics, or actuator modelling.

## Current file

```text
am-sideband-phase-lab-v1.0-lr-fft.html
```

The app is self-contained and runs directly in a modern browser.

## Signal modes

The original mono component structures are retained:

- **Carrier only** — carrier component only.
- **Carrier + Lower** — carrier and lower sideband.
- **Pure AM** — carrier with equal symmetric sidebands.
- **Same Spectrum / Phase Shift** — same frequencies and raw amplitudes as Pure AM, with both sidebands rotated relative to the carrier.

Frequency relation modes are unchanged from v0.7:

- **Free**
- **Beat Lock**
- **Grid Lock**

The harmonic grid, grid offset, grid drift readouts, interactive spectrum handles, and normalization choices remain available.

## Stereo phase modes

Both channels always use the same frequencies and component levels. Only the right-channel phase offsets change.

### Dual Mono

```text
L = R
```

Expected result:

```text
correlation ≈ +1
Side ≈ 0
```

### Whole Signal Phase

The same phase offset is applied to every right-channel component:

```text
φR,k = φL,k + θ
```

At 180°:

```text
R ≈ −L
correlation ≈ −1
Mid ≈ 0
```

### Carrier Stereo Phase

Only the carrier phase differs between channels. Lower and upper sidebands remain aligned.

### Sideband Stereo Phase

The carrier remains aligned while sideband phases change.

Two structures are available:

```text
Linked:
lower R = lower L + θs
upper R = upper L + θs
```

```text
Mirrored:
lower R = lower L − θs
upper R = upper L + θs
```

Mirrored phase is an experimental structure, not an assumed optimum.

### Component Stereo Phase

Advanced mode with independent right-channel phase offsets for:

```text
lower
carrier
upper
```

## Normalization

The recommended default is **per-channel normalization**. It keeps the channel levels stable while allowing the Mid/Side and mono-sum structure to change naturally.

Optional stereo-sum normalization is available for comparison, but it can mask the phase-dependent mono-sum effect by changing channel gain.

Displayed measurements include:

```text
Left RMS
Right RMS
Left peak
Right peak
Mid RMS
Side RMS
mono sum peak
L/R correlation
base normalization gain
stereo normalization gain
total gain
```

Definitions:

```text
Mid  = (L + R) / 2
Side = (L − R) / 2
mono sum = L + R
```

The raw mono sum can rise by approximately 6 dB in Dual Mono and nearly cancel with a 180° whole-signal phase offset, even though the individual channel energies remain unchanged.

## Visualizations

### L/R waveform

Left and right channels are drawn in the same short time-domain view.

### Envelope and pulse history

The original analytic envelope and one-second pulse-history views are retained.

### Live FFT L/R overlay

The 500 ms Hann-window FFT shows:

- Left magnitude
- Right magnitude
- inter-channel phase difference `Δφ = φR − φL`
- logarithmic 10–160 Hz frequency axis
- harmonic grid markers

The L/R magnitude traces should overlap when the experiment is configured correctly. The phase-difference trace reveals the stereo structure even when both channel magnitude spectra are identical.

The phase trace is gated below approximately −70 dB. FFT phase at very low-energy bins is unstable and should not be interpreted as a meaningful component phase.

### Component spectrum

Shows the three explicit component frequencies with:

```text
magnitude L
magnitude R
phase L
phase R
inter-channel phase difference
```

The component markers are more reliable than the continuous FFT phase trace for reading the exact lower, carrier, and upper phase offsets.

### Instantaneous amplitude / frequency plane

The original mono analytic phase plane is retained:

```text
x = instantaneous amplitude
y = instantaneous frequency deviation from fc
```

### Stereo phase plane

```text
x = Left
y = Right
```

Typical interpretation:

```text
Dual Mono       → positive diagonal
90° phase       → ellipse or circular tendency
180° phase      → negative diagonal
multi-component → evolving loop or fan structure
```

### Component phase pointers

Three pointers show the explicit L/R phase differences for lower, carrier, and upper components.

## Audio architecture

- Audio is generated in one `AudioWorkletProcessor`.
- Left and right channels share the same sample clock and phase accumulators.
- Stereo phase is implemented with explicit component phases, not `DelayNode` timing offsets.
- Stereo-mode changes use a short click-safe crossfade of approximately 20 ms.
- Oscillator time is not reset during ordinary mode changes.
- Visual analysis receives the same generated L/R samples as the live output.

## WAV rendering

WAV export is stereo:

```text
2 channels
same live sample rate when available
same signal mode
same frequency relation
same L/R phase structure
same normalization
```

The rendered Mid and mono-sum behaviour should match the corresponding live measurements.

## Basic validation cases

### Dual Mono

```text
L = R
correlation ≈ +1
Side ≈ 0
L/R FFT magnitudes overlap
```

### Whole Signal Phase 180°

```text
R ≈ −L
correlation ≈ −1
Mid ≈ 0
Side strong
individual L/R RMS unchanged
```

### Whole Signal Phase 90°

```text
L/R magnitude spectra remain equal
inter-channel phase ≈ 90°
stereo phase plane becomes elliptical
```

### Carrier Stereo Phase

```text
only carrier Δφ changes
lower and upper Δφ remain at 0°
```

### Sideband Stereo Phase

```text
carrier Δφ remains at 0°
only sideband Δφ values change
```

## Suggested test procedure

1. Start with low amplifier and actuator gain.
2. Select **Pure AM** and **Dual Mono**.
3. Confirm correlation near +1 and Side RMS near zero.
4. Switch to **Whole Signal Phase** and test 90° and 180°.
5. Confirm that L/R FFT magnitudes still overlap.
6. Compare Mid RMS, Side RMS, mono-sum peak, correlation, and stereo phase plane.
7. Test **Carrier Stereo Phase** separately.
8. Test **Sideband Stereo Phase** using Linked and Mirrored structures.
9. Change only one phase parameter at a time during perceptual comparison.

## Research hypothesis

The implementation does not assume the hypothesis is true, but allows it to be tested:

> Equal per-channel amplitude spectra may produce different audible or vibrotactile sensations when inter-channel component phases alter Mid/Side structure, actuator interaction, mechanical summation, or body-coupled pressure and motion.

The app only controls the electrical signal. Mechanical behaviour depends on the actuator installation, coupling surface, listener position, amplifier, transducers, and room or body mechanics.

## Safety

Start with low output levels. Low-frequency signals can produce unexpectedly strong actuator movement even when they do not sound loud. Avoid clipping the audio interface, amplifier, or transducer.

## Version summary

### v0.7

Mono signal architecture, AM and sideband modes, Free / Beat Lock / Grid Lock, normalization, live FFT, waveform, envelope, pulse history, harmonic grid, interactive spectrum handles, and instantaneous amplitude/frequency phase plane.

### v0.8

Stereo AudioWorklet output, stereo phase modes, Mid/Side and correlation metrics, component phase pointers, stereo phase plane, and stereo WAV export.

### v0.9

Separate L/R waveform and component-spectrum visualization.

### v1.0

Live FFT upgraded to L/R magnitude overlay with an inter-channel phase-difference trace.
