# Boot and build

## Toolchain and pipeline
`osb.kernel/build.sh` orchestrates a freestanding build:
1. **C# (mini-CoreLib)** → managed IL
2. **ILC (NativeAOT)** → freestanding x86-64 object
3. **Assembly** (`boot.S`, `sched.S`, `user.S`, generated `interrupts.S`) → objects
4. **Link** (`ld.lld` + `linker.ld`) → Multiboot2 ELF (`kernel.elf`)
5. **Image** (`grub-mkrescue`) → bootable `osb.iso` (with staged boot artifacts such as `/OSB/BIN/SHELL.EXE` as configured)

`interrupts.S` is generated deterministically (256 ISR stubs + common handler path).

## Prerequisites
- .NET SDK 10.x (and NativeAOT/ilc resolution via NuGet cache as used by the build)
- `clang`, `lld` (`ld.lld`), `xorriso`, `grub-mkrescue` (`grub-pc-bin`), `nasm` (optional)
- `qemu-system-x86_64`

## Build and run
```sh
./build.sh            # build osb.iso
./build.sh run        # headless QEMU (-display none -serial stdio), e1000 user-net
./build.sh run-gui    # QEMU with VGA window
./build.sh check      # verify kernel.elf is Multiboot2
```

## Runtime notes
- **Headless:** VGA mirrored to COM1 and serial accepted as input; type directly in terminal. Exit with `POWEROFF`.
- **GUI:** input grab may occur (`Ctrl+Alt+G` to release, `Ctrl+Alt+2` for monitor); `POWEROFF` exits emulator cleanly.
- **Siblings:** build expects required repos side by side (`osb.shell`, `osbkt`, `osb.core`, etc.) and fails clearly if missing.

> Source: `osb.kernel/README.md`, `osb.kernel/build.sh`.
