# Installing ShiftLeft Ocular

Before installing ShiftLeft Ocular, make sure you have [met all requirements](../introduction/requirements.md). The process of installing ShiftLeft Ocular is:

1. Decompress the ocular-distribution.zip file provided by ShiftLeft, by issuing the following command, where `[version]` is the actual version number of the distribution file.

```
    unzip ocular-distribution-[version].zip
```

2. Enter the password you received from ShiftLeft. The directory `ocular-distribution` is created.

3. Navigate to the new directory `ocular-distribution`. From there, for Linux and MacOS X run the installer using the `./install.sh` command. For the Windows OS, run the installing using the command `.\install.ps1` and then follow the prompt.

    If you don't have permission, first use the  command `chmod +x install.sh` and then run the installer again.

4. Identify where you want to install ShiftLeft Ocular (defaults to `~/bin/ocular` on Linux and MacOS X, and to `C:\Users\$USERNAME\bin\ocular` on Windows).

5. Specify how you want the installation to proceed. Enter either `y` to replace an existing installation, `n`to not replace the existing installation, `A`to install all ShiftLeft Ocular files, `N` to install no ShiftLeft Ocular files, or `r` to rename the installation directory.

6. If the ShiftLeft dynamic policy already exists, you are given the option to delete it. Otherwise the installer unpacks the ShiftLeft dynamic policy in the directory `~/.shiftleft/policy/dynamic`.

7. If the ShiftLeft static policy already exists, you are given the option to delete it. Otherwise the installer unpacks the ShiftLeft static policy to `~/.shiftleft/policy/static`.

8. Start ShiftLeft Ocular by using `./ocular.sh` on Linux and MacOS X, and `.\ocular.ps1` on Windows PowerShell. 

**Note**: Do not make any changes to any directories, excepting the installation and policy directories.

# Additional Configuration for Large Projects

Code analysis can require lots of memory, and unfortunately, the JVM does not pick up the available amount of memory by itself. While tuning Java memory usage is a discipline in its own right, it is usually sufficient to specify the maximum available amount of heap memory using the JVM's `-Xmx` flag. The easiest way to achieve this globally is by setting the environment variable `_JAVA_OPTS` as follows:

```bash
export _JAVA_OPTS="-Xmx$NG"
```
where `$N` is the amount of memory in gigabytes. You can add this line to your shell startup script, e.g., `~/.bashrc` or `~/.zshrc`.
