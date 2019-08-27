# Executing Code and Scripts on Startup

You can execute code and scripts when you start ShiftLeft Ocular.

## Executing Initialization Code

To execute initialization code every time you start ShiftLeft Ocular, use the `predef.sc` argument. This is 
useful if you find yourself copy and pasting the same code snippets into every ShiftLeft Ocular session.

Append `predef.sc` to the command to start ShiftLeft Ocular, specifically `~/.shiftleft/ocular/predef.sc`.  
The initialization code is made available to the REPL. For example

```
echo 'def foo = 42' > ~/.shiftleft/ocular/predef.sc
./ocular.sh
ocular> foo
res0: Int = 42
```

## Executing a Script in Non-Interactive Mode

When you run ShiftLeft Ocular in non-interactive mode, you can specify a script file to execute. Refer to the article [Using ShiftLeft in Interactive and Non-Interactive Modes](../about/modes.md#using-non-interactive-scripts) for information.
