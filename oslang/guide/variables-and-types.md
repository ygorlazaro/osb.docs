# Variables and types

OSLANG is dynamically typed. Variables hold values whose behavior depends on the runtime type; later language versions add richer primitives, methods, and constructs such as enums.

## Declarations and assignment
- Variables are introduced/assigned according to language rules (commonly `VAR` in examples/specs; see version spec).
- Types are determined at runtime; conversions and type inspection utilities (e.g. `TYPEOF`, `STR/NUMBER/BOOL` in later versions) are provided by the language/built-ins.

## Common value kinds
- **Numbers, strings, booleans, null**
- **Arrays and objects** (including nested structures in newer versions)
- **Enums / enum sets** (added in 0.61+)
- **Temporal types** such as `DATE`/`TIME` (0.5+)

## See also
- [Operators](operators.md)
- [Namespaces](namespaces.md) (primitive/namespace methods)
- [Reference: built-ins](../reference/built-ins/README.md)
- [Specification](../specification/README.md)

> Source: `osb.oslang/docs/oslang/guide/variables-and-types.md`.
