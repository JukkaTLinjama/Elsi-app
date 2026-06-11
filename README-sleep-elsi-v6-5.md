# Sleep Elsi v6.5

Sleep Elsi is a single-file browser app for calm layered sleep and relaxation sound. It combines a tonal loop source with a continuous noise bed and a compact mobile-first UI.

## Version

**v6.5 · muted volume pulse**

This version continues from the v6.1b tonal scene transition architecture and adds mobile/UI refinements through v6.5.

## Main changes since v6.1b

### Tonal scene transition architecture

- Split visible scene state from audible tonal scene state.
- `sceneKey` represents UI, noise bed, and live expression state.
- `activeTonalSceneKey` represents the currently audible loop identity.
- Tonal loop source changes only while the tonal layer is silent.
- Replaced the rejected playbackRate freeze approach with silent parameter switching.
- Tonal transition flow:
  - fade old tonal source out
  - apply loop parameters while silent
  - fade new tonal source in
- Noise bed remains continuous and morphs independently over the longer scene transition.
- Preserved 5 second noise scene transitions.
- Kept user volume independent from scene transitions.
- Preserved live playback control updates for liveliness and tonal/noise balance.

### Mobile UI fixes

- Fixed Advanced panel clipping on mobile so the full Advanced section can scroll.
- Removed the background card styling from the intro/subtitle area while keeping vertical breathing room under the title.
- Restored compact mobile Sound selector rows to save vertical space.

### Volume UI update

- Reduced the visible volume indicator from 9 arcs to 7 centered arcs.
- Synced the motion/transition gradient with the updated 7-arc visual field.
- Updated volume scale to:
  - mute
  - −30 dB
  - −22 dB
  - −16 dB
  - −10 dB
  - −4 dB
  - 0 dB
  - +3 dB
- Adjusted long-press repeat safety gates for the shorter 7-step scale.
- Added a muted-state pulse indicator:
  - shown only when audio is running and volume is muted
  - uses the same pulse rhythm as the `muted` status text
  - does not reuse the moving volume transition gradient

## Current architecture summary

The app separates three concerns:

1. **Visible scene state**
   - UI selection
   - scene button highlights
   - live expression controls
   - noise bed target

2. **Audible tonal scene state**
   - active loop family
   - loop source identity
   - playbackRate/load/pitch parameters
   - only switches during the silent midpoint of a tonal transition

3. **Noise bed state**
   - continuous shared masking/body layer
   - morphs smoothly across scene changes
   - has its own load ramp to avoid jumps caused by tonal loop load switching

This prevents audible loop pitch/load jumps while keeping the noise bed alive and smooth during scene changes.

## Important implementation notes

- Do not reintroduce the v6.1c playbackRate freeze concept; it caused pulse artifacts.
- Avoid changing tonal scene transition logic unless a specific audible problem is confirmed.
- SVG volume arcs and gradient alignment are delicate. If further changes are needed, test the visual SVG geometry separately before changing volume dB mapping.
- The muted pulse should remain a separate visual state, not part of the moving target/actual volume transition display.
- Keep the app as a single HTML file for now.

## Suggested manual test checklist

1. Start playback.
2. Change between Vacuum, Hum, and Vibro scenes.
3. Confirm tonal source fades out, switches, and fades in without a pitch/load jump.
4. Confirm noise bed continues smoothly during scene changes.
5. Change Liveliness during playback and confirm it updates immediately.
6. Change Tonal / Noise Bed balance during playback and confirm it updates immediately.
7. Stop playback, change scene, then start again.
8. Confirm start begins from the selected scene without an old-scene audible glide.
9. Test mobile Advanced panel scrolling to the bottom.
10. Test volume steps:
    - mute
    - −30 dB
    - −22 dB
    - −16 dB
    - −10 dB
    - −4 dB
    - 0 dB
    - +3 dB
11. Confirm muted state shows both `muted` text and the pulsing mute indicator in sync.

## Known caution areas

- Mobile browser behavior should still be checked on the actual device, not only Chrome responsive mode.
- Volume SVG geometry and gradient alignment are visually sensitive.
- Any future scene-transition change should be isolated from UI/CSS changes.
- Audio changes should be versioned separately from visual refinements when possible.

## Attribution

JukkaTLinjama 2026
