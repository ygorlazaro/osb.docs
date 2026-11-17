# Architecture

## Layering

```text
Userspace apps / XWin / Shell
            │
          osbkt  (INT 21h wrappers, crt0, user.ld, OSBX tooling)
            │
         INT 21h  (ring-3 ABI, DPL3 gate, validated pointers/args)
            │
        osb.kernel (ring 0: CPU/memory/process/fs/network/graphics/devices)
            │
      osb.core / MiniCoreLib (freestanding runtime stubs)
```

- **Kernel (`osb.kernel`)** owns privileged operations: IDT/PIC/GDT/TSS, paging/CR3, scheduling, syscall enforcement, drivers (VGA/serial/PS2/PIT/RTC/ATA/e1000), ext4, and in-kernel services.
- **ABI (`osbkt/docs/ABI.md`, v2 as documented)** defines the ring-3 contract: register convention, error returns, syscall table (console, memory, fs read surface, process/spawn/wait, cooked input, graphics/input/power). The kernel must enforce checks that untrusted ring-3 code must not bypass.
- **Userspace toolkit (`osbkt`)** is the only layer that should know syscall numbers/register layout; consumers call its public API. Programs are freestanding NativeAOT images linked at the user base and wrapped (OSBX) for the kernel loader.
- **Userspace shell (`osb.shell`)** targets `osb.shell -> osbkt -> INT 21h -> kernel` and owns interaction/presentation (editing, history, completion, prompt, dispatch); behavior is specified against the legacy in-kernel shell.
- **GUI stack:** `osb.toolkit` provides windowing/controls/graphics primitives for freestanding apps; `osb.xwin` is a ring-3 window manager using kernel GFX/input services.
- **OSLANG (`osb.oslang`)** is the platform scripting language (interpreter/runtime in tree; kernel includes OSLANG support) with a separate VS Code extension (`oslang.vscode`).

## Execution and isolation model (current, high level)
- Multiboot2/GRUB boot into long mode; NativeAOT-compiled kernel.
- Preemptive round-robin scheduler with per-process address spaces (private `CR3`, kernel identity-mapped in low region, per-process user window/block model).
- Ring 3 user programs enter via controlled transition; faults from CPL3 are handled as recoverable process failures where applicable.
- DOS-style presentation: drive `A:` backed by ext4 image, `\` separators, internal commands and external programs.

## References
- [Kernel overview](../os/kernel/README.md)
- [ABI contract](../os/osbkt/abi.md)
- [Shell migration](../os/shell/migration-status.md)
- [OSLANG](../oslang/README.md)
