# OSLANG specification

OSLANG has evolved through versioned specifications maintained in `osb.oslang/`. This section indexes that lineage for reference.

## Version lineage (0.1 → 0.73)
Specs present in the source tree include (non-exhaustive list of named files):
- `OSLANG 0.1 — Especificação da Linguagem.md`
- `OSLANG-0.2-SPEC.md`, `OSLANG-0.3-SPEC.md`, `OSLANG-0.4-SPEC.md`
- `OSLANG-0.5-SPEC.md`, `OSLANG-0.51-SPEC.md`
- `OSLANG-0.6-SPEC.md`, `OSLANG-0.61-SPEC.md`, `OSLANG-0.62-SPEC.md`
- `OSLANG-0.7-SPEC.md`, `OSLANG-0.72-SPEC.md`, `OSLANG-0.73-SPEC.md`
- `OSLANG-SPEC.md` (rolling/current pointer in source, if applicable)
- `OSLANG-GAP.md` (gaps/notes)

## Notable milestones (summary)
- **0.4/0.4x:** primitive methods, namespaces (MATH/FILE/DIR), dynamic typing refinements, `ARGS` in `MAIN` (per notes)
- **0.5:** native `DATE`/`TIME` types and namespaces
- **0.6:** expanded operators/methods (e.g. `MOD`, `**`, `++/--`, `+=`, `TYPEOF`, array/string/math additions, `SHOW`)
- **0.61:** `ENUM`/enum sets, `SWITCH/CASE/DEFAULT`, `BREAK`, string interpolation, multiline strings, escape sequences
- **0.62:** `OSL.JSON/CSV/XML/CNF` and `OSB.NET` APIs
- **0.7x:** later refinements as specified in 0.7/0.72/0.73 documents

## How to use
- Treat the **versioned spec in `osb.oslang`** as authoritative for implementation details.
- Use [Changelog](changelog.md) for a curated summary and `OSLANG-GAP.md` for known gaps.
- See [versions/](versions/README.md) for notes on mirrored snapshots.

> Source of truth: `osb.oslang/OSLANG-*-SPEC.md`, `OSLANG-GAP.md`.
