---
name: kinetic-illusion-workbench
description: 'Create rotating optical illusions in which arrows, ribbons, particles, or DNA/RNA helices can appear to spin either way. Use when the user asks "which way is it spinning?" or wants a browser preview or video that recomputes particle color from viewer-aligned depth.'
---

# Kinetic Illusion Workbench

Build a viewable illusion from the user's concept. Use the arrow and helix to study the method, then choose geometry that fits the requested subject.

## Completion contract

For creation requests, complete these steps:

- Produce a browser preview, video, or image that the user can inspect.
- When the medium is unspecified, create a self-contained browser preview. Add a shareable MP4 if a still image cannot show the rotation.
- If the environment includes a browser, open the preview. If the preview needs a local server, bind it to loopback and give the user a direct link. Stop the server before handoff.
- Put the illusion first. Keep explanation and diagnostics secondary.
- Add controls to pause, restart, and compare the intended interpretations.
- Ask the user whether they perceive both intended readings.

## Start with the shape

Read [references/shape-to-illusion.md](references/shape-to-illusion.md) for every creation task. Convert the subject into a parameterized surface or curve that preserves its defining features during rotation.

Choose the simplest useful carrier:

- Wrap a flat symbol or silhouette around a cylindrical ribbon.
- Sweep a tube along a curve for strands, lettering, or molecular forms.
- Use paired curves for a duplex or linked structure.
- Revolve a profile for rings, vessels, shells, or radial symbols.

Keep one or two distinctive features so the motion remains trackable. Do not add features that reveal which surface is nearer.

## Choose the relevant mode

- For a rotating ribbon, cylinder, or particle surface, read [references/rotating-depth-fields.md](references/rotating-depth-fields.md).
- For a helix that may read as left- or right-handed, also read [references/helix-chirality.md](references/helix-chirality.md).
- Before delivering a browser prototype or video, read [references/validation-and-export.md](references/validation-and-export.md).

Start from the readable [browser starter](assets/starter/index.html) for a new subject. Its named functions separate shape sampling, carrier motion, camera transformation, projection, signed depth coloring, and deterministic rendering.

Adapt the readable browser starter for new subjects. Study the runnable arrow and helix pages as completed examples. The arrow renderer is minified, and the helix renderer is specialized for paired strands.

## Build the illusion

1. Map the requested subject to a carrier that preserves its defining gap, branch, loop, crossing, or endpoint during rotation.
2. Define the target and alternate percepts. State which rendered stimulus must remain unchanged when the interpretation changes.
3. Parameterize the carrier and sample a persistent material texture. Keep each particle's identity through a complete motion cycle.
4. Move the geometry and project it into view coordinates.
5. Compute each particle's display color from its current view-space depth. A material-fixed particle changes color as it crosses the viewer-aligned field.
6. Start with orthographic projection, constant particle size, depth-independent draw order, no particle lighting, and no z-buffer occlusion.
7. Reserve stronger depth cues for labeled controls.
8. If the user supplies a reference, measure its period, projection, texture persistence, color values, and compositing separately. Compare each color-attachment model against sampled frames, and report the comparison method and image error.
9. Make the ambiguity testable. An interpretation control must preserve the pixels. A geometry control must label the changed stimulus.
10. Before export, verify deterministic frames, expected symmetry, loop closure, keyboard controls, reduced motion, and an error-free browser console.
11. Separate numerical results from the human perception test. Pixel equality establishes image recurrence; viewer reports establish perceptual bistability.

## Output expectations

For an interactive output, write into the user's active workspace. When the user has not chosen a framework, create a self-contained HTML page with working defaults, playback controls, and a separate diagnostics section.

### HTML report style

Use a minimal report layout unless the user supplies another visual style.

- Put the illusion before the report.
- Set the report column width to `65ch–75ch`.
- Use a neutral background, high-contrast text, and one restrained accent color.
- Use one text family. Reserve monospace for measurements, formulas, and code.
- Separate sections with whitespace or thin rules.
- Use plain tables, short captions, and descriptive headings.
- Avoid gradients, glass effects, nested cards, metric dashboards, decorative motion, and oversized marketing copy.
- Check the layout at 320 px, test every control by keyboard, honor `prefers-reduced-motion`, and add print styles.

For a shareable recording, render a whole number of revolutions at a fixed frame rate. Use H.264 with `yuv420p` and fast-start metadata unless the user requests another format. Export the animation without explanatory controls.

For a report, distinguish measurement, model, inference, and human acceptance criteria. Include the exact color-attachment rule. Permanent material color produces a different stimulus.
