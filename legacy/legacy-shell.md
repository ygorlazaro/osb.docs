# Legacy shell

Hosted .NET shell implementation that preceded the current freestanding userspace shell architecture.

## Contents (source)
- Shell entry and environment/config/command modules
- Applications and games
- Configuration and help assets (`CONF/`, i18n resources)

## Role today
- Serves as historical reference and source material when porting behavior.
- The **current** userspace shell (`osb.shell`) is being rebuilt for the freestanding ring-3 environment against the kernel ABI; its behavioral parity targets are specified and tested separately (see [Shell migration](../os/shell/migration-status.md)).

## References
- Source: `osb.legacy/legacy-shell/`, `osb.legacy/README.md`
