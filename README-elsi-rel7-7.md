# Sleep Elsi

Sleep Elsi is an experimental web audio app for creating calm, slowly
evolving sound environments.

It explores layered sound scenes built from tonal textures, soft noise
beds and organic modulation. The goal is not to play repeating tracks,
but to create a living sound environment that gently changes over time.

## Sound philosophy

Many artificial loops become tiring because the human ear quickly
recognizes repetition.

Sleep Elsi is built around the idea that small natural imperfections
make sounds feel more alive. Real environments are never perfectly
static: motors breathe, air moves, resonances drift and tiny variations
appear continuously.

The engine tries to recreate this feeling using controlled randomness
instead of simple looping.

## Sound families

### Vacuum

Soft broadband textures inspired by familiar steady background sounds.

### Hum

Warm tonal layers with harmonic movement, slow modulation and evolving
depth.

### Vibro

Deep low-frequency focused textures exploring gentle body-level sound
sensations.

## Organic sound engine

Sleep Elsi uses a custom real-time browser audio engine.

The engine combines:

-   multiple tonal and noise layers
-   independent modulation sources
-   organic pitch movement
-   amplitude breathing
-   stereo movement
-   harmonic and subharmonic layers
-   smooth scene transitions

A significant part of the development has focused on avoiding mechanical
repetition.

Instead of using fixed LFO cycles only, Sleep Elsi uses an organic drift
model inspired by physical systems: - inertia - damping - natural
frequency - random excitation

The result behaves more like a slowly moving object than a repeating
oscillator.

## Parameter architecture

Behind the simple interface is a larger sound engine with many
interacting parameters.

The engine controls: - tonal/noise balance - harmonic content -
subharmonic voices - modulation depth - modulation speed - spatial
movement - texture density - scene morphing - transition behavior

The user interface exposes only the most important feeling controls
while keeping deeper engine parameters available for exploration.

## Design philosophy

The goal is:

Simple outside.\
Living system inside.

Normal use: - choose a scene - adjust the feeling if desired - let the
sound evolve

Advanced controls reveal the engine for experimentation.

## Technology

-   Web Audio based synthesis and processing
-   Runs locally in the browser
-   No server processing required
-   Mobile friendly
-   Progressive web app concept

## Status

Early experimental release.

The app is functional and usable, but the sound engine continues to
evolve.

## Author

Created by Jukka Linjama

© 2026 Jukka Linjama
