# Elsi Sleep Sound App v1.6

Browser-based sleep sound prototype built around a simple architecture:

```text
Loop Source + Noise Bed = Sleep Scene
```

The current goal is calm sleep/soothing sound testing with minimal UI and a stable internal structure for future sound scenes.

## Version

`v1.6`

## Current user UI

```text
Start / Stop

Volume
[ − ] 0 dB [ + ]

Sound
[ Vacuum ]      [ Hum ]
[ Vacuum soft ] [ Hum soft ]

Tech
```

## Current architecture

### Loop Source

The Loop Source is the single tonal or pitch-focused foreground layer.

Current loop-source scene types:

- `vacuumFull`
- `vacuumSoft`
- `hum`
- `humSoft`

Only one loop source should be emotionally dominant at a time. This avoids beating, phase coloration, and the “two machines” illusion caused by multiple simultaneous tonal layers.

If external assets are missing, the app can use a browser-generated vibro-style fallback loop for testing. This makes the app playable directly in the browser without requiring a WAV file.

### Noise Bed

The Noise Bed is the shared non-tonal support layer underneath the Loop Source.

Current Noise Bed components:

- Air wash
- Soft texture low
- Soft texture high
- Masking noise
- Body rumble 30–60 Hz
- Noise width
- Texture motion

The Noise Bed provides masking, body, width, and continuity while the Loop Source provides the main tonal identity.

## Scenes

v1.6 renamed the older legacy scene keys:

```text
near → vacuumFull
far  → vacuumSoft
```

Current scene keys:

```text
vacuumFull
vacuumSoft
hum
humSoft
```

A compatibility helper keeps old `near` / `far` aliases working internally:

```js
normalizeSceneKey()
```

## Technical UI

The Tech panel is grouped by architecture:

```text
Loop Source
Noise Bed
System / preset actions
Debug
```

Visible labels have moved away from old `motor` naming toward `loop` naming. Some internal legacy control keys are still preserved to avoid breaking working audio behavior.

## Important design decisions

- The loop source is mono by default for stable tonal imaging.
- Stereo width mainly belongs to the Noise Bed.
- Loop Pan now affects all current loop sources.
- Mono check is a button.
- Loop crossfade processing updates immediately when the slider changes.
- Mode transitions are deliberately slow for sleep use.

Current transition direction:

```text
live control ramp: 0.35 s
mode transition: 5.0 s
loop source crossfade: 5.0 s
volume transition: 3.0 s
```

## Asset behavior

The app can use external assets when available:

```text
assets/vacuum-loop.wav
assets/hum.wav
```

For fast browser testing, it can fall back to a generated Vibro loop source when assets are missing.

This avoids requiring base64 data inside the HTML during development.

## Why base64 is avoided during development

Large base64 audio strings make the HTML heavy and make ChatGPT/file editing workflows inefficient.

Recommended workflow:

```text
Development/testing:
  Use assets/*.wav or generated fallback loop

Portable single-file demo:
  Add embedded base64 only at the final sharing stage
```

## Commit message

```text
Refactor scene keys toward Loop Source + Noise Bed architecture

- rename legacy scene keys near/far to vacuumFull/vacuumSoft
- keep Hum and Hum soft scenes active for testing
- add normalizeSceneKey() for backward-compatible near/far aliases
- update Tech edit/save labels to match scene names
- preserve working audio routing and loop/noise behavior
- prepare for future Vibro scenes without changing current sound engine
```

## Suggested next step

A cautious v1.7 refactor could continue internal naming cleanup:

```text
motorSource          → vacuumLoopSource
humSource            → humLoopSource
motorGain            → vacuumLoopGain
humGain              → humLoopGain
motorPanner          → vacuumLoopPanner
humPanner            → humLoopPanner
processedMotorBuffer → processedVacuumLoopBuffer
processedHumBuffer   → processedHumLoopBuffer
```

Recommended approach:

1. Keep legacy API aliases.
2. Rename only internal node/buffer names.
3. Do not change DSP behavior.
4. Keep testing after each small rename group.
