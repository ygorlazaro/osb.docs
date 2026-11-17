# Users and sessions

## Model
- Unix-style inspired login/accounts adapted to the OSB/DOS-like environment.
- Boot reaches a `login:` flow (per kernel shell implementation); root may log in without a password, others prompted as configured.
- Session state tracks current user, home, and working directory context.

## Account and privilege management (as implemented)
- User management commands: `USER` (`/ADD`, `/CHANGE`, `/DEL` with options), `LOGIN`, `EXIT/LOGOUT`
- Ownership/mode: `CHOWN`, `CHMOD`; elevation via `SUDO` for authorized users (password confirmation as designed)
- Persistence under `A:\OSB\CONF\` (e.g. `PASSWD`, `SUDOERS`) with system-owned areas and per-user homes (e.g. `A:\OSB\HOME\<user>`), plus shared/temp areas as documented.

## Notes
- Exact permission enforcement, session/console multiplexing, and multiuser semantics continue to evolve (see [Roadmap](roadmap.md) and kernel/shell sources).
- The userspace shell migration re-homes session/login behavior per the behavioral specification in `osb.shell/docs/MIGRATION.md`.

> Sources: `osb.kernel/README.md`, `osb.kernel/Shell/Users.cs`, `osb.kernel/ROADMAP.md`.
