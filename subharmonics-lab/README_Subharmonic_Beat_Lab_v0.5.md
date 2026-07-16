# Subharmonic Beat Lab v0.5

## Purpose

Subharmonic Beat Lab is a research prototype for exploring pleasant
low-frequency beat structures using subharmonic relationships rather
than conventional bass synthesis.

Current focus:

-   Manual exploration
-   Harmonic beat generation
-   Stereo transducer experiments
-   Foundation for a future Subharmonic Harmony Engine

Author: Jukka Linjama, 2026

------------------------------------------------------------------------

# Current Architecture

Keyboard → Base frequency → Subharmonic Engine → Harmonic Beat Engine →
Stereo Renderer → Playback

The architecture intentionally keeps harmony generation separate from
stereo rendering so different rendering strategies can be evaluated
without changing the harmonic engine.

------------------------------------------------------------------------

# Current Features

-   Virtual keyboard
-   Loop recording/playback
-   Offline WAV rendering (not yet fully validated)
-   Automatic ratio helper
-   Harmonic beat pair generation
-   Multiple harmonic ratios
-   Commented, modular source structure

------------------------------------------------------------------------

# Harmonic Beat Philosophy

The laboratory no longer focuses on generating the strongest possible
bass.

Instead it explores how harmonic relationships produce a pleasant,
living tactile field.

Fundamental principle:

Beat first. Harmony second.

The desired beat determines the harmonic relationship whenever possible.

------------------------------------------------------------------------

# Stereo Beat Research Notes

The largest open question is no longer how to compute subharmonics.

Instead:

How should harmonic beat structures be rendered to two tactile
transducers?

Several alternatives were explored:

1.  Identical L/R rendering
2.  Harmonic beat with stereo detuning
3.  Symmetric ±Δf rendering
4.  Harmonic pair separation

Experiments showed that perceived beat behaviour is surprisingly
sensitive to the exact frequency assignment.

Current observations:

-   Symmetric stereo rendering feels more alive.
-   Small mathematical changes can double the perceived beat.
-   Mechanical coupling between transducers probably contributes to the
    perception.
-   The optimum solution cannot be derived from theory alone; listening
    experiments are essential.

------------------------------------------------------------------------

# Open Questions

-   Should stereo be derived directly from harmonic beat equations?
-   Is the most pleasant sensation produced by harmonic correctness or
    by a moving mechanical interference field?
-   Which harmonic ratios produce the strongest body resonance?
-   How should rhythm evolve over time instead of remaining perfectly
    locked?

------------------------------------------------------------------------

# Next Milestone (v0.6)

Investigate stereo harmonic rendering independently from harmony
generation.

Possible experiments:

-   Center rendering
-   Harmonic ±Δf rendering
-   Cross stereo rendering
-   Dynamic stereo width
-   Behaviour-driven beat movement

Goal:

Create a natural, living tactile resonance field suitable for music
augmentation and future Sleep Elsi integration.
