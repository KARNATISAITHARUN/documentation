# Licensing, Installing and Updating ShiftLeft Ocular

Before installing or updating ShiftLeft Ocular, make sure you have [met all requirements](../../introduction/requirements.md). In addition, for large applications, you may want to optimize ShiftLeft Ocular performance through the appropriate combination of [memory and disk sizes](../about/ocular-memory-size.md).

**Note**: Do not make any changes to any directories, except the installation and policy directories.

Once you have installed or updated ShiftLeft Ocular, you [start ShiftLeft Ocular](starting.md).

There are currently two methods for installing, licensing and updating ShiftLeft Ocular:

* [Download via subscription (Beta)](#downloading-and-installing-shiftleft-ocular).
* [Use the distribution file](#installing-shiftleft-ocular-using-the-distribution-file).

## Additional Configuration for Large Projects

Code analysis can require lots of memory, and unfortunately, the JVM does not pick up the available amount of memory by itself. While tuning Java memory usage is a discipline in its own right, it is usually sufficient to specify the maximum available amount of heap memory using the JVM's `-Xmx` flag. The easiest way to achieve this globally is by setting the environment variable `_JAVA_OPTS` as follows:

```
export _JAVA_OPTS="-Xmx$NG"
```
where `$N` is the amount of memory in gigabytes. You can add this line to your shell startup script, e.g., `~/.bashrc` or `~/.zshrc`.

Refer to the article [Memory Size Recommendations](../about/ocular-memory-size.md) for more information.

## Downloading and Installing ShiftLeft Ocular

To download and license ShiftLeft Ocular via subscription:

1. [Install the ShiftLeft Command Line Interface (CLI)](../../using-cli/install-cli.md). 

2. [Authenticate with ShiftLeft](../../using-cli/authenticating.md).

3. Run the [CLI `sl ocular` command and options](../../using-cli/cli-reference.md). The directoy `~/.shiftleft/ocular` is automatically created, and ShiftLeft Ocular is installed there.

### Updating ShiftLeft Ocular

To update ShiftLeft Ocular, run the command `sl update ocular`. 

If you are updating an existing version of ShiftLeft Ocular via subscription, ShiftLeft Ocular automatically backs up the files in [your workspace](../about/ocular-features.md), including [custom Policies](../../policies/custom-policy.md). You are provided with information on the backup process, and asked to manually review any differences between the new downloaded files and existing files in the backup directory.

## Installing ShiftLeft Ocular Using the Distribution File

The process of installing ShiftLeft Ocular using the distribution file is:

1. Unzip the Ocular distribution ZIP file provided by ShiftLeft, by issuing the following command

   ```
   $ unzip ocular-distribution-[version].zip
   ```
     where `[version]` is the actual version number of the distribution file.

2. Enter the password you received from ShiftLeft. The folder `ocular-distribution` is created.

3. Navigate to the folder `ocular-distribution`. From there, for Linux and MacOS X run the installer using the `./install.sh` command. For the Windows OS, run the installing using the command `.\install.ps1` and then follow the prompt.

     If you don't have permission, first use the  command `chmod +x install.sh` and then run the installer again.

4. Identify where you want to install ShiftLeft Ocular (defaults to `~/bin/ocular` on Linux and MacOS X, and to `C:\Users\$USERNAME\bin\ocular` on Windows).


### Updating ShiftLeft Ocular

You can determine your current version of ShiftLeft Ocular by using the `version` command. To update ShiftLeft Ocular:

1. Repeat steps 1 - 4 from above. 

2. Specify how you want the update to proceed. Enter either `y` to replace an existing ShiftLeft Ocular installation, `n`to not replace the existing installation, `A`to install all ShiftLeft Ocular files, `N` to install no ShiftLeft Ocular files, or `r` to rename the installation directory.
