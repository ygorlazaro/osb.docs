# Contributing to OSB Documentation

## Scope
This repository hosts public, GitHub-rendered Markdown documentation for OSB and OSLANG, intended for publication at `osbfoundation/osb.docs`.

## Principles
- **Markdown-first:** use relative links and per-section `README.md` indexes so docs render natively on GitHub (no build step required).
- **Source of truth:** do not duplicate volatile contracts verbatim without attribution. Link to or clearly reference canonical sources:
  - Ring-3 ABI: `osbkt/docs/ABI.md`
  - Shell migration/behavior spec: `osb.shell/docs/MIGRATION.md`
  - OSLANG specs and language docs: `osb.oslang/OSLANG-*-SPEC.md` and `osb.oslang/docs/oslang/`
  - Kernel roadmap/notes: `osb.kernel/ROADMAP.md`, `osb.kernel/OSB_BAS_NOTES.md`
- **Status-aware:** OSB is evolving. Mark implemented vs planned/limited behavior (e.g. ext4 read vs write, shell migration phase, userspace ABI version).
- **DOS-like & freestanding context:** preserve OSB terminology (drive letters, `\`, `INT 21h`, freestanding/NativeAOT constraints) and avoid implying Unix/POSIX semantics unless documented.

## Structure
- `overview/` — project overview, architecture, repo map, build/run
- `oslang/` — language getting started, guide, reference, specs, samples
- `os/` — per-module OS documentation (core, kernel, osbkt, shell, toolkit, xwin)
- `tooling/` — editor/tooling and developer workflows
- `legacy/` — historical material

## Editing
- Keep pages focused; prefer one topic per file and link related pages.
- Use fenced code blocks for commands, paths, and code.
- When updating behavior or ABI details, verify against the corresponding module repository and note the source/version where relevant.

## Submitting changes
Open a pull request against `osbfoundation/osb.docs` with a clear summary of what changed and which source material it reflects.
