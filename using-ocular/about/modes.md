# Using ShiftLeft in Interactive and Non-Interactive Modes

ShiftLeft Ocular lets you perform code analysis on CPGs and Security Profiles, both interactively using a REPL or with non-interactive scripts. 

## Using the REPL

A REPL is an interactive shell with support for the Ocular Query Language (OQL). Among other features, a REPL offers utilities for exporting security analysis results as plain text or in JSON format, readline support, and tab-completion. It is based on Ammonite, so many of its [documented features](http://ammonite.io/#Features) apply.

The ShiftLeft Ocular underlying shell is an interactive Scala shell which includes useful things like:

* \\&lt;CTRL-c&gt; cancel current operation / clear shell. Does not quit Ocular.
* \\&lt;CTRL-d&gt; quit Ocular (shell must be clear)
* \\&lt;TAB&gt; for autocomplete.
* \\&lt;UP&gt; and <DOWN> for moving through the command history. Supports multi-line editing!
* \\&lt;CTRL-LEFT/RIGHT&gt; step through word by word (rather than character by character)
* \\&lt;CTRL-r&gt; to search the command history. Hit CTRL-r (alternatively UP/DOWN) to cycle through the matches.

### Importing additional scripts
You can dynamically load additional scripts - let's assume the file `MyScript.sc` contains simply `val elite = 31337`.

```scala
import $file.MyScript
MyScript.elite
res1: Int = 31337
```

If the file is in a subfolder (e.g. `scripts`), use dot syntax: `import $file.scripts.MyScript`. 
Use `^` to go up one directory. 

### Adding dependencies to the classpath dynamically
```scala
// java dependency
import $ivy.`com.michaelpollmeier:versionsort:1.0.1`
versionsort.VersionHelper.compare("2.1.0", "2.0.10")
// res: Int = 1

// scala dependency
import $ivy.`com.michaelpollmeier::colordiff:0.9`
colordiff.ColorDiff(List("a", "b"), List("a", "bb"))
// color coded diff
```

If the dependencies are not on Maven Central, you can add a resolver as follows:

```scala
interp.repositories() ++= Seq(coursierapi.MavenRepository.of("https://shiftleft.jfrog.io/shiftleft/libs-snapshot-local"))
```

### Browse results in a less-like viewer
```scala
browse(cpg.method.name.l)
```

### Measure the time while running a computation
```scala
time { 
  println("long running computation")
  Thread.sleep(1000)
  42
}
// res: (42, 1000332390 nanoseconds)
```

## Using Non-Interactive Scripts

ShiftLeft Ocular can be used in non-interactive mode, to execute commands and operations without typing them after the prompt. The commands are stored in a file which can be specified as an argument. ShiftLeft Ocular runs those commands and then exits. For example, include in `test.sc`

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

### Importing additional scripts

If your script depends on code from one or more additional scripts, you can use the `--import` parameter, which accepts a comma-separated list of input scripts

```
echo 'def hello(name: String) = println(s"hello, $name")' > scripts/hello.sc
echo '@main def exec(name: String) = hello(name)' > scripts/myScript.sc
./ocular.sh --script scripts/myScript.sc --params name=shiftleft --import scripts/hello.sc

# prints: 
# hello, shiftleft
# script finished successfully
```

### Writing JSON and Pretty-Printed JSON

To write JSON

```
ocular> cpg.method.toJson |> "/tmp/foo" 
```

To write Pretty-Printed JSON

```
ocular> cpg.method.toPrettyJson |> "/tmp/foo"
```

### Appending to Files

To append to a JSON file

```
ocular> cpg.method.toJson ||> "/tmp/foo" 
```

To append to a Pretty-Printed JSON file

```
ocular> cpg.method.toPrettyJson ||> "/tmp/foo"
```
