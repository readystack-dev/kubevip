# kube-vip: ReadyStack build kit

The offline build kit for the Docker image `readystack/kubevip:v1.3.2-CE-ubuntu24.04-r1`. It holds what the image is built from (upstream source, base images, toolchain, pinned dependencies) and the documents, so the image can be rebuilt with no network access (`docker build --network=none`).

- Release: [v1.3.2-CE-ubuntu24.04-r1](https://github.com/readystack-dev/kubevip/releases/tag/v1.3.2-CE-ubuntu24.04-r1)
- Image: [`docker.io/readystack/kubevip:v1.3.2-CE-ubuntu24.04-r1`](https://hub.docker.com/r/readystack/kubevip/tags), manifest digest `sha256:fc454604272bb489e4141d002825afcdbf4a43308a2767ec00be3c8a883667d6`
- Product page: https://readystack.dev/products/kubevip
- Source and build recipe: https://readystack.dev/products/kubevip/recipe
- Build trail: https://readystack.dev/products/kubevip/trail/v1.3.2-CE-ubuntu24.04-r1
- Defects: this repository's Issues (defects recorded from 25 November 2026 on) and https://readystack.dev/defects.html?product=kubevip

## The image

A static pod on every control-plane node (/etc/kubernetes/manifests/kube-vip.yaml), written by `kube-vip manifest pod` and shipped in the Kubernetes VM stack's install bundle (install/network/kube-vip.yaml). The image's default user is the unprivileged kubevip (uid 999); the pod sets runAsUser: 0 and adds NET_ADMIN and NET_RAW, because kube-vip adds the address to the node's interface, sends ARP and reads the root-only kubeconfig. Started without those settings it cannot do either.

## Who uses kube-vip and why

Administrators of on-premises, edge and bare-metal Kubernetes clusters who need a highly available control-plane address.

kube-vip gives a Kubernetes cluster a virtual IP address and load balancer for its control plane and for LoadBalancer Services, announced over ARP or BGP, without external load-balancing hardware or software.

A typical use: A team runs Kubernetes on its own servers with several control-plane nodes. kube-vip holds one virtual address for the API server on the elected leader and moves it to another control-plane node when the leader fails, so the worker nodes and administrators keep using the same address.

From the upstream project's own description: https://github.com/kube-vip/kube-vip, https://kube-vip.io/.

## This release

- Docker Scout policy grade at release: **A** (94%), graded 2026-11-20 with `docker scout policy` (CLI 1.26.0, docker-default configuration).
  - Docker Scout counts 7 packages with copyleft licences (GPL, LGPL, MPL), all of them operating-system packages (it counts only packages whose licence it can read; the SBOM lists every component).
- Signed with ReadyStack's release key (fingerprint `04daa92344a52dbc`): https://readystack.dev/keys/cosign.pub.
- Runs as a non-root user (`kubevip`).
- Attestations on the image: an SPDX SBOM and SLSA provenance v1.
- Rebuilt from this kit with network egress blocked on 2026-11-20; the rebuilt image digest is the published one.
- Base: Ubuntu 24.04.

The current grade, measured again every day, is on https://readystack.dev/products/kubevip.

kube-vip is distributed under its upstream licence (Apache-2.0); the kit contains upstream's source under that licence, and each vendored component under its own.

## Download

From the [v1.3.2-CE-ubuntu24.04-r1 release](https://github.com/readystack-dev/kubevip/releases/tag/v1.3.2-CE-ubuntu24.04-r1): `ReadyStack-kubevip-v1.3.2-CE-ubuntu24.04-r1.tar.gz` (444.7 MB), SHA-256 `501e13dbb48826c2d99cca06e927a7fc8518828220bb04ffbf8bbc285dde69e0`.

```sh
sha256sum --check --ignore-missing SHA256SUMS
```

## Verify and rebuild

The rebuild statement is signed with the ReadyStack release key. Check the signature before you rely on it:

```sh
cosign verify-blob --key https://readystack.dev/keys/cosign.pub \
  --bundle ReadyStack-kubevip-v1.3.2-CE-ubuntu24.04-r1.rebuild.json.sigstore ReadyStack-kubevip-v1.3.2-CE-ubuntu24.04-r1.rebuild.json
```

Unpack with `-p` so file modes are kept, then follow the kit's own README.md and docs/REBUILD.md:

```sh
tar -xpzf ReadyStack-kubevip-v1.3.2-CE-ubuntu24.04-r1.tar.gz
cd v1.3.2-CE-ubuntu24.04-r1
less README.md docs/REBUILD.md
```

The build trail (`ReadyStack-kubevip-v1.3.2-CE-ubuntu24.04-r1.trail.json`, signed the same way, and its Markdown copy) records how this release was built and checked.

## This repository

`recipe/` holds the kit's recipe files (Dockerfiles, build scripts, pins) as published at https://readystack.dev/products/kubevip/recipe. The complete kit is the release download. Each release is a tag here.

— ReadyStack · https://readystack.dev
