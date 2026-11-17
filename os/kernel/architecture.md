# Kernel architecture and subsystems

`osb.kernel` is organized into focused subsystems under its own namespaces. `Kernel.cs` orchestrates bring-up in order and transfers control to the configured boot program.

## Subsystem layout (source tree)
- **`Hardware/`** — device I/O boundary: VGA (`0xB8000`, mirrored to COM1), serial (COM1, IRQ4 ring), PS/2 keyboard (IRQ1), console control signals, native port/CPU helpers (P/Invoke to assembly stubs)
- **`Memory/`** — layout constants (vectors, segments, windows, storage/task limits) and physical memory/frame allocator over Multiboot2 memory map
- **`Cpu/`** — PIT (100 Hz), RTC (CMOS clock), GDT/TSS (user segments, `RSP0`), interrupts/IDT/PIC dispatch, syscall entry/handling (`INT 21h`, GFX/input services)
- **`Processes/`** — task table and preemptive round-robin scheduler, job/foreground control, per-process address spaces (private `CR3`, user window mapping)
- **`Storage/`** — ATA PIO block access, ext4 reader/writer components (current focus: read/mount/path resolution), current working directory/drive state
- **`Network/`** — PCI config/scan, polled e1000 driver, IPv4/ARP/ICMP/UDP helpers, DHCP client
- **`Shell/`** — in-kernel shell components and command/environment/prompt/user/history logic (legacy in-kernel shell; userspace shell migration moves behavior out per plan)
- **`Graphics/`** — framebuffer/VGA graphics support and font/data used by GFX paths
- **Assembly/linker/build:** `boot.S`, `sched.S`, `user.S`, `interrupts.S` (generated), `linker.ld`, `grub.cfg`, `build.sh`

## Boot sequence (conceptual)
1. Serial/VGA init and banner output
2. PIC remap, PIT, RTC, GDT/TSS, memory detection & frame allocator
3. IDT install, scheduler/address-space init, font/mouse init
4. Environment/init and spawn/wait of boot program (`SHELL.EXE` by default; `CONF/BOOT` honored only in test-configured builds as documented)

## Design notes
- Freestanding NativeAOT: no OS/runtime/BCL; `osb.core` `MiniCoreLib` defines the runtime boundary.
- Interrupt stubs save/restore required state; context switch preserves callee-saved regs while ISR frames hold the interrupted context.
- Kernel identity mapping and a dedicated per-process user window enable private process memory while allowing kernel initialization.

> Sources: `osb.kernel/README.md`, `osb.kernel/Kernel.cs`, subsystem sources.
