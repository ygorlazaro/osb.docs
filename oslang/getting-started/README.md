# Getting started with OSLANG

## Overview
OSLANG programs are plain-text scripts (commonly `.osl` / `.oslang`) interpreted by the OSLANG runtime. In the OSB ecosystem, OSLANG can run as an embedded/hosted scripting language and is also integrated into the OSB environment (the kernel includes OSLANG support in current builds).

## Prerequisites
- An OSLANG-capable host: build/run from `osb.oslang` in the OSB workspace, or use an OSB environment/tooling that embeds the interpreter.
- For editor support, see [OSLANG VS Code extension](../../tooling/oslang-vscode.md).

## First steps
1. Create a script file (e.g. `hello.osl`).
2. Run it with the OSLANG interpreter/host provided by your environment.
3. Explore the [Hello World](hello-world.md) example and the [Language guide](../guide/README.md).

## Next
- [Hello World](hello-world.md)
- [Syntax basics](../guide/syntax-basics.md)
- [Variables and types](../guide/variables-and-types.md)

> Source: adapted from `osb.oslang/docs/oslang/getting-started/`.
