# RAFCODEΦ — Architecture, dependencies and evidence V1

**State:** isolated SOURCE-SCOPED gate, DRAFT/HOLD; `claim_allowed=false` / `release_allowed=false`.

**Observed current source:** `rafaelmeloreisnovo/termux-packagesMy` main `ee11ed7327ca9c9d91ccd6ee7327ce0827975782` (2026-11-01). The upstream/source lineage recorded in `rafcodephi-federated-components.v1.json` points to `rafaelmeloreisnovo/termux-packages` at its historical commit and MUST NOT be misread as the present My-fork HEAD.

## Structural dependency DAG

```text
termux-packagesMy (source + recipes, NOT Android runtime)
  rafcodephi-bootstrap-profile
    ├─ apt bash busybox dpkg ca-certificates coreutils termux-tools
    └─ rafcodephi-federation-metadata
        ↓
  rafcodephi-runtime-profile
    ├─ openssl curl git python procps
    └─ b3sum (official upstream recipe)
        ↓
  rafcodephi-build-profile
    ├─ clang lld cmake make ninja binutils file patchelf
    ├─ rafcodephi-rmr-vector-field (GPL-2.0-only)
    └─ rafcodephi-rmr-vector-field-static (GENERATED CONDITIONALLY)
        ↓
  rafcodephi-vectras-profile
    └─ aria2 tar pulseaudio proot proot-distro
        ↓
  rafcodephi-full-profile
    └─ termux-api (Termux CLI, Android companion stays separate)
```

The arrow means the next profile depends on the preceding profile; it is not a source-code copy. Output names need not equal producing recipe names:

| Output package | Actual producing source | Evidence state |
| --- | --- | --- |
| `clang`, `lld` | `packages/libllvm/{clang,lld}.subpackage.sh` | source definition present |
| `curl` | `packages/libcurl/curl.subpackage.sh` | source definition present |
| `rafcodephi-rmr-vector-field` | `packages/rafcodephi-rmr-vector-field/build.sh` | source recipe present |
| `rafcodephi-rmr-vector-field-static` | `scripts/build/termux_create_debian_subpackages.sh` + RMR parent recipe | conditional producer, not proven .deb |
| `qemu-system-x86-64-headless` | official QEMU release recipe, separate from Vectras profile dependencies | ARM32 x86_64 guest runtime still TOKEN_VAZIO |

The `-static` generator requires `TERMUX_PKG_NO_STATICSPLIT=false` and at least one staged `.a`; the setting defaults false in `termux_step_setup_variables.sh`. The RMR recipe installs `librafcodephi-rmr-vector-field.a`. **The absence of a separate `-static/build.sh` file is not evidence that a subpackage cannot be produced.** Building and recording the actual `.deb` remains necessary before claiming availability.

## Federation and absent runtime proof

| External project | Mode / boundary | Missing proof |
| --- | --- | --- |
| termux-app-rafcodephi | consumer_app; installed Android application | exact .deb → bootstrap → APK → installed prefix |
| BLAKE3 reference fork | official-upstream b3sum, not local fork transplant | consumed .deb bytes/source digest |
| Vectras-VM-Android | android_backend_contract | app/service invocation and permission validation |
| qemu_rafaelia | official QEMU package | host ARM32, guest x86_64 boot, resource/performance receipt |
| androidx_RmR | android_build_dependency_contract | exact module and upstream origin |
| termux-api_rafcodephi | CLI package + separate Android companion | IPC/permissions/device runtime |

## Source → artifact → execution → evidence → claim

```text
manifest + source recipes + pinned RMR blobs
   ↓ stdlib structural DAG validator (cheap, no package script execution)
SOURCE_SCOPE_PASS or BLOCKED
   ↓ produce exact ARM + AArch64 package source closure
.deb outputs + SHA-256 + producer mapping + custody
   ↓ signed APT identity / prefix / key + artifact hashes
bootstrap ZIP pair + APP consumer import/verification
   ↓ physical Android APK install / CLI execution
DEVICE_RECEIPT or TOKEN_VAZIO
   ↓ independent review + rights + security gates
claim consideration only; never implied by a file or CI name
```

Graph validation does not run `dpkg`, source downloading, shell recipes, QEMU, Java, ART, GPU, Docker or a phone; Python is a **build-time hosted validator**, not a runtime dependency or claimed C freestanding implementation.

## Reproducible commands

```bash
python3 scripts/tests/test_rafcodephi_profile_architecture.py
python3 scripts/validate_rafcodephi_profile_architecture.py \
  --root . --source-sha "$(git rev-parse HEAD)" \
  --out build/reports/rafcodephi-profile-architecture.json
```

The test suite includes drift, duplicate output, graph cycle, missing source, static opt-out, generator change, source-blob tampering, forged distribution claim, unknown AndroidX attribution and QEMU false-device-claim falsifiers. Passing synthetic fixtures alone is **not** current-source PASS. An exact-head GitHub workflow runs both tests and the existing federation validator without publication or build.

## Risk and rollback

The observed baseline run `41215044878` of batch candidate pipeline had conclusion FAILURE and returned no jobs in the available run-jobs query. Its actual root cause is unresolved and is **not** silently repaired or ignored by this scope.

No edits to existing recipes, five profile dependencies, central federation manifest, old publisher, consumer app, credentials, or upstream source. To roll back, close the draft PR and discard only its newly added files. License rules remain path-aware: Apache-2.0 for appropriately licensed glue; GPL-2.0-only RMR C source; all other upstream licenses retained.

## Acceptance and next independent falsifier

- Source profile/manifest consistency; no cycles/ambiguous producer.
- RMR copies match three pinned Git blob IDs and GPL/attribution remains explicit.
- No physical or .deb claim if only generator logic was inspected.
- Exact HEAD hosted CI must be GREEN before considering any review promotion.
- Independent concrete `rafcodephi-rmr-vector-field-static*.deb` must be built, examined with `dpkg-deb`, hashed and linked to a custody receipt. Then two-ABI source closure, signed APT and Android device gates can be considered.

R3: `F_ok=structural source/producer contracts`; `F_gap=artifact/CI/device/security/license`; `F_next=exact-head CI and real conditional static .deb proof`.
