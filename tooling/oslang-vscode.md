# OSLANG VS Code extension

Editor support for OSLANG in Visual Studio Code (`oslang.vscode`).

## Features
- **Syntax highlighting** for `.osl` and `.oslang` (keywords, types, strings, comments, operators, method calls, punctuation, interpolation, enum sets)
- **IntelliSense** — context-aware completion (keywords, built-ins, document symbols, namespace member suggestions such as `MATH.`, `STRING.`, `ARRAY.`, `I18N.`, `FILE.`, `DIR.`, `DATE.`)
- **Hover** — inline documentation for keywords and built-in functions
- **Signature help** — parameter hints for supported functions
- **Document symbols** — outline for classes, interfaces, functions, enums, members
- **Code folding** for major block constructs
- **Snippets** for common patterns (enums, control flow, arrow functions, namespace helpers, data APIs, etc.)
- **Bracket matching, auto-closing, indentation rules**

## Language/API coverage (examples)
- Control flow and declarations (`IF/FOR/WHILE/DO/SWITCH/TRY`, `FUNCTION/CLASS/INTERFACE/ENUM`, etc.)
- Namespaces and helpers: `MATH`, `STRING`, `ARRAY`, `I18N`, `FILE`, `DIR`, `DATE`, `OSL.JSON/CSV/XML/CNF`, `OSB.NET`, conversions (`STR/NUMBER/BOOL/TYPEOF`, etc.)

Coverage tracks the targeted OSLANG version(s) supported by the extension; see the source README for the authoritative list.

## Installation
- Copy the extension folder to your VS Code extensions directory, or install from a packaged VSIX as provided by the project.

## References
- Source: `oslang.vscode/README.md`, `oslang.vscode/package.json`
- [OSLANG reference](../oslang/reference/README.md)
