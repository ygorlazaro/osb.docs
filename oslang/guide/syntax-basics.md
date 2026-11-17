# Syntax basics

OSLANG emphasizes readable, structured syntax with explicit block terminators in many constructs.

## Core ideas
- **Entry point:** commonly `FUNCTION MAIN()` / `END FUNCTION`.
- **Statements & blocks:** control structures and declarations use keywords with matching `END` forms (e.g. `IF ... END`, `FUNCTION ... END FUNCTION`, `CLASS ... END`) as defined by the language version.
- **Case/keywords:** keywords are written in uppercase in canonical style (e.g. `PRINT`, `VAR`, `IF`); behavior is per spec.
- **Comments & literals:** strings, numbers, booleans (`TRUE`/`FALSE`), and `NULL` are supported; later specs add escape sequences, interpolation, and multiline strings.
- **Order independence:** declarations can be organized to support multi-file/order-independent usage patterns (see [Modules](modules.md)).

## Example
```osl
FUNCTION MAIN()
    PRINT "Hello, World!"
END FUNCTION
```

## See also
- [Variables and types](variables-and-types.md)
- [Control flow](control-flow.md)
- [Specification](../specification/README.md)

> Source: `osb.oslang/docs/oslang/guide/syntax-basics.md`.
