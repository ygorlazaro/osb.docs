# osbkt (userspace toolkit)

Static/userspace toolkit providing a C# API over the OSB kernel `INT 21h` syscalls, plus the startup/link/wrapping pieces needed to build ring-3 programs.

## Contents
- **`Syscall.cs`** — syscall wrappers (console I/O, process control, memory, filesystem read surface, environment/args; extends with ABI)
- **`crt0.S`** — ring-3 entry (`_start`), `INT 21h` shim, basic memory routines
- **`user.ld`** — linker script for ring-3 images (user base)
- **`mkosbx.py`** — wraps flat binaries into OSBX images expected by the kernel loader
- **`docs/ABI.md`** — authoritative ring-3 ABI contract

## Usage model
- Consumers (e.g. `osb.shell`, `osb.xwin`) call the **public osbkt API**; syscall numbers/register layout are encapsulated in osbkt.
- Freestanding constraints apply: no BCL/GC, fixed buffers, explicit conventions.
- Programs are linked against osbkt pieces and `osb.core` `MiniCoreLib` as linked sources in the freestanding module model.

## Build/layout
```bash
dotnet build
```
Sibling-repo layout is required by dependent builds; see module READMEs.

## Documentation
- [ABI contract](abi.md)

## References
- Source: `osbkt/README.md`, `osbkt/docs/ABI.md`, `osbkt/Syscall.cs`
