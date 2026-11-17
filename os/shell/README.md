# Shell (osb.shell)

Freestanding userspace shell for OSB, built as `SHELL.EXE` and spawned by the kernel at boot. It targets the layering `osb.shell -> osbkt -> INT 21h -> osb.kernel`.

## Status
- **Migration in progress** from the legacy in-kernel shell (`osb.kernel/Shell/**`) to userspace. The legacy shell is the behavioral specification (not a code port).
- Early phases establish ABI-checked startup and minimal line input over `GETCHAR` (6Eh); full command set, editing/history/completion, sessions/login, and external execution arrive in slices.

## Goals and constraints
- Shell uses only the **public osbkt API** (no direct syscall/register knowledge).
- Kernel enforces security/privilege-sensitive checks; shell owns presentation/interaction and command behavior.
- DOS-like UX: drive letters, `\`, DOS command names/aliases/flags/messages.

## Documentation
- [Commands (behavior spec)](commands.md)
- [Migration status](migration-status.md)

## Build and test
```bash
dotnet build
```
Integration image built via `../osb.kernel/build.sh`. Smoke and golden transcript tests live under `tests/` (see migration doc for phases and coverage).

## References
- Source: `osb.shell/README.md`, `osb.shell/docs/MIGRATION.md`
- [ABI](../osbkt/abi.md), [Kernel](../kernel/README.md)
