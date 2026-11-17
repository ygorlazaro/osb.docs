# Classes and objects

OSLANG supports object-oriented programming with classes, constructors, fields/properties, and methods.

## Concepts
- **Class:** defines a type with state and behavior (`CLASS ... END`).
- **Constructor:** initialization logic (keyword/form per spec, e.g. `CONSTRUCTOR`).
- **Instances:** created with `NEW` (per version semantics).
- **Members:** methods and properties accessed on instances; `ME`/`BASE` used in relevant contexts (inheritance/overrides).

## Example (illustrative)
```osl
CLASS Greeter
    CONSTRUCTOR(name)
        ME.name = name
    END

    FUNCTION SayHello()
        PRINT "Hello, " + ME.name
    END FUNCTION
END

FUNCTION MAIN()
    g = NEW Greeter("OSB")
    g.SayHello()
END FUNCTION
```

## See also
- [Inheritance](inheritance.md), [Interfaces](interfaces.md), [Generics](generics.md)
- [Specification](../specification/README.md)

> Source: `osb.oslang/docs/oslang/guide/classes-and-objects.md`.
