# Shell commands (behavioral reference)

The userspace shell aims for behavioral parity with the legacy in-kernel shell. The authoritative, detailed inventory (boot/login, editing, history/recall, completion, prompt/env expansion, dispatch rules, per-command help, messages) is in `osb.shell/docs/MIGRATION.md`.

## Interaction model (target)
- **Line editing:** serial and PS/2 input merged; printable chars, backspace, Enter; Ctrl+C abort semantics; arrow-key history navigation over serial escape sequences and PS/2 extended keys.
- **History:** fixed ring, `HISTORY [n]`, recall via `(:N)`, up/down navigation with draft preservation.
- **Completion (TAB):** command names, flags (where applicable), and filesystem path completion on `A:` with case-insensitive prefix matching and common-prefix extension.
- **Prompt:** DOS-style `$PROMPT` tokens (e.g. `$P`, `$G`, `$N`, `$D`, `$T`, `$V`, `$USER`/env expansion) with configured defaults.
- **Environment:** session variables, `$VAR` expansion before dispatch, `SET` semantics (`HOME` read-only behavior as specified).

## Command/alias coverage (representative)
Internal commands and aliases defined by the legacy spec include (non-exhaustive): `VER/ABOUT`, `HELP`/`/?`, `CLS/CLEAR`, `ECHO`, `SET`, `DIR/CD/PWD/TYPE/SIZE`, `MD/MKDIR`, `RD/RMDIR`, `DEL/ERASE/DELETE`, `REN/MOVE`, `COPY`, `LN`, `COLOR`, `TRASH`, `GREP`, `USER/LOGIN/LOGOUT/WHOAMI/CHOWN/CHMOD/SUDO`, `HISTORY`, job/task commands (`SPAWN/RING3/DEMO/TASK|PS/KILL/WAIT/TAB`), `OSLANG`, `KISS`, `NET/IFCONFIG`, `PING`, `PCI`, and others as listed in the migration inventory.

## Notes
- Exact availability depends on migration phase and kernel ABI/services present.
- Error messages and edge cases should match the legacy behavior where specified (see golden tests in `osb.shell/tests/golden/`).

> Source of truth: `osb.shell/docs/MIGRATION.md` (§1 inventory and cases).
