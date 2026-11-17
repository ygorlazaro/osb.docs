# Developer notes

## Workspace conventions
- **Sibling checkouts:** keep OSB repos side by side (kernel, shell, osbkt, osb.core, etc.). Many build scripts validate required siblings and fail with explicit messages.
- **Freestanding model:** kernel and ring-3 components avoid BCL/GC and exception-driven normal control flow; `osb.core` `MiniCoreLib` defines the minimal runtime surface.
- **Source of truth:** ABI (`osbkt/docs/ABI.md`), shell behavior/migration (`osb.shell/docs/MIGRATION.md`), kernel roadmap (`osb.kernel/ROADMAP.md`), and OSLANG specs (`osb.oslang/OSLANG-*-SPEC.md`) live in their module repos.

## Build and test workflow
- Kernel: `osb.kernel/build.sh` produces `osb.iso`, supports headless/GUI QEMU runs and Multiboot checks.
- Shell: `dotnet build` in `osb.shell/`; integrated into kernel image builds; `tests/smoke.sh` and `tests/golden/` validate boot and transcript parity (phase-gated).
- Userspace images: linked with osbkt `crt0.S`/`user.ld` and wrapped with `mkosbx.py` as required.

## Documentation practices
- Document behavior and status (implemented vs planned) explicitly.
- Keep DOS-like terminology and ABI/register conventions precise.
- Prefer linking to canonical module docs for volatile contracts to reduce drift.

## See also
- [Building and running](../overview/building-and-running.md)
- [Shell migration status](../os/shell/migration-status.md)
- [Contributing](../CONTRIBUTING.md)
