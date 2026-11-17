# Graphics and input

## Display
- **Text mode:** VGA text buffer with serial mirroring for headless use.
- **Graphics mode:** `GFX` opens a desktop-style UI drawn over available backends:
  - **Bochs/DISPI VBE linear framebuffer** (preferred, e.g. with QEMU `-vga std`)
  - **Legacy VGA mode 12h** (640x480 planar) fallback
- Console font state is preserved/restored across graphics transitions as implemented.

## Input
- **PS/2 keyboard** (IRQ1) with scancode handling and modifier state; serial console (COM1, IRQ4) also delivers input for headless sessions.
- **PS/2 mouse** (IRQ12) in stream mode for graphical interaction.
- Shell/editor behavior (history navigation, completion, Ctrl+C handling) is implemented in the shell/input layers (see [Shell](../shell/README.md)).

## Syscall support
Kernel provides graphics/input-related services via `INT 21h` (enter/exit gfx, info, drawing primitives, text, mouse/key state, timing, power) as defined in the [ABI](../osbkt/abi.md).

> Sources: `osb.kernel/README.md`, `osb.kernel/Hardware/*`, `osb.kernel/Graphics/*`, `osb.kernel/Cpu/SyscallGfx.cs`.
