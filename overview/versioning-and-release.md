# OSB Versioning, Release Policy and GitHub Actions

This document establishes the OSB versioning and release strategy (Trello card
**1.1 — Definir política de versionamento e release do OSB**). It is the
authoritative reference for how OSB versions are declared, validated, branched,
built and published.

## Contents

- [Semantic Versioning](#semantic-versioning)
- [Generation codenames](#generation-codenames)
- [Git Flow branching model](#git-flow-branching-model)
- [Release preparation](#release-preparation)
- [Release publication](#release-publication)
- [Hotfix handling](#hotfix-handling)
- [GitHub Actions workflows](#github-actions-workflows)
- [ISO generation and artifact checksums](#iso-generation-and-artifact-checksums)
- [Cross-repository version coordination](#cross-repository-version-coordination)
- [Authoritative version metadata](#authoritative-version-metadata)

---

## Semantic Versioning

OSB adopts [Semantic Versioning](https://semver.org/):
```
MAJOR.MINOR.PATCH
```
- **MAJOR**: incompatible changes according to the project's compatibility policy.
- **MINOR**: backward-compatible functionality.
- **PATCH**: backward-compatible fixes.

### 0.y.z semantics

While MAJOR is `0` (the current generation), the API is considered **unstable**.
Per SemVer, `0.y.z` MINOR increments may represent incompatible changes, so consumers should pin exact versions during the 0.x generation.
Pre-release versions are denoted by appending a hyphen and dot-separated identifiers immediately after the patch version (e.g., `0.1.0-alpha`). Pre-releases have **lower precedence** than the associated release and are **not** official releases; a build from a pre-release tag must not be published as a stable release.
Build metadata (`+build`) is ignored for precedence.

## Generation codenames

Every MAJOR generation has exactly one codename:
- `0.x.x` — one codename for the entire 0.x generation.
- `1.x.x` — another codename for the entire 1.x generation.
- `2.x.x` — another codename for the entire 2.x generation.

Codenames are names of deities or entities from different mythologies, preferably ASCII-compatible English naming convention.
Generation 0 (current) codename is **Olorun** (Yorùbá mythology — the supreme deity / sky-associated creator). It is final and recorded in every component's `version.json` under `codename`.

## Git Flow branching model

| Branch | Purpose |
|--------|---------|
| `master` | Published and officially released versions only. A merge into `master` triggers the official publication workflow. |
| `develop` | Continuous integration branch and primary development integration point. |
| `feature/*` | New functionality, merged into `develop`. |
| `fix/*` | Ordinary fixes, merged into `develop`. |
| `release/<version>` | Release preparation branch created from `develop`. |
| `hotfix/<description>` | Emergency fixes based on the published version, merged into `master` and `develop`. |

### Normal release process

1. Development work is integrated into `develop`.
2. Create `release/<version>` from `develop`, where `<version>` matches the value declared in `version.json`.
3. Update `version.json` (`version`, `release: true`) on the release branch.
4. Stabilize: run the full build, tests and the release validation (`scripts/validate-version.cake --strict-release`). Once the codename is resolved, the validator checks generation/codename consistency.
5. Merge `release/<version>` into `master` (via PR), then back into `develop`, preserving final adjustments (so release-branch fixes are not lost on `develop`).

Direct merges from `develop` to `master` are not used; releases always flow through a `release/<version>` branch.

## Release preparation

1. Work on `develop` (via `feature/*` / `fix/*` branches merged into `develop`).
2. When ready, create `release/<version>` from `develop`, where `<version>` matches the value declared in `version.json`.
3. On the release branch set `release: true` in `version.json`.
4. Stabilize: run the full build, tests and the release validation (`scripts/validate-version.cake --strict-release`).
5. Merge `release/<version>` into `master` (via PR), then back into `develop`.

No artificial version-synchronization commits; a release branch carries only the version metadata and stabilization changes.

## Release publication

A merge of a release branch into `master` is the official publication trigger. The `release.yml` workflow (in `osb.kernel`) orchestrates it in a single workflow and must:

1. Identify and validate the intended version from `version.json`.
2. Validate the version against the repository's authoritative version metadata (`scripts/validate-version.cake`).
3. Determine the generation codename.
4. Build the required OSB components (kernel + userspace programs).
5. Build the official ISO.
6. Generate release notes and changelog (release manifest).
7. Generate checksums for published artifacts (`sha256sum osb.iso osb.img > osb.iso.sha256`).
8. Create the corresponding Git tag (`v<version>`).
9. Create the GitHub Release titled `OSB <version> — <codename>`.
10. Attach the ISO and other official artifacts.

### Dry-run mode

`scripts/release.sh` supports `DRY_RUN=1 ./scripts/release.sh`, which runs the complete release *preparation* (metadata validation, version validation, codename validation, release-branch/ref validation, deterministic sibling resolution, manifest generation and validation, required-file checks, release-layout preparation, artifact checks) but **never** performs any publication: no tags, no pushes, no GitHub Releases, no asset uploads, no ISO publication, no merge into `master`, and no remote modification. All publication operations (`git tag`, `git push`, `gh release create`) are structurally gated behind the dry-run exit and cannot be reached by `DRY_RUN=1`.

### Branch / origin guards

`release.yml` explicitly rejects any origin other than `refs/heads/master` (push or `workflow_dispatch`) before publication steps run, and `release.sh` re-checks `branch == master` plus `release: true` in `version.json` independently. `workflow_dispatch` therefore cannot bypass branch validation, metadata validation, or release-origin conditions, and creates no alternate publication path. `develop` never publishes.

Idempotency: re-running the workflow never creates duplicate releases or corrupts existing artifacts.
Existing tag/release detection: if the tag already targets the current HEAD it is a no-op; if it targets a different commit, the workflow fails rather than overwriting.
Least-privilege permissions: `contents: write` only (tags + releases); concurrency group serializes publication of the same version (`cancel-in-progress: false`).
No cross-workflow triggering: publication is contained in `release.yml` (not chained from a `GITHUB_TOKEN`-triggered workflow).
Clear failure on inconsistent version metadata, tag or release state.

The official ISO is produced **only** by this release workflow (merge into `master`), not by ordinary `develop` CI.

## Hotfix handling

1. Create `hotfix/<description>` from the tagged commit on `master` (the published version), not from `develop`.
2. Apply the emergency fix, bump `PATCH` in `version.json` (e.g., `0.1.0` → `0.1.1`), and validate.
3. Merge `hotfix/*` into `master` (triggers publication of the patch release **and** into `develop` so the fix is not lost in ongoing development).
4. Tag `v<new-patch-version>` on `master`.

## GitHub Actions workflows

### `osb.kernel/.github/workflows/ci.yml` — CI on `develop`

Triggers on push/PR to `develop`. Runs:
- version validation (`scripts/validate-version.cake`),
- `dotnet build` of the freestanding kernel,
- host unit tests (`dotnet test`),
- kernel ELF link + Multiboot2 + ABI version check.

This workflow must **not** publish a stable release or an official ISO.

### `osb.kernel/.github/workflows/release.yml` — Release on `master`

Triggers on push to `master` and `workflow_dispatch`. Before publication it
explicitly rejects any origin other than `refs/heads/master`, then runs
`scripts/release.sh`:
- validates `version.json`,
- validates the release branch/ref against `version.json`,
- resolves every sibling at an explicit deterministic commit (no default-branch resolution, never `unknown`),
- generates and validates `release-manifest-<version>.json`,
- builds the official ISO,
- generates checksums + release manifest,
- creates/pushes the Git tag idempotently,
- creates the GitHub Release and attaches artifacts.

Permissions: `contents: write`; concurrency group serializes same-version publication (`cancel-in-progress: false`).

`workflow_dispatch` cannot bypass branch validation, metadata validation or release-origin conditions, and creates no alternate publication path; publication remains unauthorized on any branch other than `master`.

### Component CI (`osb.shell`, `osb.core`, `osbkt`, `osb.oslang`)

Each component repo has a `ci.yml` on `develop`/PR that validates its `version.json` (SemVer) and runs `dotnet build` (+ unit tests where applicable).

The shell CI additionally checks out its sibling dependencies (`osbkt`, `osb.core`) side by side, because `Osb.Shell.Userspace.csproj` consumes them by linked sources and fails its `CheckSiblings` target otherwise.


## ISO generation and artifact checksums

The bootable OSB ISO (`osb.iso`) is produced by `osb.kernel/build.sh`:

1. `csc` compiles freestanding C# against `osb.core` MiniCoreLib → managed DLL.
2. `ilc` (NativeAOT) → bare x86-64 object (no BCL, no runtime).
3. `ld.lld` links boot stubs + kernel object → Multiboot2 ELF (`kernel.elf`).
4. `grub-mkrescue` wraps the ELF + ext4 disk image into a bootable ISO (GRUB BIOS).

The ext4 image (drive `A:`) is seeded with `SHELL.EXE` (and ring-3 test programs under `WITH_TESTS=1`) plus PASSWD/SUDOERS/CONF/PROMPT user env slots.

`osb.kernel/build.sh` generates `/OSB/CONF/VERSION.JSON` from all five component `version.json` files via `scripts/generate-metadata.cake` and writes it into the ext4 image. The userspace shell's `VER` command reads this generated artifact at runtime (no hardcoded version table).

The release workflow runs `build.sh` (not develop CI) to produce the official ISO, then:
- `sha256sum osb.iso osb.img > osb.iso.sha256`,
- a `release-manifest-<version>.json` records schema, version, codename, generation, commit and branch plus every component's version + exact source commit. It is deterministic (no wall-clock timestamp) and is validated before publication; any manifest containing `unknown` or missing a component is rejected.

The ISO, image, checksums file and manifest are attached to the GitHub Release.


## Cross-repository version coordination

OSB is a multi-repository ecosystem. Component versions are **independently** versioned (each repo has its own `version.json`); there is no artificial cross-repo version synchronization. The platform release lives in `osb.kernel/version.json`.

The release manifest (generated by `scripts/release.sh`) records, for each component (`osb.kernel`, `osb.shell`, `osbkt`, `osb.core`, `osb.oslang`):
- the component version (from its `version.json`),
- the exact source commit (`git rev-parse HEAD`) used in the build.

This guarantees an ISO is built from compatible, identifiable component revisions and can be reproduced by checking out each component at its recorded commit.

Side-by-side sibling checkouts (`osb.kernel`, `osb.shell`, `osbkt`, `osb.core`, `osb.oslang`) are required by `build.sh` and `release.sh`; the release workflow clones them before building. `osb.oslang` was missing from the clone loop and must be included explicitly.

## Authoritative version metadata

Each OSB component repo declares its version in `version.json` at its root:

```json
{
  "schema": "osb.version.v1",
  "component": "osb.kernel",
  "version": "0.1.0",
  "generation": 0,
  "release": false,
  "codename": "Olorun",
  "repository": "osb-foundation/osb.kernel"
}
```

- `version` is the SemVer component version (single source of truth; no duplicated manual version declarations).
- `generation` is the MAJOR generation (drives codename lookup).
- `codename` is the generation codename (`Olorun` for generation 0).
- `release` is set `true` only on a release branch.

`scripts/validate-version.cake` validates SemVer, `0.y.z` semantics, codename lookup by generation and `release/<version>` → `version.json` consistency, and is invoked by both CI and release workflows.

---

### Repository map

| Module | Repository | Role |
|--------|------------|------|
| `osb.core` | `osb-foundation/osb.core` | Shared freestanding MiniCoreLib |
| `osb.kernel` | `osb-foundation/osb.kernel` | Bootable kernel + platform release version |
| `osb.kernel/scripts/release.sh` | `osb-kernel` | Release publication script; supports `DRY_RUN=1` (safe preparation, no publication) |
| `osb.kernel/scripts/generate-metadata.cake` | `osb-kernel` | Cake metadata generator: `--root=<parent> --output=<file>`; strict validation, deterministic output |
| `osb.kernel/scripts/validate-version.cake` | `osb-kernel` | Cake validator (SemVer, codename registry, strictRelease) |
| `osb.kernel/scripts/stage-ring3.sh` | `osb-kernel` | Ring-3 program staging (compile_rt3, mkosbx) |
| `osb.kernel/scripts/ci.sh` | `osb-kernel` | CI: build, unit tests, link, ABI check |
| `osb.shell/Osb.Shell.Userspace.csproj` | `osb.shell` | Shell EXE userspace; `CheckSiblings` requires `../osbkt` and `../osb.core` |
| `osb.shell/Program.Version.cs` | `osb.shell` | Runtime metadata loader: reads `/OSB/CONF/VERSION.JSON`, feeds `VER` |
| `osb.shell/tests/golden/...` | `osb.shell` | Golden transcript cases (01-ver.out, 99-stress.out, etc.) |
| `osb.core/src/*.cs` | `osb.core` | MiniCoreLib canonical copy |
| `osbkt/mkosbx.py` | `osbkt` | OSBX header wrapper (build-critical; Python migration deferred to a future card) |
| `osbkt/tools/check-abi.sh` | `osbkt` | ABI v8 check |
| `osb.oslang/engine/build.sh` | `osb.oslang` | NativeAOT engine build (M8.7) |
| `osb.oslang/engine/host/Host.Osb/build-oslang.sh` | `osb.oslang` | OSLANG native build |
| `osb.docs/overview/versioning-and-release.md` | `osb.docs` | Documentation (updated) |

---

