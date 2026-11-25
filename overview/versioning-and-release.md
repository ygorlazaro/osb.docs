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

- **MAJOR**: incompatible changes according to the project's compatibility
  policy.
- **MINOR**: backward-compatible functionality.
- **PATCH**: backward-compatible fixes.

### `0.y.z` semantics

While MAJOR is `0` (the current generation), the API is considered **unstable**.
Per SemVer, `0.y.z` MINOR increments may represent incompatible changes, so
consumers should pin exact versions during the 0.x generation.

### Pre-release versions

Pre-release versions are denoted by appending a hyphen and dot-separated
identifiers immediately after the patch version:

```
MAJOR.MINOR.PATCH-<prerelease>
```

Examples:
- `0.1.0-alpha` — early development preview.
- `0.1.0-beta` — feature-complete beta.
- `0.1.0-rc.1` — release candidate.

Pre-release versions have **lower precedence** than the associated release
(e.g. `0.1.0-rc.1 < 0.1.0`) and are **not** official releases. A build from a
pre-release tag must not be published as a stable release.

Build metadata (`+build`) is ignored for precedence.

## Generation codenames

Every MAJOR generation has exactly one codename:

- `0.x.x` — one codename for the entire 0.x generation.
- `1.x.x` — another codename for the entire 1.x generation.
- `2.x.x` — another codename for the entire 2.x generation.

Codenames are names of deities or entities from different mythologies, preferably
associated with the purpose or character of the generation. Names used in code,
tags and technical metadata follow an ASCII-compatible English naming convention.

### Generation 0 (current)

The generation 0 codename is **pending an explicit final decision**. The current
candidates are:

| Candidate | Origin | Notes |
|-----------|--------|-------|
| `Olorun` | Yorùbá mythology — the supreme deity / sky-associated creator | Currently the preferred candidate; decision not yet final. |
| `Ptah` | Egyptian mythology — creator deity, patron of craftsmen and builders | Alternative candidate. |

**Do not hardcode either candidate as definitive.** The codename is recorded in
`osb.kernel/version.json` under `codename` (currently `null`) and must be set
explicitly when the decision is made. `codenameCandidates` documents the
options. Until a decision is recorded, releases identify the version only and
state that the codename is pending.

## Git Flow branching model

