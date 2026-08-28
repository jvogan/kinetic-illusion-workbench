# Shape-to-illusion method

Use this process to design carrier geometry for a new subject.

## 1. Identify recognizable features

Choose the fewest features that keep the subject recognizable in motion: a gap, head, branch, loop, crossing, unequal pair, terminal, or isolated marker. Preserve them in the carrier geometry.

Remove details that hinder particle tracking or add perspective, shading, occlusion, or size cues.

## 2. Choose a carrier

| Input | Useful carrier |
|---|---|
| Flat icon, arrow, glyph, logo | Silhouette wrapped around a cylindrical ribbon |
| Line drawing or word | Tube swept along one or more curves |
| DNA, RNA, rope, vine | Paired helical or spline tubes with sparse cross-links |
| Ring, eye, shell, vessel | Surface of revolution |
| Animal or object silhouette | Layered contour ribbons or a simplified point-sampled shell |

Choose a carrier with one periodic rotation parameter. Set a period that returns every material point to its initial position.

## 3. Parameterize before drawing

Define material coordinates independent of the camera. For a cylindrical carrier, use azimuth `u`, axial position `v`, and radius `r`. For a swept curve, use path position `s` and cross-section angle `u`.

Seed particle sampling deterministically and store each persistent particle in material coordinates.

- For an SVG silhouette, flatten the path to contours, normalize its bounds, sample candidate `(u,v)` points, and keep points that pass a point-in-path test. Map `u` around the carrier.
- For a raster silhouette, threshold alpha or luminance into a mask, sample occupied pixels, and normalize them to `(u,v)`. Map the points to the carrier.

Preserve holes and one or two defining outline features.

## 4. Apply motion and camera projection

Transform material points into world and then view coordinates at time `t`. Begin with orthographic projection. Keep the full view-space depth value even if projection discards it.

The basic order is:

```text
material point -> moving world point -> view point (x, y, z) -> screen point
```

For a carrier that rotates around world `y`, define `a` as its animated azimuth, `v` as its axial coordinate, and `r` as its radius:

```text
x_world = r * cos(a)
y_world = v
z_world = -r * sin(a)
```

Let `theta` be the camera elevation. Rotate the world point into the camera frame around `x`:

```text
x_view = x_world
y_view = y_world * cos(theta) - z_world * sin(theta)
z_view = y_world * sin(theta) + z_world * cos(theta)
```

Project the first two camera coordinates and preserve the third:

```text
screen_x = center_x + scale * x_view
screen_y = center_y - scale * y_view
view_depth = z_view
```

These signs define the starter's convention. If you use another sign convention, apply it consistently to projection, color polarity, tests, and interpretation labels.

## 5. Apply the viewer-aligned field

Compute displayed color after the point reaches view space:

```text
R_depth = max(abs(view_z)) over all points and one full motion cycle
color = palette(clamp(0.5 - polarity * view_z / (2 * R_depth), 0, 1))
```

Keep `R_depth` fixed for the whole animation; per-frame normalization changes the color scale during rotation. The formula defines an invisible viewer-aligned color field. The field changes each persistent particle's displayed color as its view-space depth changes.

For the cylindrical camera model, each particle's full-cycle extent is:

```text
abs(v * sin(theta)) + abs(r * cos(theta))
```

Use the maximum over all particles. For another carrier, sample every material point over one complete motion cycle and keep the maximum absolute `z_view`.

Apply the transformation in this order for every frame:

```text
for each persistent material particle:
    world_point = carrier(material_particle, time)
    view_point = camera(world_point)
    palette_position = clamp(0.5 - polarity * view_point.z / (2 * R_depth), 0, 1)
    draw(project(view_point), palette(palette_position))
```

Use broad endpoint regions and a compressed transition band when matching the examples' cream-to-red-to-blue ramp. Keep palette direction configurable because viewers may reverse its depth meaning.

## 6. Suppress unintended depth cues

Start without perspective, z-buffer occlusion, directional shading, depth-dependent particle size, or depth-dependent luminance. Keep draw order independent of depth. Use weighted accumulation when exact order independence matters. Add each depth cue separately when testing its effect.

## 7. Preserve trackability

If a periodic shape becomes stationary or hard to follow, add one asymmetric gap, marker, or endpoint. Keep the feature equally visible on near and far surfaces.

## 8. Deliver a testable preview

Default to a self-contained browser preview when the user does not choose a format. Show the illusion first, keep diagnostics secondary, and provide pause and restart controls. Export at least one full revolution when a shareable video helps.

Ask viewers whether they perceive both intended readings of depth or rotation.
