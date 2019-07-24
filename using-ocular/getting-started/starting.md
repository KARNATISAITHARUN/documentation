# Starting ShiftLeft Ocular

ShiftLeft Ocular supports the Linux, MacOS X and Windows operating systems. 

You start ShiftLeft Ocular from the directory in which it is installed (default is ~/bin/ocular on Linux and MacOS X, and C:\Users\$USERNAME\bin\ocular on Windows).

If you are automating the provision of your servers, you need to append the command `-J-Xmx<heapsize>` when starting ShiftLeft Ocular. Refer to the article [ShiftLeft Ocular Memory Size Recommendations](../about/ocular-memory-size.md) for specifics.

After starting ShiftLeft Ocular, you can: 

* [Create a CPG](create-cpg.md) for a new application or a new version of an application.
* [Work with an exisitng CPG and its layers](working-with-cpg.md).

## Linux and MacOS X

Use the following command to start ShiftLeft Ocular on the Linux and MacOS X operating systems

```
./ocular.sh
```

## Windows PowerShell

Use the following command to start ShiftLeft Ocular on the Windows operating systems

```
.\ocular.ps1
```
