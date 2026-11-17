# Building and running

> Details vary by module and evolve; always check each module's `README.md` and build scripts. This page summarizes the common workspace workflow.

## Workspace layout
Check out required repositories **side by side**, for example:

```text
osb/
├── osb.core/
├── osb.kernel/
├── osbkt/
├── osb.shell/
├── osb.xwin/        (optional for some flows)
├── osb.oslang/
└── osb.docs/
```

Builds such as `osb.kernel/build.sh` and `osb.shell` expect sibling repos and emit clear errors if required ones are missing.

## Common prerequisites (kernel/toolchain)
- .NET SDK (NativeAOT/`ilc` tooling as resolved by the build)
- `clang`, `lld` (`ld.lld`)
- `xorriso`, `grub-mkrescue` (`grub-pc-bin`)
- `nasm` (optional, depending on build)
- `qemu-system-x86_64` for testing

## Kernel: build an ISO and run
From `osb.kernel/` (example):

```sh
chmod +x build.sh
./build.sh            # produces osb.iso (stages boot artifacts incl. SHELL.EXE as configured)
./build.sh run        # headless: serial on stdio (-display none -serial stdio)
./build.sh run-gui    # VGA window; mind input grab (Ctrl+Alt+G release, Ctrl+Alt+2 monitor)
./build.sh check      # validate Multiboot2 ELF
```

Notes:
- Serial console is mirrored/usable headless; `POWEROFF` triggers ACPI shutdown in QEMU.
- Networking runs (e.g. e1000 on QEMU user-net) expose the kernel's `NET`/`PING`/`DHCP` capabilities; external ICMP reachability may be limited by user-net behavior (see kernel docs).

## Userspace shell
From `osb.shell/`:
```sh
dotnet build
```
Full image integration is driven by the kernel build (`../osb.kernel/build.sh`). Tests include `tests/smoke.sh` and golden transcript tooling (see `osb.shell/docs/MIGRATION.md` and `tests/`).

## Other modules
- `osb.core`, `osbkt`, `osb.toolkit`, `osb.oslang`: typically `dotnet build` (subject to freestanding/linking conventions per consumer).
- `osb.xwin`: `./build.sh` (produces a ring-3 binary for the OSB environment).
- `oslang.vscode`: standard VS Code extension packaging/install flow.

## See also
- [Kernel boot and build](../os/kernel/boot-and-build.md)
- [Shell overview](../os/shell/README.md)
- [Developer notes](../tooling/developer-notes.md)
