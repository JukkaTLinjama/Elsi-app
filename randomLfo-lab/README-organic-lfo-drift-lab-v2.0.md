# Organic LFO Drift Lab v2.0

Standalone research lab for Elsi Sleep Sound App organic modulation.

## Purpose

Create a natural, non-repetitive modulation source for slow pitch and amplitude drift.

## Core model

The modulation source is now a physical spring-mass-damper style stochastic system.

Previous parameters:

- centerForce
- damping

were replaced with:

- Natural frequency (Hz)
- Damping ratio ζ

Model:

x'' + 2ζω0 x' + ω0²x = random excitation

where:

ω0 = 2π * naturalFrequencyHz
mass = 1

## Signal flow

random excitation
→ acceleration
→ velocity
→ position / drift value
→ pitch + amplitude modulation

## v2.0 changes

- Replaced centerForce with natural frequency Hz
- Replaced damping with damping ratio
- Added physical parameter mapping
- Added excitation normalization compensation
- Fixed debug readout jumping
- Added protection for missing debug values

## Analysis tools

Included:

- output time plot
- velocity plot
- random force plot
- modulation output plot
- spectrum estimate
- autocorrelation plot

## Time scale

Time scale runs the stable process faster:

1x ... 100x

Purpose:

- faster spectrum analysis
- faster autocorrelation testing
- listening for hidden periodicity

The internal model behavior remains based on the stable baseline.

## Findings

Low damping:
- narrow-band behavior
- damped sine autocorrelation
- more resonant character

Higher damping:
- broader spectrum
- less repeating character
- more organic random drift

## Elsi integration idea

User UI:

Liveliness

Possible tech controls:

- Drift natural frequency
- Damping ratio
- Pitch drift depth
- Amp drift depth

## Future experiments

- OU process comparison
- wandering resonance
- feedback controlled resonance
- audio rendering of the stochastic process
