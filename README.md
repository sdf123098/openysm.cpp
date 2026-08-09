# openysm.cpp — Sparkle Morpher native fork

This repository is the public, modified source used to build the `ysm-core`
native libraries packaged by [Sparkle Morpher](https://github.com/sdf123098/Sparkle-Morpher).

It is a fork of [OpenYSMDev/openysm.cpp](https://github.com/OpenYSMDev/openysm.cpp),
based on upstream commit [`3e86bb01b370ecf225d8d89442e6089840f999c5`](https://github.com/OpenYSMDev/openysm.cpp/commit/3e86bb01b370ecf225d8d89442e6089840f999c5).

The fork keeps the upstream MIT license. See [MODIFICATIONS.md](MODIFICATIONS.md)
for the Sparkle Morpher changes and [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md)
for bundled header notices.

## Build

Requirements:

- Zig 0.16.0 or newer
- Android NDK (only for `android-arm64`)

Build every packaged target:

```powershell
zig build -Dplatform=all -Drelease -Dandroid-ndk="$env:ANDROID_NDK_ROOT"
```

Use `-Dplatform=<target>` to build one target. Supported targets are
`windows-x64`, `windows-x86`, `linux-x64`, `macos-x64`, `macos-arm64`, and
`android-arm64`. Outputs are written to `zig-out/<target>/`.

The [Build native binaries](.github/workflows/build-natives.yml) workflow
builds all six targets on every push and pull request, publishes the generated
libraries with SHA-256 checksums as an Actions artifact, and provides the
reproducible build trail for the copies distributed by Sparkle Morpher.

## Source layout

- `dllmain.cpp` — JNI implementation and SIMD renderer
- `build.zig` — cross-platform Zig build definition
- `third_party/jni` — JNI headers
- `third_party/sse2neon` — ARM SIMD compatibility header
