# ffmpeg-builds

Reproducible, **LGPL-only** builds of [FFmpeg](https://ffmpeg.org/), published as shared libraries for use by
Chibilogic audio applications. The applications load the libraries dynamically and keep them as separate, replaceable
files.

The repository holds the documentation and licence texts. The binaries and the matching sources are attached to each
**GitHub release**.

## What is built

- **Upstream:** the unmodified FFmpeg release tarball, verified against its upstream signature. No source patches are
  applied.
- **Libraries:** `avutil`, `avcodec`, `avformat` and `swresample`, shared only. No programs, no `avdevice`,
  `avfilter` or `swscale`, no network code, and no hardware or external library auto-detection.
- **Licence switches:** `--disable-gpl --disable-nonfree --disable-version3`. The resulting libraries are therefore
  under the GNU Lesser General Public License, version 2.1 or later.
- **Profile `p2`:** a decode-only component set for audio playback and analysis.
  - Decoders: `mp3float`, `aac`, `alac`, `flac`, `vorbis`, `wmav1`, `wmav2`, `wmapro`, and PCM (`u8`, `s8`,
    `s16le/be`, `s24le/be`, `s32le/be`, `f32le/be`, `f64le/be`).
  - Demuxers: `mp3`, `mov` (MP4 / M4A), `aac`, `asf`, `wav`, `flac`, `ogg`, `aiff`.
  - Parsers: `mpegaudio`, `aac`, `flac`.
  - Protocol: `file` only. No encoders, muxers or bitstream filters.

## Releases

Each release is named `<ffmpeg version>-<profile>-<target>`, for example `8.0.3-p2-W1`, and carries:

| Asset | Content |
|---|---|
| `ffmpeg-<version>-<profile>-<target>.zip` | The build: `bin/` (DLLs), `lib/` (import libraries and `.def` files), `include/`, `licenses/`, and `BUILDINFO.txt` with the exact configure line and toolchain versions. |
| `ffmpeg-<version>-<profile>-corresponding-source.tar` | The LGPL corresponding source: the upstream tarball and its signature, the build scripts, and the build information of every target built from it. |
| `SHA256SUMS` | The SHA-256 of every asset above. |

| Release | FFmpeg | Target | Status |
|---|---|---|---|
| `8.0.3-p2-W1` | 8.0.3 | Windows x64 (mingw-w64 GCC cross-build; MSVC import libraries) | in preparation |

## Verifying a download

```sh
sha256sum -c SHA256SUMS
```

Applications built against these releases pin the asset's SHA-256. A build fails if the downloaded file differs.

## Rebuilding from source

The corresponding-source archive contains everything needed:
1. Check the upstream tarball against its `.asc` signature.
2. Configure with the line recorded in `BUILDINFO.txt` for the target.
3. Build.

For `W1` the build is a cross-compile with `x86_64-w64-mingw32-gcc`. The MSVC import libraries (`*.lib`) are then
generated from the `.def` files with `lib.exe /def:<name>.def /machine:x64 /Brepro`. That step does not change the
DLLs.

## Licence

- FFmpeg is free software. The libraries in these releases are licensed under the
  **GNU Lesser General Public License v2.1 or later**; see [`COPYING.LGPLv2.1`](COPYING.LGPLv2.1) and FFmpeg's own
  `LICENSE.md` inside the source tarball.
- FFmpeg is a trademark of Fabrice Bellard, originator of the FFmpeg project. These builds are not produced or
  endorsed by the FFmpeg project.
- The documentation in this repository is provided under the same licence.

If you received these libraries as part of an application, you may replace them with your own build of the same
FFmpeg version, made from the corresponding source above.
