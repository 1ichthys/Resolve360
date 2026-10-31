# Changelog

## 0.1.0 - Alpha
First public alpha.

- GPU reframer for 2:1 equirectangular 360 video, built as a Fusion Fuse (Metal / CUDA / OpenCL through Fusion's compute kernel support).
- Controls: Field Of View, Up/Down, Left/Right, Rotate, Distortion.
- Distortion slider blends from a flat view (0) through stereographic (1) to a round fisheye disc (above 1).
- Four preset buttons: Linear, Ultra, Wide, Fisheye.
- Edit-page wrapper (`Resolve360.setting`) so the effect can be dragged onto a clip from the Effects panel.
- Automatic detection of Resolve's "Fit" letterbox / pillarbox bars, so non-2:1 timelines work when clip scaling is set to Fit.
- Seam-safe sampling across the 360 degree wrap-around.
- Verified on DaVinci Resolve Studio 21 on macOS only. Windows, Linux and other GPUs are untested.
