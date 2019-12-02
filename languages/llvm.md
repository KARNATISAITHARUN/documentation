# ShiftLeft Ocular for LLVM (Beta)

ShiftLeft Ocular supports the examination and investigation of LLVM applications. LLVM is a compiler infrastructure project providing a set of compiler and toolchain technologies, which can be used to develop a front end for any programming language and a back end for any instruction set architecture.

ShiftLeft Ocular examines LLVM bitcode in the following forms:

* LLVM IR (human-readable representation)
* LLVM Bitcode (bitstream representation)
* Embedded Bitcode (bitstream representation embedded into a binary)

Before using ShiftLeft Ocular, you must extract the LLVM bitcode out of high-level source code by introducing the appropriate flags into your build system (i.e. `-emit-llvm`, `-flto`, or `-fembed-bitcode`), or use `whole-program-llvm`.

After [installing the ShiftLeft Command Line Interface (CLI)](../using-cli/install-cli.md), [authenticating](../using-cli/authenticating.md) and [starting ShiftLeft Ocular](../using-ocular/getting-started/starting.md), [create the Code Property Graph (CPG)](../using-ocular/getting-started/create-cpg.md) for your LLVM application using

```scala
ocular> createCpg(<inputPaths>)
```

where `<inputPaths>` is the path of the target application; multiple applications are separated by a comma. For LLVM, the path is the directory containing the LLVM files.

## Next Steps

[Generate the Security Profile](../using-ocular/getting-started/generate-sp.md)

[Querying the CPG and Security Profile](../using-ocular/getting-started/query-cpg.md)

[Uncover the Attack Surface](../using-ocular/use-cases/attack-surface.md)

