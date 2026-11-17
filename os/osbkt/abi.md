# Ring-3 ABI (summary)

**Authoritative source:** `osbkt/docs/ABI.md` (ABI version **2** as documented). This page summarizes the contract; consult the source for the complete, normative tables and structures.

## Calling convention
- **Entry:** `INT 21h` (DPL3 gate). Function number in **AH**; arguments in 64-bit registers; result in **RAX**.
- **Registers:** `RDI` (arg1), `RSI` (arg2), `RDX` (arg3), `RCX` (arg4). Some calls use `AL` for a value (e.g. exit code, built-in spawn id).
- **osbkt gate:** register-based `osb_syscall` shim (`crt0.S`) — kernel saved-frame layout is not ABI.
- **Errors:** `-1` EFAULT (bad/rejected pointer/range), `-2` ENOSYS (unknown function). `SPAWNPATH` defines specific negative codes (`-3` EACCES, `-4` ENOENT, `-5` EINVAL, `-6` ENOMEM). `GETCHAR` uses its own return space (`-1` empty, `-2` Ctrl+C).
- **Strings:** byte strings (ASCII/UTF-8) with explicit byte lengths for ring-3 string arguments (e.g. paths/args/write/resolve).
- **Pointer rule:** validated against user window with overflow-safe bounds checks.
- **User window:** base/limit defined by kernel layout (e.g. `0x2000000`–`0x2200000` per ABI doc).

## Boot program
Kernel boots the program named in `A:\OSB\CONF\BOOT` (when enabled in test-configured builds) or defaults to `A:\OSB\BIN\SHELL.EXE`; see ABI doc for `WITH_TESTS`/`BOOT_FROM_CONF` behavior.

## Syscall groups (v2)
- **Console:** `WRITE` (01h), legacy `PRINT` (09h)
- **Memory:** `MEMINFO` (10h)
- **Filesystem (read surface):** `RESOLVE` (20h), `DIRNEXT` (21h), `GETCWD` (22h), `GETARGS` (23h)
- **Processes:** `GETPID` (30h), `EXIT` (4Ch, AL), `SPAWN` (80h, AL), `SPAWNPATH` (6Ah, `SpawnReq*`), `WAIT` (81h), `GETABI` (82h → 2), `GETCHAR` (6Eh, cooked)
- **Graphics/input/power (40h–59h):** `GFX_ENTER/EXIT/INFO`, drawing (`FILL/RECT/BEVEL/PIXEL/LINE/TEXT`), mouse/cursor/key/mods/close/ticks/sleep/time, `POWEROFF/REBOOT`

## Out of v2 / planned
File write/mutation, directory mutation, env/identity/privilege, richer task control, and other services are reserved/planned (see `osb.shell/docs/MIGRATION.md` and ABI "Out of v0/v2" notes); additions bump `GETABI`.

## Notes
- `SPAWNPATH` validates request, path, type, and execute permission before creating a task (per ABI doc).
- `GETCHAR` merges serial+PS/2, is non-blocking, and has defined flush/sequence behavior.

> Source of truth: `osbkt/docs/ABI.md`.
