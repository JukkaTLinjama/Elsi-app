# Elsi Sleep Sound App v2.12

## Overview

Elsi is a browser-based sleep sound application designed for infant sleep testing. The architecture combines a tonal loop source with a shared noise bed.

## Audio Architecture

### Loop Source

One active tonal source:

- Vacuum
- Hum
- Vibro

### Noise Bed

Shared masking layer:

- Air wash
- Soft texture low
- Soft texture high
- Masking noise
- Body rumble
- Stereo width
- Texture motion

## Scene Families

### Vacuum

- vacuumSoft
- vacuum
- vacuumFull

### Hum

- humSoft
- hum
- humFull

### Vibro

- vibroSoft
- vibro
- vibroFull

## Scene Metadata

Each scene is described by:

- key
- family
- intensity

## Vibro Family

Version 2.12 introduces Vibro as a third canonical loop-source family.

Current source:

assets/vibro.wav

Fallback:

Synthetic Vibro loop generator.

## Future Direction

Phase 1:
- Single Vibro loop

Phase 2:
- Multiple Vibro loops mixed internally

Phase 3:
- Vibro chords and harmonic structures

## Version History

### v2.9
- Gradient scene-family controls
- Vacuum and Hum families

### v2.10
- Canonical scene architecture

### v2.11
- Hidden Vibro scene definitions

### v2.12
- Vibro family added
- Dedicated Vibro loop source
- Vacuum / Hum / Vibro architecture
