# XWin (osb.xwin)

Text-mode graphical window manager for OSB, inspired by classic desktop environments, running as a freestanding NativeAOT ring-3 program.

## Features
- Window management (create/move/resize/manage windows)
- Desktop environment elements (taskbar/deskbar, icons, menus) with a Haiku-style layout influence
- Rendering over kernel graphics services/backends exposed to userspace
- Mouse and keyboard interaction via OSB input facilities (e.g. SGR/console and kernel GFX/input syscalls as applicable)

## Architecture
- **`osb.toolkit`** — GUI framework primitives
- **`osb.core`** — freestanding runtime (`MiniCoreLib`)
- **Kernel** — display/input and system services via `INT 21h`

## Build
```bash
./build.sh
```
Produces a binary loadable/executable in the OSB environment (subject to kernel/userspace image setup).

## References
- Source: `osb.xwin/README.md`
- [Toolkit](../toolkit/README.md), [Kernel graphics/input](../kernel/graphics-and-input.md), [ABI](../osbkt/abi.md)
