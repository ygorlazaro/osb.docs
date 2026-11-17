# osb.core

Shared minimal runtime library for freestanding OSB NativeAOT projects.

## Purpose
- Provides `MiniCoreLib` (minimal type/runtime stubs) required by the compiler/runtime in a no-OS environment.
- Enforces freestanding constraints across consumers: no BCL convenience, no GC allocations, no exceptions for normal control flow, fixed/stack buffers where appropriate.

## Contents
- `src/MiniCoreLib.cs` — canonical minimal core library linked/used by OSB components (kernel, shell, toolkit, xwin, osbkt consumers as applicable).

## Usage
Other OSB projects reference or include `MiniCoreLib.cs` to satisfy freestanding compilation requirements. Keep it minimal and consistent with the kernel/runtime boundary.

## Build
```bash
dotnet build
```

## References
- Source: `osb.core/README.md`, `osb.core/src/MiniCoreLib.cs`
- [Architecture](../../overview/architecture.md)
