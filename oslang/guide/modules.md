# Modules

OSLANG supports organizing code across multiple files/modules.

## Concepts
- **`USING`** imports or references modules/namespaces as defined by the runtime/spec.
- Multi-file programs enable separation of concerns (e.g. shared classes/utilities).
- Order-independent declaration behavior is a design goal; consult the spec for precise resolution rules.

## Example (illustrative)
```osl
USING OSL.I18N

FUNCTION MAIN()
    PRINT "Modules example"
END FUNCTION
```

## See also
- [Namespaces](namespaces.md)
- [Samples](../samples/README.md)
- [Specification](../specification/README.md)

> Source: `osb.oslang/docs/oslang/guide/modules.md` and `osb.oslang/samples/`.
