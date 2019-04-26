# Installing the ShiftLeft Command Line Interface (CLI)

If you haven't already downloaded the CLI as part of the [Quick Start process](../../using-inspect-protect/inspect-protect-quick-start.md) when you first logged into ShiftLeft, you can do so by running the appropriate CLI install command.

#### Linux and MacOS X

```
curl https://cdn.shiftleft.io/download/sl >/usr/local/bin/sl && chmod a+rx /usr/local/bin/sl
```
`sl` automatically updates so that you don't have to reinstall the CLI whenever there are new features or fixes (`curl` or `wget` are required for automatic updates). You can disable automatic updating by setting the environment variable `SHIFTLEFT_NO_AUTO_UPDATE=true` when running `sl`.

#### Windows .NET Framework

In PowerShell, you can issue the following command to download an installer that enables `sl` and the ShiftLeft Protect for .NET Microagent

```
Invoke-WebRequest -Uri https://cdn.shiftleft.io/download/installer-dotnet-framework-latest-windows-x64.zip -UseBasicParsing -OutFile sl-latest-windows-x64.zip
```
Alternatively,you can use a browser to download the file.

#### Windows .NET Core

In PowerShell, you can issue the following command to download an installer that enables `sl` and the ShiftLeft Protect for .NET Microagent

```
Invoke-WebRequest -Uri https://cdn.shiftleft.io/download/installer-dotnet-core-latest-windows-x64.zip -UseBasicParsing -OutFile sl-latest-windows-x64.zip
```
Alternatively you can use a browser to download the file.
