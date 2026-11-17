# OSLANG

OSLANG is the scripting language for OSB — a dynamically typed, interpreted language supporting procedural, object-oriented, and event-driven styles with order-independent declarations and multi-file programs.

## Contents
- [Getting started](getting-started/README.md)
- [Language guide](guide/README.md)
- [Reference](reference/README.md) — [keywords](reference/keywords/README.md), [built-ins](reference/built-ins/README.md), [namespaces](reference/namespaces.md)
- [Specification](specification/README.md) — version lineage 0.1 → 0.73
- [Samples](samples/README.md)

## Highlights (by capability)
- **Core:** variables/types, control flow (`IF/FOR/WHILE/DO/SWITCH`), functions (incl. arrow), modules/imports
- **OOP:** classes, constructors/properties/methods, interfaces, inheritance (`VIRTUAL`/`OVERRIDE`), generics
- **Data & events:** arrays/objects, enums/enum sets, events (`EVENT/ON/RAISE`), try/catch
- **Namespaces & APIs (version-dependent):** `MATH`, `STRING`, `ARRAY`, `FILE`, `DIR`, `DATE`, `TIME`, `I18N`, data formats `OSL.JSON/CSV/XML/CNF`, networking `OSB.NET`, and more
- **Modern syntax (later versions):** string interpolation, multiline strings, escape sequences, operators/methods added across 0.4–0.62+

## Source of truth
- Language implementation: `osb.oslang/` (lexer, parsing, AST, compilation, runtime, interpreter)
- Existing language docs: `osb.oslang/docs/oslang/`
- Versioned specifications: `osb.oslang/OSLANG-*-SPEC.md` (and `OSLANG-GAP.md`)

This section curates those materials for publication; for the latest spec details, consult the `osb.oslang` repository.
