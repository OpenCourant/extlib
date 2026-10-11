# OpenCourant external libraries (extlib)

Prebuilt external libraries required to build and run OpenCourant Starter
and Engine. The package is distributed as a release asset of this
repository and downloaded automatically at build time by
`Compiling_tools/script/load_extlib.py` in the main
[OpenCourant](https://github.com/OpenCourant/OpenCourant) repository, which
verifies its SHA-256 checksum before use.

## Current package

| Release | File | SHA-256 |
|---|---|---|
| [`upstream-v82-extlib`](../../releases/tag/upstream-v82-extlib) | `extlib.zip` | `2d0365018e67dd4813e6a2cf2ebf19d00d4771aa1aeb9b9706b7ea45fbb56982` |

This is the **pristine official extlib v82** — the final upstream delivery
(2026-09-22), recovered from a community-donated source checkout and served
unmodified. Its hm_reader binaries (all three platforms, build
`20260918_f907fe76`) provide the complete selection API: OpenCourant builds
with **no reader compatibility fallbacks**.

**The reader recovery effort is complete.** Every upstream artifact that
mattered has been recovered, authenticated and archived (see
[Archive releases](#archive-releases) below). Thank you to everyone who dug
through their downloads folders.

### Known limitation

`/ALE/STRUCTURED_MESH` is rejected explicitly by OpenCourant builds. The
feature is non-functional in the shipped readers: testing against the v82
reader (with its debug channel enabled) shows input parsing succeeds but
mesh generation emits degenerate output — all generated nodes collapse to a
single coordinate — matching the invalid-node-coordinates failure
previously observed with the v70 win64 reader. The feature shipped upstream
without tests, examples or documentation, and its generator never appeared
in the open-reader source. An explicit rejection beats silently wrong
meshes; a from-scratch implementation in the open reader is the eventual
fix.

## Package contents

| Component | Type | License |
|---|---|---|
| hm_reader (linux64/linuxa64 `.so`, win64 `.dll`/`.lib`) | input reader, binary only | Altair limited-use notice |
| H3D (headers, linux64/linuxa64 `.so`, win64 `.dll`) | result file I/O, binary + headers | Altair limited-use notice |
| APR (`libapr-1.so.0`) | hm_reader dependency | Apache-2.0 |
| Boost 1.70.0 | headers + static libs | BSL-1.0 |
| exprtk | header-only | MIT |
| LAPACK 3.10.0 | sources/libs | BSD-3-Clause (modified) |
| metis | static libs | Apache-2.0 era notice |
| md5 | header + lib | RSA MD5 notice |
| zlib | shared/static libs | zlib |

Full license texts are in [`LICENSES/`](LICENSES/) and are also bundled
inside the package (`extlib/license/`).

### Altair license terms

The hm_reader and H3D components are Copyright Altair Engineering Inc. /
Siemens and are distributed under a limited-use notice:

> grants to third parties limited permission to use [HMR/H3D] solely in
> connection with OpenRadioss software.

OpenCourant is the community continuation of OpenRadioss. These components
are redistributed here, unmodified and with all original notices intact,
solely to keep OpenRadioss-derived software buildable after the upstream
distribution channels were removed.

## Provenance

The current package is the unmodified official v82 `extlib.zip`:

- Recovered from a community-donated OpenRadioss source checkout built
  2026-09-28 from upstream main at commit `1a0d691` — the exact commit
  OpenCourant continues from. The checkout's manifest pins the official
  v82 download URL.
- Authenticated against an independent source: the contained
  `libh3dwriter.so` (linux64) is byte-identical to the blob preserved in
  the recovered upstream git history (branch
  [`upstream-v82`](../../tree/upstream-v82), `9975a8fa…`), and internal
  file dates match the v82 delivery day.
- Reader build ID `20260918_f907fe76` across linux64, linuxa64 and win64.

### Reader build versions

| Component | Build ID | Effective extlib era |
|---|---|---|
| `libhm_reader_linux64.so` | `20260918_f907fe76` | v82 |
| `libhm_reader_linuxa64.so` | `20260918_f907fe76` | v82 |
| `hm_reader_win64.dll` | `20260918_f907fe76` | v82 |

All reader APIs the Starter uses are present (`cpp_is_part_with_elements`,
`cpp_get_include_file_by_index`, `cpp_sale_mesh_create`, the rigid-part
pair); the `HM_READER_*` compatibility options in the main repository
default OFF and exist only for builds against the older archived readers
below.

## Package history

Earlier OpenCourant packages, superseded by the pristine v82 but kept for
reproducibility:

| Package | Readers | Notes |
|---|---|---|
| [`v82-hybrid-r4`](../../releases/tag/v82-hybrid-r4) | v70 (linux64/win64), v70 (linuxa64) | v82 git-tree libraries + recovered v70 readers |
| [`v82-hybrid-r3`](../../releases/tag/v82-hybrid-r3) | v70 (x86/win), Feb-2026 (linuxa64) | first ARM reader |
| [`v82-hybrid-r2`](../../releases/tag/v82-hybrid-r2) | v70 (linux64/win64) | Linux reader upgraded v60 → v70 |
| [`v82-hybrid`](../../releases/tag/v82-hybrid) | v60 (linux64), v70 (win64) | v82 git-tree H3D restoration |
| [`v67-compat`](../../releases/tag/v67-compat) | v60 (linux64), v70 (win64) | original community rescue package, first published by [@AlexanderHuels](https://github.com/AlexanderHuels) |

## Archive releases

Pristine upstream artifacts, preserved with published SHA-256 digests:

| Release | Contents |
|---|---|
| [`upstream-v82-extlib`](../../releases/tag/upstream-v82-extlib) | official `extlib.zip` v82 (current package) |
| [`upstream-v70`](../../releases/tag/upstream-v70) | official `extlib.zip` v70 |
| [`upstream-latest-20260728-win64`](../../releases/tag/upstream-latest-20260728-win64) | official `OpenRadioss_win64.zip`, last stable release |
| [`upstream-latest-20260728-linux64`](../../releases/tag/upstream-latest-20260728-linux64) | official `OpenRadioss_linux64.zip`, last stable release |
| [`upstream-latest-20260728-linuxa64`](../../releases/tag/upstream-latest-20260728-linuxa64) | official `OpenRadioss_linuxa64.zip` + source tar, last stable release |
| [`upstream-linux64-20260320`](../../releases/tag/upstream-linux64-20260320) | March 2026 linux64 release package |
| [`upstream-linuxa64-20260320`](../../releases/tag/upstream-linuxa64-20260320) | March 2026 linuxa64 release package |

## Upstream git history (branch `upstream-v82`)

The branch [`upstream-v82`](../../tree/upstream-v82) preserves the complete
upstream `OpenRadioss_extlib` git history through the final v82 delivery
(2026-09-22, commit `f2fa2cd00dd5937d96e521f163a191f2dee90148`, 111
commits), recovered from the GitHub fork network before it could be garbage
collected. Tags `upstream-v67` and `upstream-v82` mark the two reference
points. Note that hm_reader binaries were never tracked in the upstream git
tree; they were injected from an internal artifact store at release time.

## Validation

Every package revision was validated before adoption: Starter and Engine
builds on all supported platforms and the full MiniQA regression suite
(81/81) gate each change, in CI and in the release pipeline.
