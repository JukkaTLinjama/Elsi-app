# Subharmonic Harmony Lab v0.2

## Purpose

Subharmonic Harmony Lab is a research application for exploring
low-frequency harmonic relationships and rhythm-locked beat phenomena
before integration into Sleep Elsi.

## Features

-   Virtual keyboard
-   Pure sine synthesis
-   L/R beat and phase modes
-   Glide
-   Loop recording and playback
-   Offline WAV rendering (base engine)
-   Independent Subharmonic Engine

### Subharmonic Engine

Supported ratios:

-   1/2
-   2/3
-   3/4
-   2/5
-   3/5
-   4/5
-   3/7
-   4/7
-   5/8

Modes:

-   Pure Harmonic
-   Beat Tuned Harmonic

Beat tuned equation:

subFreq = (m × baseFreq ± beatTargetHz) / n

Minimum playable subharmonic frequency is 10 Hz.

Auto Ratio continuously searches for the most suitable playable ratio.

## Beat Pair

Version 0.2 introduces a dedicated harmonic beat pair.

Instead of generating many harmonics, only the harmonics responsible for
the desired rhythmic interference are emphasized:

-   m × base
-   n × sub

This makes the intended beat significantly easier to perceive while
minimizing unwanted interference.

## Roadmap

v0.3 - Organic Drift for beat target

v0.4 - A/B subharmonic morph

Future - Integration into Sleep Elsi

------------------------------------------------------------------------

Author: Jukka Linjama 2026
