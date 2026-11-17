# Kernel (osb.kernel)

Bootable x86-64 kernel for OSB, written in freestanding C# (NativeAOT) with a Multiboot2 boot path and custom mini-CoreLib. It brings up hardware, memory/process management, ring-3 execution with a DOS-style `INT 21h` ABI, storage/networking/graphics services, and hands control to a userspace boot program.

## Current capabilities (high level)
- **Boot/output:** GRUB Multiboot2 (BIOS/ISO), VGA text mirrored to COM1, headless serial console input (IRQ4)
- **CPU/interrupts:** IDT (256 gates), PIC remapped (IRQ0–15 → 0x30–0x3F), CPU exceptions as panics; PIT 100 Hz (IRQ0); RTC (IRQ8)
- **Scheduling/processes:** preemptive round-robin task table, per-process address spaces (`CR3`), spawn/exit/wait, zombies/reaping, job control/foreground-background semantics
- **Privilege/syscalls:** ring 3 (GDT/TSS, user paging window), `INT 21h` syscall port and graphics/input services; recoverable CPL3 faults (terminate offending process)
- **Storage:** ATA PIO, ext4 image mounted as `A:` with read operations (path resolution, DIR/CD/TYPE surface) — write operations planned
- **Users/sessions:** login/accounts, `PASSWD`/`SUDOERS` persistence under `A:\OSB\CONF`, ownership/mode commands (per current implementation)
- **Networking:** polled Intel e1000 over PCI, IPv4 stack helpers, ARP/DHCP/ICMP ping (subject to QEMU user-net limits)
- **Graphics:** Bochs VBE linear framebuffer preferred, VGA 12h fallback; `GFX` desktop mode
- **Language/runtime:** includes OSLANG support in-kernel (version per tree)

## Documentation
- [Architecture & subsystems](architecture.md)
- [Boot and build](boot-and-build.md)
- [Interrupts and syscalls](interrupts-and-syscalls.md)
- [Memory and processes](memory-and-processes.md)
- [Storage and filesystem](storage-and-filesystem.md)
- [Networking](networking.md)
- [Graphics and input](graphics-and-input.md)
- [Users and sessions](users-and-sessions.md)
- [Roadmap](roadmap.md)

## References
- Source: `osb.kernel/README.md`, `osb.kernel/Kernel.cs`, `osb.kernel/ROADMAP.md`, `osb.kernel/OSB_BAS_NOTES.md`
