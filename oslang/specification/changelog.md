# OSLANG changelog (summary)

Curated highlights by version. For exact semantics, see the corresponding spec in `osb.oslang/`.

## 0.8
- Formalizes object identity and equality by value category: arrays, class instances, closures and enum sets are compared by identity; `NULL` equals only `NULL`; booleans, numbers and strings compare by value/content.
- Intentional breaking change: closures now compare by identity — a closure is equal to itself (reflexive) and distinct closures remain unequal. Previously closures always compared unequal, including a closure compared with itself.
- No syntax, operator, or standard-library additions; otherwise preserves OSLANG 0.73 semantics.

## 0.7
- Terminal and interactive application support (primary integration target: KISS.NET).
- New namespaces: `OSL.CONSOLE`, `OSL.APP` (existing `OSL.FILE` retained).
- Full-screen applications, keyboard event input, special keys, cursor positioning, color control, efficient rendering, resize detection, interactive terminal mode, alternate screen, text file editing, command-line arguments, error handling and event-loop applications.
- Additive / non-breaking.

## 0.62
- Data interchange namespaces: `OSL.JSON`, `OSL.CSV`, `OSL.XML`, `OSL.CNF`
- Networking namespace: `OSB.NET` (`PING`, `DOWN`)

## 0.61
- `ENUM` and enum sets
- `SWITCH` / `CASE` / `DEFAULT`, `BREAK`
- String interpolation, multiline strings, escape sequences (`\n`, `\t`, `\\`)

## 0.6
- `SHOW`, arrow functions
- Operators: `MOD`, `**`, postfix `++/--`, `+=`, `TYPEOF`
- Nested arrays and array methods (e.g. `FINDINDEX`, `FOREACH`, `CONTAINS`, `JOIN`, `PUSH`, `POP`, `SORT`, `FLAT`, `FLATMAP`)
- Math/string additions (e.g. `MATH.PI`, trig functions, `NUMBER.TRUNC`, string padding/repeat)

## 0.5 / 0.51
- Native `DATE` and `TIME` types and `DATE.*` / `TIME.*` namespaces
- Temporal conversions

## 0.4 / 0.41
- Primitive methods and namespaces (`MATH`, `FILE`, `DIR`)
- Stable dynamic typing improvements
- Optional `ARGS` parameter to `FUNCTION MAIN(Args)`

## 0.1 – 0.3
- Foundational language definitions and early evolution (see 0.1–0.3 specs)

## Gaps and notes
- See `osb.oslang/OSLANG-GAP.md` in the source repository for documented gaps and follow-ups.

> Sources: `osb.oslang/docs/oslang/index.md`, `OSLANG-*-SPEC.md`, `OSLANG-GAP.md`.
