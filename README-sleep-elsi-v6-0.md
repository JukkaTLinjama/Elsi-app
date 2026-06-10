# Sleep Elsi v6.0

Sleep Elsi is a single-file browser app for calm layered sleep and relaxation sound. Version 6.0 is the first tuned sound-set milestone after the v5.x UI, transport, modulation and scene-control work.

## v6.0 milestone

- Updated the default scene presets from the latest tuned debug export.
- Preset set now contains 9 active scenes:
  - Vacuum Soft / Vacuum / Vacuum Full
  - Hum Soft / Hum / Hum Full
  - Vibro Soft / Vibro / Vibro Full
- The old hidden `rumble` scene remains removed.
- Main user-facing controls are consolidated into the Scene balance section:
  - Tonal / Noise Bed balance
  - Liveliness
  - Subharmonic level
  - Body rumble
- Advanced keeps technical output, loop source and noise bed controls.

## Sound architecture

The app still uses the same core architecture:

- one active tonal loop family at a time: Vacuum, Hum or Vibro
- shared Noise Bed underneath all scenes
- loop-family voices for base loop, harmonic beat and Subharmonic A/B voices
- independent Organic Drift for Subharmonic A/B morph movement
- separate Organic Drift for pitch/load/amp liveliness
- reversible transport state machine for calm start/stop behavior

## Important stability notes

The v6.0 change is primarily a tuned preset milestone. It does not intentionally refactor the audio routing, loop-family voice engine, Organic Drift model or reversible transport state machine.

Keep future changes small and test each scene after changes, especially Vibro Full and Hum Full where subharmonic and harmonic beat levels are more active.

## Assets

Expected optional asset paths:

- `assets/vacuum-loop.wav`
- `assets/hum.wav`
- `assets/vibro.wav`

If assets are missing, the app can use generated fallback loop material for testing.

## Public API

The browser console API remains available as:

```js
elsiSleepSoundApi.getState()
elsiSleepSoundApi.start()
elsiSleepSoundApi.stop()
elsiSleepSoundApi.updateControl({ ... })
elsiSleepSoundApi.copyCurrentScenePreset()
```

Legacy API name remains available for older tests:

```js
vacuumSampleLabApi
```

## Version

Sleep Elsi v6.0 · tuned sleep sound set  
JukkaTLinjama 2026
