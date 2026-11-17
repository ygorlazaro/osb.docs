# Repository map

Projects in the OSB workspace (typically checked out side by side). Each module is the canonical source for its implementation and low-level contracts.

| Project | Path | Summary | Primary docs |
|---|---|---|---|
| **osb.core** | `osb.core/` | Shared freestanding `MiniCoreLib` runtime stubs required by NativeAOT components; enforces no-BCL/no-GC/no-exception-flow constraints. | [os/core](../os/core/README.md), `osb.core/README.md` |
| **osb.kernel** | `osb.kernel/` | Bootable x86-64 kernel: boot/CPU/interrupts, memory & frame allocator, scheduler & per-process CR3, ring 3 + `INT 21h`, ATA/ext4, users/sessions, networking, GFX/input, in-kernel OSLANG. | [os/kernel](../os/kernel/README.md), `osb.kernel/README.md`, `ROADMAP.md` |
| **osbkt** | `osbkt/` | Userspace syscall toolkit and ring-3 build pieces (`Syscall` API, `crt0.S`, `user.ld`, `mkosbx.py`); defines/implements against the ABI contract. | [os/osbkt](../os/osbkt/README.md), `osbkt/docs/ABI.md` |
| **osb.shell** | `osb.shell/` | Freestanding userspace shell (`SHELL.EXE`) replacing the legacy in-kernel shell in phases; behavior specified by migration/golden tests. | [os/shell](../os/shell/README.md), `osb.shell/docs/MIGRATION.md` |
| **osb.toolkit** | `osb.toolkit/` | Minimal GUI toolkit (app lifecycle, controls, desktop/window manager, paint/graphics types) for freestanding programs. | [os/toolkit](../os/toolkit/README.md) |
| **osb.xwin** | `osb.xwin/` | Text-mode graphical window manager/desktop environment running as a ring-3 program on toolkit + kernel services. | [os/xwin](../os/xwin/README.md) |
| **osb.oslang** | `osb.oslang/` | OSLANG language (lexing, parsing, AST, compilation, runtime, extensibility), samples, language docs, and versioned specs (0.1–0.73). | [oslang](../oslang/README.md), `osb.oslang/docs/oslang/`, `OSLANG-*-SPEC.md` |
| **oslang.vscode** | `oslang.vscode/` | VS Code extension: syntax highlighting, IntelliSense, hover/signatures, symbols, folding, snippets for `.osl/.oslang`. | [tooling/oslang-vscode](../tooling/oslang-vscode.md) |
| **osb.legacy** | `osb.legacy/` | Historical archive: original QBasic-era sources (`original-src/`, incl. `OSB.BAS`, XWIN effects) and `legacy-shell/` hosted predecessor. | [legacy](../legacy/README.md) |
| **osb.docs** | `osb.docs/` | This documentation site (GitHub-linked Markdown), published to `osbfoundation/osb.docs`. | [README](../README.md) |

## Dependency/layout notes
- Several builds assume **sibling checkouts** (e.g. kernel/shell/osbkt/osb.core side by side); build scripts fail with clear messages when required siblings are missing.
- `osbkt` and `MiniCoreLib` are commonly consumed as linked sources in freestanding consumers (no runtime DLL dependency in that model).
