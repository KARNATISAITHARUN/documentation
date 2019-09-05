# Executing Code and Scripts on Startup

You can execute code and scripts when you start ShiftLeft Ocular.
This is useful if you find yourself copy and pasting the same code snippets into every ShiftLeft Ocular session.

## For a particular session

To execute initialization code for one session, you can provide your input scripts with `--import`, which allows to specify a comma-separated list of scripts:
```
echo 'def foo = 42' > scripts/myScript1.sc
echo 'def bar = 43' > scripts/myScript2.sc
./ocular.sh --import scripts/myScript1.sc,scripts/myScript2.sc
ocular> foo
res0: Int = 42
ocular> bar
res1: Int = 42
```


## Every time you start Ocular

To execute initialization code every time you start ShiftLeft Ocular, simply write it to a `.sc` file in your `~/.shiftleft/ocular/` directory. 
A good convention is to use `~/.shiftleft/ocular/predef.sc`, but Ocular will execute all `.sc` files on every startup.

```
echo 'def foo = 42' > ~/.shiftleft/ocular/predef.sc
./ocular.sh
ocular> foo
res0: Int = 42
```

## Executing a Script in Non-Interactive Mode

When you run ShiftLeft Ocular in non-interactive mode, you can specify a script file to execute. Refer to the article [Using ShiftLeft in Interactive and Non-Interactive Modes](../about/modes.md#using-non-interactive-scripts) for information.
