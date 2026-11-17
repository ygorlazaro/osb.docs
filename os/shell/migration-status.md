# Shell migration status

## Summary
The userspace shell replaces the legacy in-kernel shell in phased slices. The legacy implementation is the **behavioral specification**; parity is validated with smoke and golden transcript tests.

## Documented phases (from migration plan)
- **Phase 1a:** ABI-checked startup against kernel/userspace contract (completed baseline).
- **Phase 1b:** minimal line reader over `GETCHAR` (6Eh), echo/backspace/enter, Ctrl+C abort, basic command handling (`EXIT`), `GETABI >= 2` checks.
- **Phase 1b.1 (in progress as recorded):** review/hardening round — byte-string conventions, pointer/length validation and error-code tightening for SPAWNPATH/RESOLVE, exec checks, PS/2 extended-key mapping, input flush on spawn, idle behavior on empty input, ABI documentation alignment; golden/smoke coverage for affected cases.
- **Later phases:** expand toward full legacy parity (editing/history/completion, prompt/env, filesystem and user commands, pipes, job/external execution, login/session re-homing) as kernel ABI/services are added.

## Testing
- `tests/smoke.sh` — boot/bring-up checks (including boot-program behavior rules)
- `tests/golden/` — transcript capture and comparison against legacy behavior (cases added per phase; some cases pending where legacy capture is not feasible)

## References
- Source of truth: `osb.shell/docs/MIGRATION.md` (status header, inventory, acceptance/golden manifest)
- [ABI](../osbkt/abi.md)
- [Kernel roadmap](../kernel/roadmap.md)
