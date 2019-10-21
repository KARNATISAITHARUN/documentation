# Starting ShiftLeft Ocular

ShiftLeft Ocular supports the Linux, MacOS X and Windows operating systems. 

You start ShiftLeft Ocular from the directory in which it is installed (default is ~/bin/ocular on Linux and MacOS X, and C:\Users\$USERNAME\bin\ocular on Windows).

If you are automating the provision of your servers, you need to append the command `-J-Xmx<heapsize>` when starting ShiftLeft Ocular. Refer to the article [ShiftLeft Ocular Memory Size Recommendations](../about/ocular-memory-size.md) for specifics.

After starting ShiftLeft Ocular, you can: 

* [Create a CPG](create-cpg.md) for a new application or a new version of an application.
* [Generate a Security Profile](generate-sp.md) to automate code analysis by summarizing the vulnerabilities and data leaks present in code.
* Easily work with CPGs and Security Profiles [using your Workspace](manage-workspace.md) for a new application or a new version of an application.

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
