# kube-vip: ReadyStack build kit

The offline build kit for the Docker image `readystack/kubevip:v1.3.2-CE-ubuntu24.04-r1`. It holds what the image is built from (upstream source, base images, toolchain, pinned dependencies) and the documents, so the image can be rebuilt with no network access (`docker build --network=none`).

- Release: [v1.3.2-CE-ubuntu24.04-r1](https://github.com/readystack-dev/kubevip/releases/tag/v1.3.2-CE-ubuntu24.04-r1)
- Image: [`docker.io/readystack/kubevip:v1.3.2-CE-ubuntu24.04-r1`](https://hub.docker.com/r/readystack/kubevip/tags), manifest digest `sha256:fc454604272bb489e4141d002825afcdbf4a43308a2767ec00be3c8a883667d6`
- Product page: https://readystack.dev/products/kubevip
- Defects: this repository's Issues (defects recorded from 25 November 2026 on) and https://readystack.dev/defects.html?product=kubevip

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
