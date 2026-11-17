# Overview

OSB (Operating System Basic) is a from-scratch, DOS-like operating system project written in C# and compiled with NativeAOT for a freestanding environment (no host OS, no GC/BCL runtime assumptions in the kernel/userspace core). It pairs a bootable kernel, userspace components, and **OSLANG**, a scripting language designed for the platform.

## In this section
- [Architecture](architecture.md) — system layering and module responsibilities
- [Repository map](repository-map.md) — projects in the OSB workspace and their roles
- [Building and running](building-and-running.md) — prerequisites, build pipeline, and QEMU usage

## Key characteristics
- **DOS-like model:** drive letters (e.g. `A:`), `\` paths, internal/external command model, `INT 21h`-style syscall ABI.
- **Freestanding C# stack:** `osb.core` provides `MiniCoreLib`; kernel/userspace code targets fixed buffers and explicit constraints (no GC allocations, no exception-based normal control flow in core paths).
- **Evolving system:** kernel and shell capabilities are delivered in slices; consult module docs for current status (e.g. ext4 read support now, write planned; userspace shell migration in progress).

See [OS modules](../os/README.md) and [OSLANG](../oslang/README.md).
