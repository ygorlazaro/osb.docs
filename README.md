# OSB Documentation

Public documentation for **OSB (Operating System Basic)** and **OSLANG**, maintained for the [osbfoundation/osb.docs](https://github.com/osbfoundation/osb.docs) repository.

OSB is a DOS-like operating system built from scratch in C# with NativeAOT (freestanding, no host OS/runtime for the kernel). OSLANG is its scripting language, with its own lexer, parser, compiler, and runtime.

## Start here

- [Overview](overview/README.md) — what OSB is, architecture, repository map, build & run
- [OSLANG](oslang/README.md) — language guide, reference, and specification lineage
- [OS modules](os/README.md) — kernel and operating-system components
- [Tooling](tooling/README.md) — editor/tooling and developer notes
- [Legacy](legacy/README.md) — original BASIC-era material and legacy shell

## Module repositories (source of truth)

| Module | Repository / path | Role |
|---|---|---|
| `osb.core` | `osb.core` | Shared freestanding `MiniCoreLib` runtime stubs |
| `osb.kernel` | `osb.kernel` | Bootable x86-64 kernel (Multiboot2, ring 0/3, scheduler, ext4, networking, GFX) |
| `osbkt` | `osbkt` | Userspace toolkit: `INT 21h` wrappers, `crt0.S`, `user.ld`, `mkosbx.py`, ABI contract |
| `osb.shell` | `osb.shell` | Freestanding userspace shell (`SHELL.EXE`), migrating from legacy in-kernel shell |
| `osb.toolkit` | `osb.toolkit` | Minimal GUI toolkit for freestanding apps |
| `osb.xwin` | `osb.xwin` | Text-mode graphical window manager (ring 3) |
| `osb.oslang` | `osb.oslang` | OSLANG language implementation and language docs/specs |
| `oslang.vscode` | `oslang.vscode` | VS Code extension for OSLANG |
| `osb.legacy` | `osb.legacy` | Original BASIC sources and legacy hosted shell |

This repository curates and organizes documentation; canonical implementation details and evolving contracts live in the module source repositories (notably ABI in `osbkt/docs/ABI.md`, shell migration spec in `osb.shell/docs/MIGRATION.md`, OSLANG specs in `osb.oslang/`).

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).
