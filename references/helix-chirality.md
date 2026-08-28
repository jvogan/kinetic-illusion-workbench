# Ambiguous helix chirality

## Paired interpretation

Represent a strand as:

```text
a(v, t) = hand * twist(v) + spin * t + phase
x = r * cos(a)
z = -r * sin(a)
```

Under orthographic projection, reflecting depth sends `a` to `-a` while leaving projected `x` unchanged. The paired transformation is:

```text
hand  -> -hand
spin  -> -spin
phase -> -phase
camera elevation -> -camera elevation
```

Negating handedness, spin, phase, and camera elevation produces the paired projection. For a pixel-identical rendering, pair the depth reflection with the opposite perceived depth-color polarity.

## Useful molecular presets

Use these approximate values for optical studies. Use a molecular model when the user requests molecular accuracy.

| Form | Hand | Base pairs/turn | Rise/base pair | Radius | Notes |
|---|---:|---:|---:|---:|---|
| B-DNA | right | 10.5 | 3.4 Å | 10 Å | Familiar duplex silhouette |
| A-form RNA duplex | right | 11.0 | 2.55 Å | 11.5 Å | Wider, more compact pitch |

## Preserve ambiguity

- Keep perspective, depth-dependent size, shading, and occlusion off by default.
- Optional: Use unequal groove spacing. Reflection preserves the unordered groove-width pair.
- Include an aperiodic marker or visible termini when rotation becomes hard to track, but verify that the marker does not encode depth order.
- Make the interpretation button update the text while the canvas remains unchanged.

## Human acceptance criterion

Mathematical equivalence establishes that two scene interpretations fit the projection. Viewer reports establish whether perception reverses spontaneously or on request.
