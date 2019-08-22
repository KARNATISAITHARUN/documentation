# Installing the ShiftLeft Command Line Interface (CLI)

The ShiftLeft CLI is used with [ShiftLeft Inspect to analyze your application for vulnerabilities](../inspect/analyzing-applications.md) and with [ShiftLeft Protect to monitor and protect your application at runtime](../protect/run-protect.md). The tool is named `sl`. There are three methods for installing the ShiftLeft CLI: 

* [Using the Welcome page](#using-the-welcome-page).
* [Downloading the installer](#downloading-the-cli-installer). 
* [Running the appropriate CLI install command](cli-reference.md).

The ShiftLeft CLI is invoked using the following syntax:

```
sl [global options] command [command options] [arguments...]
```

Once you have installed the ShiftLeft CLI, make sure you have [authenticated the CLI with your ShiftLeft Account](../using-cli/authenticating.md). Refer to the [CLI Command Reference](cli-reference.md) for additional information.

## Using the Welcome Page

You can use the Welcome page to install the CLI only if you are running ShiftLeft on either the Linux or MacOS X operating systems. The [Welcome page](https://www.shiftleft.io/dashboard) is displayed the first time you log into ShiftLeft.

1. Click the Download the ShiftLeft CLI button.

   ![Click to Download the CLI](img/download-cli.jpg)

2. Add the CLI to your system path. The process is different for each operating system, as explained below.

3. Verify the CLI installation by typing `sl help`.

### Adding the CLI to your System Path for Linux and MacOS X

Make sure that `/usr/local/bin` is in your `$PATH`.

Extract the `sl` or `sl.exe` binary and then add the directory location of the `sl` binary to your `$PATH` (or manually copy it to `/usr/local/bin`).

### Adding the CLI to your System Path for Windows .NET

Note that for Windows .NET there are two variants: .NET Framework and .NET Core. Make sure to pick the right variant for your project.

After you have downloaded the appropriate installer, unzip the file and invoke the installer. If you are running the installer from the terminal, add `--no-prompt` to disable waiting for user input.

## Downloading the CLI Installer

The process is different, depending on your operating system.

### Linux and MacOS X

Use the command

```
curl https://cdn.shiftleft.io/download/sl >/usr/local/bin/sl && chmod a+rx /usr/local/bin/sl
```

The ShiftLeft CLI automatically updates so that you don't have to reinstall the CLI whenever there are new features or fixes (`curl` or `wget` are required for automatic updates). You can disable automatic updating by setting the environment variable `SHIFTLEFT_NO_AUTO_UPDATE=true` when running `sl`.

### Windows .NET Framework

In PowerShell, you can issue the following command to download an installer that enables the ShiftLeft CLI and ShiftLeft Protect for .NET Framework

```
Invoke-WebRequest -Uri https://cdn.shiftleft.io/download/installer-dotnet-framework-latest-windows-x64.zip -UseBasicParsing -OutFile sl-latest-windows-x64.zip
```
Alternatively, you can use a browser to download the file.

### Windows .NET Core

In PowerShell, you can issue the following command to download an installer that enables the ShiftLeft CLI

```
Invoke-WebRequest -Uri https://cdn.shiftleft.io/download/installer-dotnet-core-latest-windows-x64.zip -UseBasicParsing -OutFile sl-latest-windows-x64.zip
```
Alternatively you can use a browser to download the file.
