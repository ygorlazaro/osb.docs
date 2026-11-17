# Storage and filesystem

## Block device
- **ATA PIO** driver for sector I/O (used with an IDE-backed disk image in QEMU).

## ext4 on `A:`
- Disk image generated during build and mounted as the DOS-style drive `A:`.
- **Implemented (read-focused):** superblock/group descriptors/inode access, directory entries, path resolution (`.`, `..`, relative/absolute), and commands/surfaces such as `DIR`, `CD`, `TYPE` over the mounted filesystem (per current kernel state).
- **Planned:** write-side operations (allocation/bitmaps, create/remove/write, then higher-level commands like `MD/RD/COPY/DEL/REN` as the implementation matures).

## DOS presentation
- Drive-letter namespace with `\` separators and per-drive current directory semantics as applicable.
- System areas under `A:\OSB\...` (e.g. `BIN`, `CONF`, `HOME`, `TEMP`) used by boot/login/config flows.

## See also
- [Users and sessions](users-and-sessions.md)
- [Roadmap](roadmap.md)
- [ABI (filesystem syscalls)](../osbkt/abi.md)

> Sources: `osb.kernel/README.md`, `osb.kernel/ROADMAP.md`.
