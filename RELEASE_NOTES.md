# kube-vip v1.3.2 — ReadyStack release `v1.3.2-CE-ubuntu24.04-r1`

Generated 2026-11-20 from this release's build records and strategy. Statements that come from a single record name it in brackets; the others come from this release's strategy and build log.

## Release

- **Image:** `readystack/kubevip:v1.3.2-CE-ubuntu24.04-r1` (linux/amd64). Index digest `sha256:283bf2072e87e9f460c8475f0bd98c73e2fa55aa1707e77c48aa9bd7ad8e10be` [`docs/provenance/LOCKED_DIGEST.txt`].
- **Image manifest digest:** `sha256:fc454604272bb489e4141d002825afcdbf4a43308a2767ec00be3c8a883667d6` [`TARGET_DIGEST.txt`].
- **Upstream:** kube-vip v1.3.2 built from source at `v1.3.2` of https://github.com/kube-vip/kube-vip — upstream notes: https://github.com/kube-vip/kube-vip/releases/tag/v1.3.2 [`docs/provenance/BUILD_RECIPE.json`].
- **Base image:** none (`FROM scratch`): a chiselled Ubuntu 24.04 runtime, the files Canonical's chisel cut from Ubuntu's own packages (`vendored/chisel/slices.txt`, `vendored/chisel/packages.tsv`); runtime user `kubevip`.
- **Build:** `source-rebuild-go`, compiled offline with `--network=none`.
- **Supersedes:** ReadyStack release `v1.3.1-CE-ubuntu24.04-r3`.

## What changed in this release

- Upstream version v1.3.1 → v1.3.2.
- Minimum dependency versions ReadyStack enforces on top of upstream (raised only where upstream's own version was lower); shipped versions from the SBOM [`docs/sbom/SBOM.json`]:
  - `golang.org/x/net` at least `v0.62.0`: not raised, upstream already pins `v0.62.0` (shipped: `v0.62.0`)
- This release moves kube-vip from 1.3.1 to 1.3.2, a newer release of upstream's 1.3.x line. Upstream's release notes list what changed: https://github.com/kube-vip/kube-vip/releases/tag/v1.3.2

## Security status at release (measured 2026-11-20)

- **Docker Scout:** 0 fixable critical/high vulnerabilities; runs as `kubevip` (non-root); SBOM + provenance attestations present (SPDX SBOM + SLSA v1) [`docs/provenance/SCOUT_VERDICT.json`].
- **OSV cross-check:** no fixable critical/high vulnerability beyond Docker Scout's findings.
- **Signature:** cosign signature verified by ReadyStack's release gate before publishing; check it yourself with `cosign verify --key https://readystack.dev/keys/cosign.pub readystack/kubevip:v1.3.2-CE-ubuntu24.04-r1` (signatures are recorded in the public Sigstore transparency log).
- **ReadyStack release check at release:** passed (ReadyStack's own test, not a Docker Scout grade). Vulnerabilities are disclosed every day and the image never changes, so it is re-checked daily, beside Docker Scout's own dated grade, at https://readystack.dev/queue.

## Reproducibility

Rebuilds offline from this archive to the identical image digest `sha256:fc454604272bb489e4141d002825afcdbf4a43308a2767ec00be3c8a883667d6`: run `./BUILD_OFFLINE.sh`, then `./VERIFY_OFFLINE.sh` (add `--online` to confirm the Docker Hub tag holds the same digest). Before publishing, ReadyStack rebuilt the kit with `./BUILD_OFFLINE.sh` and compared the result with the published digest [`docs/provenance/REPRO_VERIFICATION.json`], then ran `./BUILD_OFFLINE.sh` and `./VERIFY_OFFLINE.sh` from this archive (without `--online`).

## Validation

- 7 of 7 runbook checks passed (7 authored) on the stack's 3-node integration cluster, recorded 2026-10-29T02:34:55Z [`docs/validation/test-results.json`, from the run's record `docs/stack/RUNBOOK_EVIDENCE.json`]. They are the checks of the stack's operator runbook that name this component; no container of this image was started on its own for them. The record does not show this release's image (`sha256:fc454604272bb489e4141d002825afcdbf4a43308a2767ec00be3c8a883667d6`) in that run: it lists the component's images as they were then, an earlier build or a tag, so the checks describe the component, not this digest.
- Failed cases are listed in `docs/validation/FAILED_TESTS.md`.

## Known issues

- None found (reviewed by held semantic review (Claude) on 2026-11-20).

## Defects fixed

- 1 problem found before release and fixed before it shipped (1 by review); each is listed in `docs/defects/defects.json`.

## Verify this archive

```
sha256sum -c CHECKSUMS.sha256
./BUILD_OFFLINE.sh && ./VERIFY_OFFLINE.sh --online
cosign verify --key https://readystack.dev/keys/cosign.pub readystack/kubevip:v1.3.2-CE-ubuntu24.04-r1
```

ReadyStack records every signature in the public Sigstore transparency log (Rekor), so the command needs no extra flags.

