# Kernel roadmap (summary)

High-level evolution from a single ring-0 prototype toward a multitasking, multiuser DOS-like system with ext4 on disk. Status below summarizes the documented roadmap; verify `osb.kernel/ROADMAP.md` for the authoritative, current plan.

## Principles
- **DOS-like UX:** drive letters, `\`, internal commands, external programs (not Unix semantics).
- **Preemptive multitasking** and **multiuser** model adapted to OSB conventions.
- **ext4 on-disk** for Linux tooling interoperability, presented DOS-like.
- **`INT 21h` ABI** for syscalls.

## Slices (documented)
1. **Ring 3 + `INT 21h` ABI** — ✅ implemented (user segments/TSS, DPL3 gate, basic syscalls, recoverable CPL3 faults, job/task management foundations)
2. **Per-process address spaces** — ✅ implemented (`CR3` per process, spawn/exit/wait, private memory isolation)
3. **Consoles/sessions + login** — planned/in progress per roadmap
4. **ext4 read-only** — largely implemented for core read paths/mount/navigation
5. **ext4 read/write + DOS file commands** — planned (allocation/bitmaps, create/write/delete/rename/copy)
6. **Multiuser permissions over ext4** — planned (uid/gid/mode enforcement, elevation/admin model)
7. **init + services** — planned
8. **DOS comfort** — partial (env/`SET`/`PROMPT` present); PATH resolution, redirection, scripting model (OSH/OSLANG direction) planned

## Open decisions / risks (examples)
- Permission model mapping (ext4 modes/attrs vs DOS-like UI)
- Executable format conventions
- Hardening (IST, SMAP/SMEP, stricter pointer validation) later

> Source of truth: `osb.kernel/ROADMAP.md`.
