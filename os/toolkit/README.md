# osb.toolkit

Minimal GUI toolkit for OSB freestanding programs, providing a text/graphics-mode UI framework used by applications such as XWin.

## Components
- **`App.cs`** — application lifecycle and message/render loop integration
- **`Desktop` / `Window`** — desktop and window management, input and paint handling
- **`Control.cs`** — base control abstractions
- **`Graphics.cs`, `Paint.cs`, `Types.cs`** — drawing primitives and shared types

## Design constraints
- Built for freestanding NativeAOT environments consistent with OSB runtime constraints.
- Integrates with kernel-provided display/input services in the target environment.

## Build
```bash
dotnet build
```

## References
- Source: `osb.toolkit/README.md`, `osb.toolkit/*.cs`
- Consumers: [XWin](../xwin/README.md)
