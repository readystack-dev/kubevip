# GENERATED L-009 hermetic build (--network=none, SBOM+SLSA-attestation-producing).
# Pinned bases from BASE_IMAGE_DIGESTS.lock; vendored Go tree + debs from Dockerfile.vendor.
FROM golang:1.27.4-bookworm@sha256:4fa7c968ba93f1635670522dbb01a2c78150443e1385746924566079f36570ea AS build_kube-vip
WORKDIR /src
COPY vendored/kubevip-src/ .
ENV CGO_ENABLED=0 GOTOOLCHAIN=local GOWORK=off GOFLAGS="-mod=vendor -buildvcs=false" GOPROXY=off SOURCE_DATE_EPOCH=1795200741
RUN go build -trimpath -ldflags='-s -w -X=main.Version=v1.3.2 -X=main.Build=af00db1f742b88475d1f145ba226c6cd025d2895 -extldflags -static' -o /kube-vip . && /kube-vip version > /tmp/kube-vip.version 2>&1; cat /tmp/kube-vip.version; grep -qF 'v1.3.2' /tmp/kube-vip.version || { echo 'kube-vip version does not report v1.3.2'; exit 1; } && \
    { readelf -lW /kube-vip | sed -n 's/.*Requesting program interpreter: \(.*\)]$/\1/p'; readelf -dW /kube-vip | sed -n 's/.*Shared library: \[\(.*\)\]$/\1/p'; } > /kube-vip.needs
# rs-probe: the HEALTHCHECK helper (standard library only, from the kit's rs-probe.go)
FROM golang:1.27.4-bookworm@sha256:4fa7c968ba93f1635670522dbb01a2c78150443e1385746924566079f36570ea AS build_rs_probe
WORKDIR /probe
COPY rs-probe.go main.go
COPY rs-probe.mod go.mod
ENV CGO_ENABLED=0 GOTOOLCHAIN=local GOFLAGS="-buildvcs=false" GOPROXY=off SOURCE_DATE_EPOCH=1795200741
RUN go build -trimpath -ldflags='-s -w' -o /rs-probe .
# the chiselled rootfs (vendored/chisel, cut by Dockerfile.vendor), the service user, the binaries
FROM ubuntu:24.04@sha256:9c7f200c07e3d4a03e7249d2c16bf66a51120fb876aa2242cf31451412543289 AS rootfs
ARG SOURCE_DATE_EPOCH=1795200741
ADD vendored/chisel/rootfs.tar /rootfs/
RUN printf 'kubevip:x:999:999::/var/lib/kubevip:/bin/sh\n' >> /rootfs/etc/passwd && printf 'kubevip:x:999:\n' >> /rootfs/etc/group && \
    mkdir -p /rootfs/var/lib/kubevip && chown -R 999:999 /rootfs/var/lib/kubevip
COPY --from=build_kube-vip /kube-vip /rootfs/kube-vip
COPY --from=build_kube-vip /kube-vip.needs /tmp/needs/kube-vip
COPY --from=build_rs_probe /rs-probe /rootfs/usr/local/bin/rs-probe
RUN chmod 755 /rootfs/kube-vip /rootfs/usr/local/bin/rs-probe
RUN --mount=type=bind,source=vendored/chisel,target=/chisel set -eu; \
    for n in /tmp/needs/*; do while read -r lib; do [ -n "$lib" ] || continue; \
      case "$lib" in /*) [ -e "/rootfs$lib" ] ;; *) [ -n "$(find /rootfs/usr/lib /rootfs/usr/lib64 -name "$lib" 2>/dev/null | head -1)" ] ;; esac || \
      { echo "$(basename "$n") needs $lib, which the chiselled rootfs does not have: add its slice to runtime_base.slices" >&2; exit 1; }; \
    done < "$n"; done; rm -rf /tmp/needs; \
    [ -e /rootfs/etc/os-release ] || { echo "no /etc/os-release: base-files_release-info is missing" >&2; exit 1; }; \
    [ "$(grep -c '^Package: ' /rootfs/var/lib/dpkg/status)" = "$(wc -l < /chisel/packages.tsv)" ] || \
      { echo "/var/lib/dpkg/status does not list every package of vendored/chisel/packages.tsv" >&2; exit 1; }; \
    rm -rf /rootfs/var/log/dpkg.log /rootfs/var/log/alternatives.log /rootfs/var/log/apt /rootfs/var/cache/ldconfig/aux-cache /rootfs/var/cache/fontconfig
FROM scratch
ARG SOURCE_DATE_EPOCH=1795200741
COPY --from=rootfs /rootfs/ /
ENV PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
LABEL org.opencontainers.image.licenses="Apache-2.0"
LABEL org.opencontainers.image.title="kube-vip"
LABEL org.opencontainers.image.source="https://github.com/kube-vip/kube-vip"
LABEL org.opencontainers.image.version="v1.3.2-CE-ubuntu24.04-r1"
LABEL org.opencontainers.image.revision="af00db1f742b88475d1f145ba226c6cd025d2895"
# image_healthcheck false: no HEALTHCHECK (a Kubernetes pod component; the kubelet ignores one)
USER kubevip
ENTRYPOINT ["/kube-vip"]
