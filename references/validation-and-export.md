# Validation and export

## Browser prototype

Check the page in a real browser at desktop and narrow mobile sizes.

- Confirm that the browser console reports zero errors or warnings.
- Confirm that the page has no horizontal overflow.
- Confirm that controls have associated labels and keyboard-visible focus.
- Confirm that animation respects reduced-motion preferences.
- Confirm that primary controls remain visible and diagnostics remain secondary.
- Open the self-contained page offline and confirm that every local asset loads.

## Perceptual invariants

Seed particle generation and expose a way to render an exact time.

Expose `renderFrame(t)` or `setTime(t)`. The hook must pause playback, set an exact time, render once, and resolve after the canvas updates. Use a browser harness to save numbered screenshots. An offline renderer can use the same geometry and seed.

Run these checks:

- In signed view-depth mode, confirm that a tracked point changes color as its view-space depth changes.
- Confirm that an interpretation-label switch produces an identical canvas hash.
- Confirm that diagnostic depth cues default to off.

Numerical tests establish image invariants. Viewer reports establish perceptual reversal.

## Loop export

If the renderer accepts exact times, render each frame directly; playback recording introduces timing variance.

Choose a frame count that completes a whole number of turns:

```text
frames_per_turn = frame_rate * 360 / degrees_per_second
```

Encode a broadly compatible MP4:

```bash
ffmpeg -y -framerate 30 -i frames/f%04d.png \
  -c:v libx264 -preset slow -crf 18 -pix_fmt yuv420p \
  -movflags +faststart output.mp4
```

Verify with `ffprobe`:

- Confirm that the file contains one H.264 video stream and zero audio streams.
- Confirm the `yuv420p` pixel format.
- Confirm the expected dimensions, frame rate, frame count, and duration.

Render the frame immediately after the final encoded frame and compare it with frame zero. The two frames must be pixel-identical.
