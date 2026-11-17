# Control flow

OSLANG provides structured control flow constructs; available forms depend on the language version.

## Conditionals
- `IF` / `ELIF` / `ELSE` / `END` for branching.

## Loops
- `FOR` (including step/range forms as specified)
- `WHILE`
- `DO ... WHILE` (version-dependent)
- Loop control such as `BREAK` / `CONTINUE` (version-dependent)

## Selection
- `SWITCH` / `CASE` / `DEFAULT` / `END` (added in 0.61)

## Example (illustrative)
```osl
IF x > 0 THEN
    PRINT "positive"
ELSE
    PRINT "non-positive"
END
```

## See also
- [Operators](operators.md)
- [Error handling](error-handling.md)
- [Specification](../specification/README.md)

> Source: `osb.oslang/docs/oslang/guide/control-flow.md`.
