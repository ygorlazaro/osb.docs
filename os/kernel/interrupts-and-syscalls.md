# Interrupts and syscalls

## PIC and IRQ mapping
- 8259A PIC remapped so IRQ0–15 map to vectors **0x30–0x3F** (leaving **0x21** free for `INT 21h`).
- Key vectors (as documented): timer IRQ0 → `0x30`, keyboard IRQ1 → `0x31`, serial IRQ4 → `0x34`, RTC IRQ8 → `0x38`, mouse IRQ12 → `0x3C`.
- Slave IRQ delivery requires master IRQ2 cascade unmasked (per kernel notes).

## IDT and exception handling
- 256 IDT gates installed; CPU exceptions reported as readable panics (vector/RIP) in CPL0.
- Faults from **CPL3** are treated as recoverable process failures: the offending process is terminated (zombie/exit code as applicable) while the system continues.

## Privilege transition and ring 3
- GDT includes kernel and user segments (DPL3) plus TSS; `ltr` loaded; `TSS.RSP0` points to the active process kernel stack (updated on switches).
- Paging exposes only the designated user window as user-accessible; kernel mappings remain supervisor-only.
- User entry via `iretq` to CPL3; `INT 21h` exposed as a DPL3 gate.

## Syscall model (DOS-style)
- **`INT 21h`** with function in `AH`, arguments/results in registers (see [ABI](../osbkt/abi.md)).
- Services include console/memory, filesystem read surface, process control (getpid/exit/spawn/wait), cooked input, and graphics/input/power operations.
- Pointer validation and error semantics are enforced by the kernel per ABI (e.g. EFAULT/ENOSYS conventions).

## See also
- [ABI contract](../osbkt/abi.md)
- [Memory and processes](memory-and-processes.md)

> Sources: `osb.kernel/README.md`, `osb.kernel/ROADMAP.md`, `osbkt/docs/ABI.md`.
