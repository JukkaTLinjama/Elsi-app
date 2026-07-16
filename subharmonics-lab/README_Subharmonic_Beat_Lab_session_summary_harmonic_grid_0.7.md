# Subharmonic Beat Lab — Session Summary

Author: Jukka Linjama, 2026

## Current lab version

Current working file: `subharmonic-beat-lab-v0.7d-absolute-lissajous.html`

The current lab has been simplified back to first principles:

- keyboard input
- base oscillator pair
- sub oscillator pair derived from a ratio
- stereo Δf split
- loop recorder and loop playback
- WAV export
- live Lissajous visualization

Removed from the earlier experimental branch:

- Auto Ratio
- Beat Pair Engine
- Beat Target
- Beat Pair Level
- Beat Direction
- explicit `m × base` / `n × sub` harmonic beat oscillators

The reason for this simplification was that added harmonic beat-pair oscillators easily became too tonal and made it difficult to identify the actual source of the pleasant tactile beat.

---

# Main conceptual shift

The session moved away from the question:

> How do we synthesize a beat?

Toward the deeper question:

> What shared temporal grid makes a set of frequencies feel coherent?

This led to the working concept of a **Harmonic Grid**.

---

# Harmonic Grid idea

A set of frequencies may be understood as selected integer points on a hidden temporal grid:

```text
grid = F0
frequencies = indices × grid
```

Example:

```text
grid = 20 Hz
sub  = 2 × grid = 40 Hz
base = 3 × grid = 60 Hz
upper = 4 × grid = 80 Hz
```

This gives the index set:

```text
[2, 3, 4]
```

The grid is not necessarily physically present as a sounding frequency. It can be an inferred common period, similar to the missing fundamental / virtual pitch idea in hearing.

Working formulation:

```text
Harmony = shared periodicity
```

Or more carefully:

```text
Perceived harmony may reflect how strongly a frequency set supports a stable shared temporal grid.
```

---

# Relation to rhythm

The same harmonic grid can be interpreted differently at different time scales.

Example:

```text
grid = 20 Hz  → vibrotactile texture / low harmonic field
grid = 4 Hz   → pulse / rhythm
grid = 0.5 Hz → gesture / phrase
```

This led to the working phrase:

```text
A chord may be a fast rhythm.
A rhythm may be a slow chord.
```

More cautiously:

```text
The integer relationship can remain the same while perception changes with time scale.
```

---

# Common repetition rate

The common repetition rate is the grid frequency that explains a set of frequencies as integer multiples.

Example:

```text
40 Hz and 60 Hz
40 = 2 × 20
60 = 3 × 20
common repetition rate = 20 Hz
```

A perfectly locked grid can feel stable. A slightly detuned grid can create a slow wandering or living tactile field.

Observed interpretation:

```text
1–4 Hz   → wandering / slow motion
5–20 Hz  → vibration / pulse
>20 Hz   → more continuous tone / texture
```

---

# Base + sub before beat-pair synthesis

A key decision was to return to the simplest signal model:

```text
base + sub
```

rather than:

```text
m × base + n × sub
```

Reason: explicit harmonic beat-pair oscillators can create audible tonal layers in the 50–150 Hz range, which may distract from the tactile field.

The current hypothesis is that the desired living effect may already emerge from:

```text
base
sub = ratio × base
small stereo Δf
mechanical coupling in transducers / body / furniture
```

---

# Stereo Δf model

The current v0.7 line uses a whole-stack stereo split:

```text
baseL = baseCenter − Δf/2
baseR = baseCenter + Δf/2
subL  = ratio × baseL
subR  = ratio × baseR
```

Example:

```text
baseCenter = 60 Hz
stereo Δf = 2 Hz
ratio = 2/3

Left:
base = 59 Hz
sub  = 39.33 Hz

Right:
base = 61 Hz
sub  = 40.67 Hz
```

This preserves the ratio inside each channel while creating a slow stereo field movement between transducers.

---

# Lissajous visualization

The lab now contains three Lissajous scopes:

```text
Base L/R
Sub L/R
Sum L/R
```

These show:

```text
x = left signal
y = right signal
```

The Sum view is closest to what the two-transducer field may feel like:

```text
left  = baseLeft + subLeft
right = baseRight + subRight
```

Important implementation decision:

- the scopes are generated from the engine’s own oscillator model
- they do not analyze microphone input or Web Audio output
- this makes them a visualization of the intended harmonic field, not a measurement of the physical system

v0.7d changed the Lissajous visualizer to:

- draw only while the audio gate is open
- let old points fade after note release
- update display around 30 fps
- sample the Lissajous points at an absolute fixed visual sample rate, currently 3000 Hz
- keep the visual sampling independent of oscillator frequency

This is closer to an oscilloscope and preserves the physical time scale.

---

# Upper candidate idea

A possible next experiment is to add a quiet or visualization-only upper frequency derived from the harmonic grid.

If:

```text
sub/base = a/b
sub  = a × grid
base = b × grid
```

then the next-grid upper is:

```text
upper = (b + 1) × grid
upper = base + grid
```

Example for ratio 2/3:

```text
sub  = 2 × grid = 40 Hz
base = 3 × grid = 60 Hz
upper = 4 × grid = 80 Hz
```

This produces the compact index set:

```text
[2, 3, 4]
```

Alternative candidate:

```text
upper = base + sub
```

For the same example:

```text
upper = 60 + 40 = 100 Hz
set = [2, 3, 5]
```

Interpretation:

```text
base + grid → compact / stable / less bright
base + sub  → wider / more open / more colorful
```

The suggested first test is `upper = base + grid`, probably at a very low level or visual-only.

---

# Theoretical framing

This session connected the lab to several known ideas:

- missing fundamental
- virtual pitch
- harmonicity
- periodicity detection
- tonal fusion
- neural phase locking
- Sethares-style tuning/timbre/consonance thinking
- rhythm transform / periodic grid thinking

The working hypothesis became:

```text
Harmony is not one frequency.
Harmony is a multi-scale synchronization affordance.
```

In Finnish:

```text
Harmonia ei ole yksi taajuus.
Harmonia on moniaikaskaalainen synkronoitumisen mahdollisuus.
```

This should be treated as a research hypothesis, not as a proven theory.

---

# Suggested next experiment

After testing v0.7d, the next clean experiment could be:

```text
v0.8
Add optional Upper = base + grid
Add Upper Lissajous or add upper into Sum view
Keep everything else unchanged
```

Do not yet add:

- Auto Ratio
- Beat Pair Engine
- complex drift engine
- multiple upper algorithms
- FFT / spectrum analysis

The purpose is to test one question only:

> Does adding the next harmonic-grid point increase tactile coherence, or does it create too much audible tonality?

---

# Current caution

The project should avoid adding too many mechanisms at once.

The main experimental discipline is:

```text
change one mechanism
listen / feel
observe Lissajous
then decide
```

The current most important distinction is:

```text
Harmonic Grid Engine ≠ Stereo Renderer ≠ Physical transducer/body response
```

These must be kept separate so experiments remain interpretable.
