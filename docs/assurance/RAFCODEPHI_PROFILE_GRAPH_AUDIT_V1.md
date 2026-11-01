# RAFCODEPHI profile graph — isolated audit V1

Source: termux-packagesMy main@ee11ed7327ca9c9d91ccd6ee7327ce0827975782.

The five profile metapackages are not reimplementations of external tools. The six federated components remain contracts. Keep individual upstream notices and licenses.

Critical producer distinction: clang and lld are subpackage outputs of libllvm; curl is a subpackage output of libcurl. The RMR vector field static library is installed by packages/rafcodephi-rmr-vector-field/build.sh; a `-static` subpackage is generated conditionally by scripts/build/termux_create_debian_subpackages.sh when the staged `.a` exists and TERMUX_PKG_NO_STATICSPLIT=false. The default is false in scripts/build/termux_step_setup_variables.sh. Directory absence is NOT evidence of absent output; generation logic is NOT proof that a `.deb` was produced.

Required gates: manifest↔recipes equality; dependency DAG and producer resolution; pinned RMR blobs; exact ARM/ARM64 .deb hashes and contents; signed APT custody; Android install/runtime; QEMU ARM32 guest test. The first three are source-scoped, all other stages remain TOKEN_VAZIO until a real receipt.

Observed baseline: batch-pipeline run 41215044878 failed; exact root cause not established. Do not infer success from other successful workflows. No existing production recipes, publisher, workflow, manifest or handoff modified in this audit.

SOURCE ≠ ARTIFACT ≠ EXECUTION ≠ EVIDENCE ≠ CLAIM. claim_allowed=false; release_allowed=false. Rollback: close draft branch; no merge without exact HEAD tests and human review.
