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
| [`v67-compat`](../../releases/tag/v67-compat) | `extlib.zip` | `6ebf4f3d5caa3f4c8608e6d5dafc360c055272cbfc754120181435dd8f51e4cd` |

This is the **v67 compatibility set**: the newest external-library set that
remained recoverable from public sources after the upstream
`OpenRadioss/OpenRadioss_extlib` repository (and its release archives,
including v82) were deleted from GitHub.

### Known limitation

The v67 reader cannot generate `/ALE/STRUCTURED_MESH`. Compatibility builds
of OpenCourant reject that keyword explicitly rather than silently
producing invalid behavior.

## Package contents

| Component | Type | License |
|---|---|---|
| hm_reader (linux64 `.so`, win64 `.dll`/`.lib`) | input reader, binary only | Altair limited-use notice |
| H3D (headers, linux64/linuxa64 `.so`, win64 `.dll`) | result file I/O, binary + headers | Altair limited-use notice |
| APR (`libapr-1.so.0`) | hm_reader dependency | Apache-2.0 |
| Boost 1.70.0 | headers + static libs | BSL-1.0 |
| exprtk | header-only | MIT |
| LAPACK 3.10.0 | sources/libs | BSD-3-Clause (modified) |
| metis | static libs | Apache-2.0 era notice |
| md5 | header + lib | RSA MD5 notice |
| zlib | shared/static libs | zlib |

Full license texts are in [`LICENSES/`](LICENSES/) and are also bundled
inside the package (`extlib/license/` and per-component `notices/`
directories).

### Altair license terms

The hm_reader and H3D components are Copyright Altair Engineering Inc. and
are distributed under a limited-use notice:

> Altair Engineering Inc. grants to third parties limited permission to
> use [HMR/H3D] solely in connection with OpenRadioss software.

OpenCourant is the community continuation of OpenRadioss. These components
are redistributed here, unmodified and with all original notices intact,
solely to keep OpenRadioss-derived software buildable after the upstream
distribution channels were removed.

## Provenance

The v67-compat package was assembled from the following sources (recorded
in [`EXTLIB_VERSION.json`](EXTLIB_VERSION.json), which is a copy of the
manifest bundled inside the package):

- **Base library set (v67)**: git revision
  `f10f11a4ba6943e2d950ac12da47461ea7b8f05d` of the `OpenRadioss_extlib`
  repository, preserved by public forks after the upstream repository was
  deleted (fork network of `xupeiwust/OpenRadioss_extlib`).
- **hm_reader linux64** (`libhm_reader_linux64.so`,
  SHA-256 `37a8fcc2da2fc4e3581ae668176204fa4d206ac866ee4da53a1e988a543195ca`)
  and `libapr-1.so.0`
  (SHA-256 `f76d9796e59b675cb374fc3f3403f7878bdcb348e4c6f8af017c3dc3046be89c`):
  recovered from Docker image `dheiny/openradioss-solver:1.0.37`
  (manifest `sha256:5de9321d…`, layer `sha256:a54673c1…`), which packaged
  the upstream `latest-20260615` release.
- **hm_reader win64** (`hm_reader_win64.dll`,
  SHA-256 `ee7279ed3f5271217a8420358bbea69043470a08a6a9e641ba574f3b5d304eac`):
  extracted from the official `OpenRadioss_win64.zip` of upstream release
  `latest-20260728` (archive SHA-256 `04dfecb0…`), before the upstream
  repository was deleted.
- Package assembled and first published by
  [@AlexanderHuels](https://github.com/AlexanderHuels) as
  `OpenCourant_extlib` release `v67-compat` (2026-10-03); rehosted here
  byte-identical (same SHA-256).

### Reader build versions

The hm_reader binaries embed build IDs that pin their actual vintage.
Since package `v82-hybrid-r2`, both platforms use the v70 reader build:

| Component | Build ID | Source | Effective extlib era |
|---|---|---|---|
| `libhm_reader_linux64.so` | `20260710_d899773e` | community-archived original v70 `extlib.zip` (see release [`upstream-v70`](../../releases/tag/upstream-v70)) | v70 |
| `hm_reader_win64.dll` | `20260710_d899773e` | `latest-20260728` (commit `a62b27e`) | v70 |

Historical: package `v67-compat` and `v82-hybrid` shipped a v60-era Linux
reader (build `20260602_fcb3b29a`, recovered from Docker image
`dheiny/openradioss-solver:1.0.37`).

Consequences:

- Both v70 readers export `cpp_is_part_with_elements`; the legacy part-API
  fallback is no longer needed.
- Both lack `cpp_get_include_file_by_index` (added upstream around extlib
  v76); the include-file listing uses the legacy fallback.
- `/ALE/STRUCTURED_MESH` remains disabled: the v70 readers export
  `cpp_sale_mesh_create`, but a Windows trial with this build produced
  invalid node coordinates and no QA reference exists to validate the
  generated meshes.
- A reader satisfying the current Starter without compatibility options
  would require extlib v76 or later (2026-09-11 onward). Those binaries
  only existed in upstream release assets, which are no longer available.
  If you hold an `extlib.zip` v71-v82 or an `extlib/` folder from a source
  build made between 2026-09-11 and 2026-10-01, please open an issue.

## Upstream git history (branch `upstream-v82`)

The branch [`upstream-v82`](../../tree/upstream-v82) preserves the complete
upstream `OpenRadioss_extlib` git history through the final v82 delivery
(2026-09-22, commit `f2fa2cd00dd5937d96e521f163a191f2dee90148`, 111
commits), recovered from the GitHub fork network before it could be garbage
collected. The surviving fork heads stop at v67; the v67→v82 range adds
rebuilt H3D writer libraries (linux64/linuxa64/win64), updated H3D headers
including the new `h3dpublic_import.h`, and Siemens-copyrighted license
texts. Tags `upstream-v67` and `upstream-v82` mark the two reference
points. Note that hm_reader binaries were never tracked in the upstream git
tree; they were injected from an internal artifact store at release time.

## Validation

With this package, OpenCourant builds Starter and Engine on Linux
(GCC/GFortran 11, double precision) and the full MiniQA suite passes
(81/81). See OpenCourant PR
[#3](https://github.com/OpenCourant/OpenCourant/pull/3) for the
compatibility changes required in the main repository.
