# Provenance — kube-vip (`v1.3.2-CE-ubuntu24.04-r1`)

Every item below is copied from a record in this archive; the record is named in brackets.

## Source

- Upstream repository: https://github.com/kube-vip/kube-vip
- Revision built: `v1.3.2` (commit `af00db1f742b88475d1f145ba226c6cd025d2895`)
- Source tree: `f2f210a18b5c18dd910d4e4dfe71f9843501b61f60a45e858db453affe58dac3` over 318 files [`docs/provenance/SOURCE_TREE_HASH.json`]
- Community Edition only: enterprise code is removed or refused before the build (CE audit at acquire).

## Build

- Method: `source-rebuild-go`; base image `scratch + a chiselled Ubuntu 24.04 runtime (vendored/chisel/slices.txt, packages.tsv)`
- Compiled with `--network=none` from vendored source, dependencies and base images; BuildKit pinned to `moby/buildkit:v0.31.2` (shipped in `toolchain/`).
- SOURCE_DATE_EPOCH: `1795200741` [`SOURCE_DATE_EPOCH.txt`]
- Frozen inputs: [`docs/provenance/FREEZE_MANIFEST.json`], build recipe: [`docs/provenance/BUILD_RECIPE.json`].

## Output

- Published image: `readystack/kubevip:v1.3.2-CE-ubuntu24.04-r1` — index digest `sha256:283bf2072e87e9f460c8475f0bd98c73e2fa55aa1707e77c48aa9bd7ad8e10be` [`docs/provenance/LOCKED_DIGEST.txt`]
- linux/amd64 image digest `sha256:fc454604272bb489e4141d002825afcdbf4a43308a2767ec00be3c8a883667d6` — reproduced from this archive before publishing [`docs/provenance/REPRO_VERIFICATION.json`].

## Attestations and signature

- SBOM + SLSA provenance attached to the image: attached: an SPDX SBOM and SLSA v1 provenance [`docs/provenance/SCOUT_VERDICT.json`]
- cosign signature by the ReadyStack release key: verified before publishing. Check it: `cosign verify --key https://readystack.dev/keys/cosign.pub readystack/kubevip:v1.3.2-CE-ubuntu24.04-r1`

## Pinned inputs, and why

- 21 pinned inputs, each with where it came from, why this one, how it was checked and when it was decided: `docs/PINS.md` [`docs/provenance/PINS.json`] (upstream source 2, base and build images 3, image digests 2, dependency versions readystack sets 1, pins in the kit's dockerfiles 6, readystack's build engine 5, dependencies upstream chose 2).

## Build trail

- How this release passed through the ReadyStack harness, record by record (every chain, drive, stage, review round, defect and check, each naming the record it is copied from): `ReadyStack-kubevip-v1.3.2-CE-ubuntu24.04-r1.trail.json`, published beside this archive and signed with the release key (`ReadyStack-kubevip-v1.3.2-CE-ubuntu24.04-r1.trail.json.sigstore`), and readable at https://readystack.dev/products/kubevip/trail/v1.3.2-CE-ubuntu24.04-r1.

