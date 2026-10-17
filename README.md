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
- OpenHarmony Native SDK 6.0 native component (only for `ohos-arm64`)

Build every packaged target:

```powershell
zig build -Dplatform=all -Drelease -Dandroid-ndk="$env:ANDROID_NDK_ROOT"
```

Use `-Dplatform=<target>` to build one target. Supported targets are
`windows-x64`, `windows-x86`, `linux-x64`, `macos-x64`, `macos-arm64`, and
`android-arm64`, plus `ohos-arm64` when OpenHarmony SDK is provided. Outputs
are written to `zig-out/<target>/`.

Build only OpenHarmony ARM64 output from `build.zig`:

```bash
export OHOS_NATIVE_SDK=/absolute/path/to/ohos-sdk/native
zig build -Dplatform=ohos-arm64 -Drelease -Dohos-sdk-native="$OHOS_NATIVE_SDK"
```

This target uses OpenHarmony clang/sysroot with `--target=aarch64-linux-ohos`
and generates `zig-out/ohos-arm64/libysm-core.so`. It is intentionally isolated
from Android NDK/Bionic.

The [Build native binaries](.github/workflows/build-natives.yml) workflow
builds all six targets on every push and pull request, publishes the generated
libraries with SHA-256 checksums as an Actions artifact, and provides the
reproducible build trail for the copies distributed by Sparkle Morpher.

The [OpenHarmony ARM64 native workflow](.github/workflows/build-ohos-arm64.yml)
adds an experimental OHOS cross-build gate with static JNI ABI checks and ELF
dependency allowlisting. It currently expects `libc.so` and
`libc++_shared.so`; packaging/runtime resolution of `libc++_shared.so` must be
handled by the downstream OHOS runtime/loader integration.

## HarmonyOS NEXT release gates

- Cross-compile checks and JNI symbol verification must pass in CI.
- ABI v3 and native buffer layout guards must pass static regression checks.
- JNI runtime smoke on an OHOS-capable JVM/device is required before claiming
  device compatibility (currently blocked in GitHub-hosted CI).
- Do not publish this fork as fully validated HarmonyOS NEXT device support
  until runtime loading/testing is complete.

## Source layout

- `dllmain.cpp` — JNI implementation and SIMD renderer
- `build.zig` — cross-platform Zig build definition
- `third_party/jni` — JNI headers
- `third_party/sse2neon` — ARM SIMD compatibility header
