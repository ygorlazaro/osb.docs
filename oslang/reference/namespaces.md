# Namespaces and APIs

Built-in namespaces expose grouped functionality. This is a summary; consult the relevant OSLANG specification for exact members and semantics.

## Core namespaces
- **`MATH`** — numeric operations (e.g. `SQRT`, `POW`, `FLOOR`, `CEIL`, `ABS`, `SIN/COS/TAN`, `PI`, `RANDOM`, `MAX/MIN`, `TRUNC` in later versions)
- **`STRING`** — string manipulation (length, case, trim, substring/search, formatting helpers — version-dependent)
- **`ARRAY`** — collection operations (`COUNT`, indexing/search, push/pop/join/sort/flat variants — version-dependent)

## Host/environment namespaces
- **`FILE`** — file operations (exists/read/write/append/delete/list — host-dependent)
- **`DIR`** — directory operations (exists/list/create/delete/current — host-dependent)
- **`DATE` / `TIME`** — temporal values and helpers (0.5+)
- **`I18N`** — localization helpers (load/get/language management where available)

## Data interchange (0.62+)
- **`OSL.JSON`** — parse/stringify/pretty/read/write
- **`OSL.CSV`** — parse/stringify/read/write
- **`OSL.XML`** — parse/stringify/read/write and node access helpers
- **`OSL.CNF`** — config read/write/get/set/keys/save

## Networking (0.62+)
- **`OSB.NET`** — network helpers (e.g. `PING`, `DOWN`) as provided by the runtime/host

## See also
- [Guide: namespaces](../guide/namespaces.md)
- [Specification](../specification/README.md)

> Sources: `osb.oslang/docs/oslang/`, `OSLANG-*-SPEC.md`, `oslang.vscode/README.md`.
