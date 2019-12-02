# ShiftLeft Ocular for C++

ShiftLeft Ocular supports the examination and investigation of C++ applications.

After [installing the ShiftLeft Command Line Interface (CLI)](../using-cli/install-cli.md), [authenticating](../using-cli/authenticating.md) and [starting ShiftLeft Ocular](../using-ocular/getting-started/starting.md), [create the Code Property Graph (CPG)](../using-ocular/getting-started/create-cpg.md) for your C++ application using

```scala
ocular> createCpg(<inputPaths>)
```

where `<inputPaths>` is the path of the target application; multiple applications are separated by a comma. For C++, the path is the directory containing the c, cpp, cc, h and hpp files.

## Next Steps

[Generate the Security Profile](../using-ocular/getting-started/generate-sp.md)

[Querying the CPG and Security Profile](../using-ocular/getting-started/query-cpg.md)

[Uncover the Attack Surface](../using-ocular/use-cases/attack-surface.md)
