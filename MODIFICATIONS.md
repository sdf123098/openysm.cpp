# Sparkle Morpher modifications

This fork is based on upstream commit
[`3e86bb01b370ecf225d8d89442e6089840f999c5`](https://github.com/OpenYSMDev/openysm.cpp/commit/3e86bb01b370ecf225d8d89442e6089840f999c5).

## `dllmain.cpp`

- Adds native ABI version `3` and registers it through `ModelAccelerationBridge`.
- Aligns native bone visibility and child traversal with Sparkle Morpher's Java
  fallback: a bone is hidden only when all three scale channels are zero, and
  the native-only `skipChildrenFlag` no longer suppresses child bones.
- Removes the native screen-space back-face determinant test. Back-face culling
  is performed by the existing OpenGL render pipeline, avoiding missing or
  flickering visible faces at certain rotations.

## `build.zig`

- Uses `ReleaseFast` as the default optimization mode.
- On Windows, calls the Android NDK's `clang++.exe` directly and supplies the
  Android target as an argument. This avoids the NDK batch-wrapper argument
  parsing failure while preserving the same compiler target.

## `README.md`

- Documents source provenance and reproducible builds for the distributed
  Sparkle Morpher native libraries.
