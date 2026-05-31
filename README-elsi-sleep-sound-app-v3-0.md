# Elsi Sleep Sound App v3.0 — Modular Load LFO

## Summary

This version cleans up the internal modulation architecture while preserving the current UI and sleep-sound behavior.

The main change is that the load-motion LFO is now treated as a dedicated modulator under:

```js
state.modulators.loadLfo
```

This makes the pitch/load movement easier to inspect, tune, and extend later for harmonic voice drift, pan drift, and future loop families.

## Main changes

- Updated app version to `v3.0`.
- Moved the load-motion LFO state under `state.modulators.loadLfo`.
- Kept a legacy `loadLfo` debug alias so older console inspection remains readable.
- Preserved the dual random-ramp LFO model:
  - `ampScale` changes the load-motion width.
  - `currentPeriodSec` changes the load-motion speed.
  - Both use independent random ramps, so the movement does not repeat as a fixed sine pattern.
- Updated technical slider labels:
  - `Harmonic load motion` → `Harmonic pitch drift`
  - `Loop drift` → `Loop pan drift`
- Kept UI layout unchanged.
- Kept audio behavior intentionally close to v2.18, except for the internal LFO state organization.

## Current loop voice model

Loop families are configured through:

```js
LOOP_FAMILY_VOICES
```

Each loop family has its own visible voice list:

- `mainLoop`
- `humLoop`
- `vibroLoop`

Vibro voices can be edited manually for harmonic experiments. The voice model supports semitone-based pitch ratios and per-voice gain.

Example:

```js
Object.freeze({ semitone: 12, gain: 0.12, harmonic: true })
```

## Load LFO model

The load-motion modulator now follows this conceptual structure:

```js
state.modulators.loadLfo = {
  phase,
  value,
  motionOnly,
  effectiveLoad,
  amp,
  period
}
```

The audio mapping is conceptually:

```js
rawLfo = Math.sin(phase * Math.PI * 2)
value = ampScale * rawLfo
motionOnly = loadLfoDepth01 * value
effectiveLoad = loadSigned01 + motionOnly
```

The period is integrated over time:

```js
phase += dt / currentPeriodSec
```

This avoids phase jumps when the LFO speed changes.

## Why this matters

The old fixed-width LFO made the sound feel too repetitive. The new model separates two fundamental sources of liveliness:

1. LFO speed variation
2. LFO width variation

Keeping these as independent processes should make the sound less mechanical without requiring a heavy wavelet or 3D projection modulation system.

## Testing notes

Suggested listening checks:

1. Start with `Vacuum soft`.
2. Increase `Slow load motion` gradually.
3. Switch to `Vibro`.
4. Edit `vibroLoop` harmonics manually by raising selected `gain` values.
5. Use `Harmonic pitch drift` carefully; it should remain subtle.

If the sound becomes restless, reduce:

```js
loadLfoDepth01
```

or lower harmonic gains.

## Known limitations

- The app is still a single-file HTML prototype.
- Some internal names still retain legacy `motor*` naming for compatibility.
- `getState()` includes legacy fields for older console tests.
- Harmonic voice tuning is still manual in code.
- No preset export/import flow is included in this version.

## Recommended next steps

Possible v3.1 directions:

1. Move more modulator state under `state.modulators`.
2. Add per-voice harmonic drift defaults.
3. Clean legacy `motor*` naming gradually.
4. Add a debug-only voice summary to `getState()`.
5. Keep UI stable until the sound model is more settled.

## Commit message

```text
Refactor load LFO into modular modulator state

- Move load-motion LFO under state.modulators.loadLfo
- Preserve legacy loadLfo debug alias for console inspection
- Keep dual random-ramp amp/period LFO behavior from v2.18
- Rename technical sliders for clearer audio meaning
- Update app version to v3.0
```
