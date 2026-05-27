# Elsi Vacuum Sleep Sound v0.12

Experimental browser-based vacuum sleep sound generator for calming background audio and baby sleep testing.

This prototype combines:
- one tonal vacuum motor loop
- synthetic air turbulence layers
- velvet-noise texture layers
- low-frequency rumble layers
- simplified end-user UI
- hidden technical tuning UI

The project is designed as:
- a standalone HTML prototype
- lightweight Web Audio experiment
- browser-safe realtime engine
- future mobile-friendly sleep app concept

---

## Main Features

### Simplified End User UI
The visible UI is intentionally minimal:

- Start / Stop toggle
- Volume + / -
- Mode selection:
  - Near
  - Far
  - Rumble
- Small "Tech" button for advanced controls

The goal is a calm, non-technical experience.

---

## Sound Design Structure

### Near Mode
Closer vacuum feeling:
- stronger motor identity
- lower stereo width
- less reverb
- more harmonic content

### Far Mode
Distant calming vacuum ambience:
- softer motor
- wider stereo field
- softer turbulence
- stronger low-frequency bed

### Rumble Mode
Extended low-frequency calming layer:
- stronger 30–60 Hz rumble
- deeper background pressure feeling
- intended for testing only

---

## Stereo Design

Stereo is intentionally conservative for the motor layer:
- motor remains nearly centered
- turbulence and velvet layers create width
- avoids "two vacuum cleaners" illusion

Wide stereo test settings were added in v0.9+ for evaluation.

---

## Technical UI

The hidden technical UI contains:
- runtime sliders
- stereo controls
- preset endpoint editing
- debug state logging
- loop preprocessing controls

This separation allows:
- simple end-user operation
- advanced tuning without clutter

---

## Embedded Sample Support

The app supports:
1. external WAV loading
2. embedded fallback sample via base64 data URL

This enables:
- standalone HTML testing
- file:// browser testing without Live Server
- easier demo sharing

Placeholder:

```js
const EMBEDDED_SAMPLE_DATA_URL = "";
```

---

## Current Architecture

The engine currently uses:
- one looping tonal motor source
- synthetic non-tonal layers
- parameter morphing between endpoint presets

The project intentionally avoids:
- granular synthesis
- FFT morphing
- heavy DSP pipelines

The current architecture favors:
- stability
- low CPU usage
- realtime control
- browser compatibility

---

## Future Ideas

Possible future directions:
- more organic random-walk modulation
- evolving turbulence spectra
- dynamic resonant body filters
- adaptive stereo movement
- embedded sample library
- mobile-first UI polish
- sleep timer and automation

---



---

## Background

This prototype is based on earlier vacuum-loop and Web Audio API experiments developed in:

https://github.com/JukkaTLinjama/jukkatlinjama.github.io/tree/main/vacuum-loop-lab

The Elsi prototype evolved from:
- vacuum loop preprocessing experiments
- realtime Web Audio API control testing
- velvet-noise ambience layers
- stereo and rumble prototyping
- simplified sleep-app UI concepts


## Version

v0.12 — calm transitions and rumble mode prototype

