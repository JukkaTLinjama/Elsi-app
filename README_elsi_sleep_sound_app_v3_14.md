# Elsi Sleep Sound App — v3.14 session notes

Version: `elsi-sleep-sound-app-v3.14`
Date: 2026-06-05

## Summary

This session integrated the Organic LFO Drift Lab engine into the Elsi Sleep Sound App and cleaned up the technical UI around the new modulation model.

The main goal was to replace the old random ramp LFO with the Organic Drift engine while keeping the existing Elsi scene, loop family, voice, noise bed and crossfade architecture intact.

## Main changes

### Organic drift modulation

- Integrated `createOrganicDriftLfo()` from Organic LFO Drift Lab v2.1.
- Replaced the old random ramp load LFO with a stochastic spring-mass-damper drift process.
- Kept the control update rate fixed at 10 Hz.
- Removed lab-only sine mixing from the production drift path.
- Removed old random-ramp helper code after the organic drift path was stable.

The production modulation path is now:

```text
random excitation → spring-mass-damper drift → pitch/load modulation + delayed amplitude drift
```

### Organic LFO controls

Added technical controls for:

- Liveliness
- Load drift depth
- Pitch drift max oct
- Amp drift max dB
- Amp drift delay
- Natural frequency Hz
- Damping ratio ζ
- Random excitation

The Liveliness macro scales pitch depth, amplitude depth, natural frequency and random excitation.

### Scene / volume behavior

- Fixed scene-volume behavior so volume changes do not reload scene presets unnecessarily.
- Kept runtime tech-slider edits active while staying in the same scene.
- Updated the volume indicator so scene changes immediately sync the visual volume arcs to the new scene volume.
- Fixed the earlier issue where scene changes left white transition arcs visible even when volume was already at the new target.
- Master volume remains continuous, while the user-facing volume arcs display the nearest volume step.

### Preset behavior

- Reworked save/reset actions toward the current active scene model.
- Replaced scene-specific save buttons with clearer current-scene actions:
  - Save current preset
  - Reset current preset
  - Reset all scene defaults

### Technical UI cleanup

- Added a `Scene balance` area for basic tuning.
- Moved advanced/debug/actions into Advanced UI areas.
- Renamed `Loop level` to `Tonal loop level`.
- Renamed `Loop basic` to `Tonal loop basic`.
- Moved Mono on/off into the advanced action area.
- Fixed advanced show/hide behavior after UI restructuring.

### Noise bed balance

- Raised Air wash, Body rumble and texture layer audibility.
- Slightly reduced Masking noise dominance relative to the other noise bed layers.
- Kept the exact values subject to listening tests.

## Versions produced in this session

- v3.5 — Organic drift integration baseline
- v3.6 — Removed sine mix from organic drift
- v3.7 — Organic drift cleanup and `Load drift depth` naming
- v3.8 — UI naming and slider order cleanup
- v3.9 — Scene balance and Advanced split
- v3.10 — Master volume in Scene balance and volume arc sync model
- v3.11 — Noise balance and Advanced toggle fix
- v3.12 — Advanced UI cleanup
- v3.13 — Loop control binding fix after UI restructuring
- v3.14 — Volume indicator scene sync fix

## Current stable file

`elsi-sleep-sound-app-v3-14-volume-indicator-scene-sync.html`

## Known follow-up items

- Listen-test the updated noise bed balance.
- Continue UI grouping later; current advanced slider grouping is functional but not final.
- Consider renaming internal `loadLfo` debug/API structures to `organicDrift` later, but only after the sound and UI behavior are stable.
- Avoid further architecture changes until v3.14 behavior has been tested in browser.

## Testing checklist

1. Start app.
2. Change `Liveliness` and verify modulation changes smoothly.
3. Change `Load / pitch pull` and verify tonal loop pitch/load responds.
4. Change volume in the main UI and verify tech settings are not reset.
5. Change scene and verify volume arcs sync to the new scene without stale white transition arcs.
6. Toggle Advanced open/closed.
7. Test `Save current preset`, `Reset current preset`, and `Reset all scene defaults`.
8. Listen to Air wash, Masking noise, Body rumble and texture layers at high values.

## Commit message

```text
Integrate organic drift LFO and clean Elsi tech controls

- Replace random ramp load LFO with Organic Drift spring-mass-damper engine
- Keep fixed 10 Hz control-rate for stable modulation
- Add liveliness, pitch depth, amp depth, amp delay, natural frequency, damping and excitation controls
- Remove lab sine-mix and old random-ramp leftovers from production drift path
- Preserve existing scene, loop family, voice, noise bed and crossfade architecture
- Split tech UI into basic Scene balance and Advanced areas
- Move master volume into Scene balance while keeping continuous audio gain
- Sync volume arcs to nearest displayed step without quantizing audio gain
- Fix scene volume changes so UI indicator follows current scene preset cleanly
- Rework preset actions around current active scene save/reset behavior
- Improve noise bed balance and advanced UI behavior
- Fix loop control binding after UI restructuring
```
