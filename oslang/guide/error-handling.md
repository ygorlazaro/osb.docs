# Error handling

OSLANG provides structured runtime error handling.

## Concepts
- **`TRY` / `CATCH`** blocks capture runtime errors so programs can recover or report failures gracefully.
- **`RAISE`** (where available) can signal errors/events depending on context and version.
- Error objects/messages and supported patterns are defined by the language/runtime version.

## Example (illustrative)
```osl
TRY
    ' risky operation
CATCH err
    PRINT err
END
```

## See also
- [Events](events.md)
- [Specification](../specification/README.md)

> Source: `osb.oslang/docs/oslang/guide/error-handling.md`.
