# Functions

Functions encapsulate reusable logic. OSLANG supports named functions and, in newer versions, arrow functions/lambdas.

## Declaring and calling
- Declared with `FUNCTION ... END FUNCTION` (and related forms per spec).
- Parameters and return values follow dynamic typing; `RETURN` yields a value.
- `MAIN` is the conventional program entry point.

## Arrow functions
- Later versions add arrow-function syntax for concise function expressions (see spec/version notes).

## Example
```osl
FUNCTION ADD(a, b)
    RETURN a + b
END FUNCTION

FUNCTION MAIN()
    PRINT ADD(2, 3)
END FUNCTION
```

## See also
- [Classes and objects](classes-and-objects.md)
- [Modules](modules.md)
- [Specification](../specification/README.md)

> Source: `osb.oslang/docs/oslang/guide/functions.md`.
