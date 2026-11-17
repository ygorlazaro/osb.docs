# Original OSB (BASIC era)

Preserved from the original OSB implementation (circa 2000, QBasic/BC7), providing historical behavior and design intent.

## Contents (source)
- **`OSB.BAS`** — main kernel/shell logic
- **`XWIN/`** — graphical/animation effects (e.g. RADIAIS, CIRCULOS, COMPLEX, LINHAS, FOGO)
- **`CONF/`**, **`HELP/`**, **`APLIC/`**, **`GAMES/`** — configuration, help, applications, games
- Compiled artifacts (`.COM`, `.EXE`) as historically produced

## Behavioral notes (from kernel analysis)
- Boot read configuration (system paths, colors, logo/message, mouse/clock helpers) and ran startup scripts.
- DOS-like prompt and command normalization with aliases/help behaviors; commands spanning filesystem ops, environment, utilities, and external programs.
- Error handling mapped BASIC runtime errors to user-facing messages.

These behaviors inform the modern reimplementation and the behavioral specification used by the shell migration.

## References
- Source: `osb.legacy/original-src/`, `osb.kernel/OSB_BAS_NOTES.md`
