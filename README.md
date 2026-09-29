# layer-kubernetes

The Kubernetes client toolchain — `kubectl`, `helm`, and `k3d` — at stable
`/usr/bin` paths, as a standalone OpenCharly layer repo.

The candy installs the three core Kubernetes client tools uniformly across every
distro: `kubectl` (the API client — the distro package on Arch/Fedora, or the
official `dl.k8s.io` release mirror as a fallback binary), `helm` (the chart
package manager — the distro package or the upstream `get-helm-3` script), and
`k3d` (the k3d-in-docker cluster manager, always a standalone release binary).
Each lands at a predictable `/usr/bin` path and answers a version query, so any
box composing this candy can drive a cluster with no further setup.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `kubernetes` |
| Pinned versions | `K3D_VERSION` `v5.8.3`, `KUBECTL_VERSION` `v1.30.5` |
| Binaries | `/usr/bin/kubectl`, `/usr/bin/helm`, `/usr/bin/k3d` |
| Service / port | none |

## How to use it

Compose the layer as a nested `candy:` list inside a named box body:

```yaml
k8s-client-box:
  candy:
    base: fedora
    candy:
      - '@github.com/opencharly/layer-kubernetes:v2026.241.0654'
```

The `plan:` `check:` steps assert that each binary exists at its path and reports
its version, so a box composing this layer is directly verifiable from a fresh
build with no service or deploy state.

## Layout

- `charly.yml` — the `kubernetes:` candy entity (the pinned version vars, the
  `download:`/`command:` plan steps, and the `check:` assertions).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Family skill: `/charly-coder:kubernetes-layer` — the Kubernetes client-tools
  reference (kubectl + Helm). This repo declares no `skill:` entity; the gap is
  tracked in [`opencharly/opencharly#291`](https://github.com/opencharly/opencharly/issues/291).
- `/charly-kubernetes:check-k8s` — the `kube:` cluster-probe check verb.
- `/charly-image:layer` — candy authoring reference.
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
