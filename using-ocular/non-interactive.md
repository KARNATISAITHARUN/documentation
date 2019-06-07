# Non-Interactive Querying with ShiftLeft Ocular

ShiftLeft Ocular can be used in non-interactive mode, to execute commands and operations without typing them after the prompt. The commands are stored in a file which can be specified as an argument. ShiftLeft Ocular runs those commands and then exits.

## Passing Scripts to ShiftLeft Ocular

To pass a script to ShiftLeft Ocular in non-interactive mode, include in `test.sc`

```
@main def exec(cpgFile: String, outFile: String) = {
   loadCpg(cpgFile)
   cpg.namespace.name.l |> outFile
}

```

You can include arbitrary Scala code in `test.sc` and use the `|>`
operator to pipe output into files. The script is run as 

```
./ocular.sh --script test.sc --params cpgFile=/fullpath/to/cpg.bin.zip,outFile=out.log
```

## Writing JSON and Pretty-Printed JSON

To write JSON

```
cpg.method.toJson |> "/tmp/foo" 
```

To write Pretty-Printed JSON

```
cpg.method.toPrettyJson |> "/tmp/foo"
```

## Appending to Files

To append to a JSON file

```
cpg.method.toJson ||> "/tmp/foo" 
```

To append to a Pretty-Printed JSON file

```
cpg.method.toPrettyJson ||> "/tmp/foo"
```
