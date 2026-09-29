# AGENTS.md — layer-kubernetes

Standalone candy repo for the `kubernetes` layer — the `kubectl` / `helm` / `k3d`
client toolchain at stable `/usr/bin` paths. The candy lives in `charly.yml` at
the repo root: the pinned `K3D_VERSION` / `KUBECTL_VERSION` vars, the
`download:`/`command:` plan steps, and the `check:` assertions.

The repo declares **no `skill:` entity**; the owning guidance is the family skill
`/charly-coder:kubernetes-layer` (the gap is tracked in
`opencharly/opencharly#291`).

Canonical files:

- `charly.yml` — the `kubernetes:` candy entity.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-coder:kubernetes-layer` — the owning (family) skill. The kubectl +
  Helm client-tool reference. Load before editing or troubleshooting the layer.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `download:`/`check:`, per-distro `distro:` arms,
  package/repo sections, and service declarations). Load before editing any
  entity field or plan step.

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- The candy's `plan:` `check:` steps are the functional evidence; they must stay
  valid on every distro arm they run on. Scope a distro-specific check in the
  command itself — the check runner does not honour runner-level
  `exclude-distro` fields.
- Keep the pinned `K3D_VERSION` / `KUBECTL_VERSION` vars and the `check:`
  assertions in sync; a version bump must update both.

## Modify this repo

- The `kubectl` download step is guarded by `unless_exists` so a distro package
  wins; preserve that precedence when changing the plan.
- New behaviour claims belong in the `plan:` as an observable `check:` step.
- This repo has no `skill:` entity, so there is no projected corpus to mirror.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
