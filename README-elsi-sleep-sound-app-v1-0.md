# Elsi Sleep Sound App v1.0

Browser-based sleep sound prototype built around a simple audio architecture:

```text
Loop Source + Noise Bed = Sleep Scene
```

The app is designed for testing calm active backgrounds for sleep and soothing. It runs directly in the browser and can generate its own fallback loop if external audio assets are missing.

## Current milestone

Version: `elsi-sleep-sound-app-v1.0`

Main goal of this milestone:

- move away from a vacuum-only engine toward a more general sleep sound engine
- use one tonal/loop source as the emotional anchor
- keep a shared non-tonal noise bed underneath the selected scene
- support browser-only testing without requiring external assets or base64 embedding

## Audio architecture

### Loop Source

The Loop Source is the main tonal or quasi-tonal foreground layer.

Current options and test modes:

- Vacuum
- Vacuum soft
- Hum
- Hum soft

For testing, all modes can fall back to a browser-generated Vibro loop source if external assets are missing.

The generated fallback is intentionally simple:

- 2 second loop
- low-frequency sine-based body tone
- light harmonics
- soft saturation
- phase-aligned modulation for seamless looping

This makes the app usable without `assets/` files and without embedding a large base64 WAV into the HTML.

### Noise Bed

The Noise Bed is the shared non-tonal support layer underneath all scenes.

Current noise components:

- Air wash
- Soft texture low
- Soft texture high
- Masking noise
- Body rumble 30–60 Hz
- Noise width
- Texture motion

The Noise Bed creates the environment, masking, softness, and body feel around the Loop Source.

## User UI

The visible end-user UI is intentionally minimal:

```text
Start / Stop
Volume
Sound
  Vacuum
  Hum
  Vacuum soft
  Hum soft
Tech
```

The UI avoids technical DSP terms for normal use. Technical controls are hidden behind the Tech button.

## Technical UI

The technical controls are grouped into:

### Tonal bed

- Load / pitch pull
- Slow load motion
- Vacuum tone
- Tonal loop crossfade
- Tonal pan
- Tonal drift

### Noise bed

- Air wash
- Soft texture low
- Soft texture high
- Masking noise
- Body rumble 30–60 Hz
- Noise width
- Texture motion
- Mono check
- Master volume

Mono check is now a button instead of a slider.

Tech action buttons now show a visible press flash when clicked.

## Asset handling

The app first tries to load external assets:

```text
assets/vacuum-loop.wav
assets/hum.wav
```

If assets are missing, the app generates a synthetic Vibro loop source in the browser.

This is the preferred test workflow for this milestone because it avoids repeated base64 editing and keeps the HTML small enough for iteration.

## Why base64 is not included by default

Large embedded base64 audio strings make the HTML file much larger and also make ChatGPT iteration harder because they consume a lot of context tokens.

Recommended workflow:

```text
Development/testing:
  use browser-generated fallback or assets/*.wav

Portable single-file demo:
  add embedded base64 only at the end if needed
```

## Important design principle

Only one tonal/loop source should be active as the main emotional anchor at a time.

Multiple simultaneous tonal layers can create:

- beating
- phase coloration
- two-source illusion
- unstable emotional focus

The Noise Bed can remain underneath all scenes because it is non-tonal and functions as masking/support texture.

## Status

This is a working v1.0 test milestone, not a final product.

The generated Vibro loop works as both:

- a fallback test signal
- a possible future vibroacoustic soothing mode

The next likely development direction is to improve the scene model so each visible sound button maps clearly to:

```text
Loop Source preset + Noise Bed preset + Strength
```

## Suggested commit message

```text
Release Elsi Sleep Sound App v1.0 loop-source/noise-bed milestone

- Rename milestone around Loop Source + Noise Bed architecture
- Add browser-generated Vibro loop fallback for asset-free testing
- Keep external WAV assets optional
- Group technical controls into Tonal bed and Noise bed sections
- Convert Mono check from slider to button
- Add visible press feedback for technical action buttons
- Preserve minimal end-user UI with Vacuum/Hum and soft variants
```