OSB uses [Git Flow](https://nvie.com/posts/a-successful-git-branching-model/).
The branches have these responsibilities:

| Branch | Purpose |
|--------|---------|
| `master` | Published and officially released versions only. A merge into `master` triggers the official publication workflow. |
| `develop` | Continuous integration branch and primary development integration point. |
| `feature/*` | New functionality, merged into `develop`. |
| `fix/*` | Ordinary fixes, merged into `develop`. |
| `release/<version>` | Release preparation branch created from `develop`. |
| `hotfix/*` | Emergency fixes based on the published version. |

### Normal release process

1. Development work is integrated into `develop`.
2. Create `release/<version>` from `develop`.
3. Stabilize and validate the release branch.
4. Merge the release branch into `master`.
5. Merge the release branch back into `develop`, preserving final release
   adjustments (so release-branch fixes are not lost on `develop`).
6. The merge into `master` triggers the official publication workflow.

Direct merges from `develop` to `master` are not used; releases always flow
through a `release/<version>` branch.

## Release preparation

1. Work on `develop` (via `feature/*` / `fix/*` branches merged into `develop`).
2. When ready, create `release/<version>` from `develop`, where `<version>`
   matches the value declared in `version.json`.
3. Update `version.json` (`version`, `release: true`) on the release branch.
4. Stabilize: run the full build, tests and the release validation
   (`scripts/validate-version.py --strict-release` once the codename is set).
5. Merge `release/<version>` into `master` (via PR), then back into `develop`.

Do not introduce artificial version-synchronization commits; a release branch
carries only the version metadata and stabilization changes.

## Release publication

A merge of a release branch into `master` is the official publication trigger.
The `release.yml` workflow (in `osb.kernel`) orchestrates publication in a
single workflow and must:

1. Identify and validate the intended version from `version.json`.
2. Validate the version against the repository's authoritative version metadata
   (`scripts/validate-version.py`).
3. Determine the generation codename.
4. Build the required OSB components (kernel + userspace programs).
5. Build the official ISO.
6. Generate release notes and changelog (release manifest).
7. Generate checksums for published artifacts (`sha256sum`).
8. Create the corresponding Git tag (`v<version>`).
9. Create the GitHub Release titled `OSB <version> — <codename>`.
10. Attach the ISO and other official artifacts.
11. Record sufficient metadata to identify the source commit and build
    (release manifest: commit, branch, component versions + commits).

### Publication guarantees

- **Idempotent:** re-running the workflow never creates duplicate releases or
  corrupts existing artifacts.
- **Existing tag/release detection:** if the tag already targets the current
  HEAD it is a no-op; if it targets a different commit, the workflow fails
  rather than overwriting.
- **Least-privilege permissions:** `contents: write` only (tags + releases).
- **No secrets in logs.**
- **Concurrency control:** a concurrency group serializes publication of the
  same version.
- **No cross-workflow triggering:** publication is contained in `release.yml`
  (not chained from a `GITHUB_TOKEN`-triggered workflow).
- **Clear failure** on inconsistent version metadata, tag or release state.

The official ISO is produced **only** by this release workflow (merge into
`master`), not by ordinary `develop` CI.

## Hotfix handling

1. Create `hotfix/<description>` from the tagged commit on `master` (the
   published version), not from `develop`.
2. Apply the emergency fix, bump `PATCH` in `version.json` (e.g. `0.1.0` →
   `0.1.1`), and validate.
3. Merge `hotfix/*` into `master` (triggers publication of the patch release)
   **and** into `develop` so the fix is not lost in ongoing development.
4. Tag `v<new-patch-version>` on `master`.

## GitHub Actions workflows

### `osb.kernel/.github/workflows/ci.yml` — CI on `develop`

Triggers on push/PR to `develop`. Runs:
- version validation (`scripts/validate-version.py`),
- `dotnet build` of the freestanding kernel,
- host unit tests (`dotnet test`),
- kernel ELF link + Multiboot2 + ABI version check.

This workflow must **not** publish a stable release or an official ISO.

### `osb.kernel/.github/workflows/release.yml` — Release on `master`

Triggers on push to `master`. Runs `scripts/release.sh`:
- validates `version.json`,
- builds the official ISO,
- generates checksums + release manifest,
- creates/pushes the Git tag idempotently,
- creates the GitHub Release and attaches artifacts.

Permissions: `contents: write`. Concurrency group serializes same-version
publication (`cancel-in-progress: false`).

### Component CI (`osb.shell`, `osb.core`, `osbkt`, `osb.oslang`)

Each component repo has a `ci.yml` on `develop`/PR that validates its
`version.json` (SemVer) and runs `dotnet build` (+ unit tests where
applicable).

## ISO generation and artifact checksums

The bootable OSB ISO (`osb.iso`) is produced by `osb.kernel/build.sh`:

1. `csc` compiles freestanding C# against `osb.core` MiniCoreLib → managed DLL.
2. `ilc` (NativeAOT) → bare x86-64 object (no BCL, no runtime).
3. `ld.lld` links boot stubs + kernel object → Multiboot2 ELF (`kernel.elf`).
4. `grub-mkrescue` wraps the ELF + ext4 disk image into a bootable ISO (GRUB BIOS).

The ext4 image (drive `A:`) is seeded with `SHELL.EXE` (and ring-3 test
programs under `WITH_TESTS=1`) plus PASSWD/SUDOERS/CONF/BOOT.

The release workflow runs `build.sh` (not develop CI) to produce the official
ISO, then:

- `sha256sum osb.iso osb.img > osb.iso.sha256`
- a `release-manifest-<version>.json` records version, codename, generation,
  commit, branch, build timestamp, and every component's version + exact commit.
- The ISO, image, checksums file and manifest are attached to the GitHub
  Release, making a release traceable to the exact source revisions used to
  build it.

## Cross-repository version coordination

OSB is a multi-repository ecosystem. Component versions are **independently
versioned** (each repo has its own `version.json`); there is no artificial
cross-repo version synchronization. The platform release lives in
`osb.kernel/version.json`.

The release manifest (generated by `scripts/release.sh`) records, for each
component (`osb.kernel`, `osb.shell`, `osbkt`, `osb.core`, `osb.oslang`):
- the component version (from its `version.json`),
- the exact source commit (`git rev-parse HEAD`) used in the build.

This guarantees an ISO is built from compatible, identifiable component
revisions and can be reproduced by checking out each component at its recorded
commit.

Side-by-side sibling checkouts (`osb.kernel`, `osb.shell`, `osbkt`,
`osb.core`) are required by `build.sh`; the release workflow clones them before
building.

## Authoritative version metadata

Each OSB component repo declares its version in `version.json` at its root:

```json
{
  "schema": "osb.version.v1",
  "component": "osb.kernel",
  "version": "0.1.0",
  "generation": 0,
  "release": false,
  "codename": null,
  "codenameCandidates": ["Olorun", "Ptah"],
  "repository": "osb-foundation/osb.kernel"
}
```

- `version` is the SemVer component version (single source of truth; no
  duplicated manual version declarations).
- `generation` is the MAJOR generation (drives codename lookup).
- `codename` is the generation codename (`null` until explicitly decided).
- `release` is set `true` only on a release branch.

`scripts/validate-version.py` validates SemVer, `0.y.z` semantics, codename
lookup by generation and `release/<version>` → `version.json` consistency, and
is invoked by both CI and release workflows.

---

### Repository map

| Module | Repository | Role |
|--------|------------|------|
| `osb.core` | `osb-foundation/osb.core` | Shared freestanding MiniCoreLib |
| `osb.kernel` | `osb-foundation/osb.kernel` | Bootable kernel + platform release version |
| `osbkt` | `osb-foundation/osbkt` | Ring-3 toolkit + INT 21h ABI contract |
| `osb.shell` | `osb-foundation/osb.shell` | Userspace shell (SHELL.EXE) |
| `osb.oslang` | `osb-foundation/osb.oslang` | OSLANG language (spec versions separate) |
