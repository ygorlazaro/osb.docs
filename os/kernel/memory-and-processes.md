# Memory and processes

## Physical memory
- Physical frame allocator (4 KiB frames, bitmap) over the Multiboot2 memory map.
- A reserved user window range is excluded from general allocation to support per-process mappings initialized via kernel identity mapping (see kernel notes).

## Address spaces
- Each process has its own paging structures (`PML4/PDPT/PD`/CR3) with kernel mappings copied as needed and a **private 2 MiB user block** mapped at the process window (same virtual range, distinct physical backing).
- Scheduler performs `CR3` switches and updates `TSS.RSP0` to the target process kernel stack.

## Scheduling and tasks
- Preemptive round-robin over a fixed task table (slot 0 is the console/shell context).
- Timer-driven preemption (PIT IRQ0) performs context switches; free slots skipped.
- Context switching preserves required register state in cooperation with ISR-saved frames.

## Process lifecycle and job control
- **`spawn` / `exit` / `wait`** semantics: terminated processes become zombies with exit codes until reaped; killing can reap/free as implemented.
- Foreground/background execution (`&`), `TASK/PS`, `KILL`, `WAIT`, `TAB` and Ctrl+C foreground abort behavior are provided by the shell/job-control layer over kernel primitives.
- User-mode isolation demonstrated by private-memory behavior across concurrent ring-3 processes; faults from CPL3 terminate only the offending process.

## Syscalls
Process-related ABI calls include `GETPID`, `EXIT`, `SPAWN`/`SPAWNPATH`, `WAIT`, and related services (see [ABI](../osbkt/abi.md)).

> Sources: `osb.kernel/README.md`, `osb.kernel/ROADMAP.md`.
