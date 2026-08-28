# Rotating surfaces and view-depth color fields

## The essential separation

The method uses two coordinate attachments:

- Particles remain fixed to the rotating material.
- Display color comes from a viewer-aligned field on every frame.

For a point with rotating azimuth `a(t)`, radius `r`, and axial coordinate `v`:

```text
a(t) = a0 + omega * t
x = r * cos(a)
z = -r * sin(a)
```

With a normalized view-depth palette and a fixed full-cycle depth extent `R_depth`:

```text
palette_position = clamp(0.5 - polarity * z / (2 * R_depth), 0, 1)
color = palette(palette_position)
```

Compute `R_depth = max(abs(z))` over every sampled point and a complete motion cycle, then keep it fixed. Recompute `z` and `color` on every frame. Store the particle's material coordinates and derive its view state per frame.

## Common attachment errors

- `color = f(v)` produces permanent top-to-bottom or axial dye.
- `color = f(screen_y)` creates a screen overlay rather than a depth volume.
- `color = f(material_u)` attaches the palette to the rotating texture.
- Sorting or z-buffering particles by depth adds an occlusion cue when hue has no assigned near-or-far meaning.

Keep these alternatives as labeled controls when they help compare hypotheses. Label each as a separate attachment model.

## Weak and strong depth cues

Hue can encode view-space depth without a learned near-or-far association. Luminance, perspective, size, blur, shading, and occlusion provide stronger depth evidence.

Start with:

- Use orthographic projection.
- Keep particle size and brightness constant.
- Disable z-buffer occlusion.
- Disable cast shadows and directional lighting.
- Use a draw order that does not depend on view-space depth.
- If exact order independence is required, use weighted accumulation.

Constant brightness excludes particle lighting and depth attenuation. When reproducing a palette with unequal Rec.709 luma, preserve it and add a constant-luma control.

## Reference analysis

Measure one variable at a time:

- Infer the rotation period from phase repetition or tracked features.
- Test texture persistence after motion compensation.
- Compare color against view-space depth, surface normal, screen height, and material coordinates.
- Compare orthographic and perspective fits.
- Inspect tangent seams and crossings for compositing behavior.

Sample at least 12 evenly spaced phases over one revolution. Report the mean absolute error or R² for each candidate attachment model. Use the full set of phases because screen height and view depth can correlate within one frame.
