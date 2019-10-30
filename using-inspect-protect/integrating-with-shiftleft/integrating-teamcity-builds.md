# Integrating with TeamCity

TeamCity is a build management and continuous integration server. You can integrate your TeamCity project with ShiftLeft Inspect for automated code analysis and profiling. To do so, configure each TeamCity project to execute the ShiftLeft Inspect shell command [`sl analyze`](../inspect/analyzing-applications.md).
The process of integrating GoCD with ShiftLeft Inspect is.

1. [Install the ShiftLeft Command Line Interface (CLI)](#install-shiftleft-cli).
2. [Configure the build](#configure-the-build).


## TeamCity Integration Prerequisites

The prerequisites for integrating TeamCity application builds are:

- [TeamCity installation](https://www.jetbrains.com/teamcity/download/).
- [ShiftLeft requirements](../../introduction/requirements.md).
- Familiarity with [ShiftLeft Inspect and ShiftLeft Protect](../../using-inspect-protect/inspect-protect-quick-start.md).
- ShiftLeft account credentials: **Organization ID** and **Access Token**. When you first log into ShiftLeft, these credentials are provided. Once you have established your account, you can obtain your Organization ID and Access Token from the [**Account Settings** page of the ShiftLeft Dashboard](https://www.shiftleft.io/user/profile).

![ShiftLeft Account Credentials](img/credentials.jpg)


## Install ShiftLeft CLI

To install the ShiftLeft CLI:

1. [Install the ShiftLeft CLI](../using-cli/install-cli.md) on the host where GoCD server is installed.
2. [Authenticate with ShiftLeft](../using-cli/authenticating.md).
2. Log in to GoCD server as an administrator. 
3. Create the following **Environment variables**
 * Name: `SHIFTLEFT_ORG_ID`| Value: <**Organization ID**>
 * Name: `SHIFTLEFT_ACCESS_TOKEN`| Value: <**Access Token**>

## Configure the Build

To create a TeamCity job that executes SL commands:

1. Login to TeamCity.
2. Go to **Project**.
3. Edit **Settings**.
4. Go to **General Settings > Build Configurations**.
5. Click **Edit**.
6. Select **Build Step**.
7. Add **Build Step**.
8. Configure the new build step as follows (see screenshot below):
 - Runner type: Select **Command Line**
 - Step name: **SL Analyze** (for example)
 - Execute step: Select **If all previous steps finished successfully** 
 - Working directory: Enter `/directory/where/built/project/packages/are/located` 
 - Run: Select **Executable with parameters**
 - Command executable: Enter `/usr/local/bin/sl`
 - Command parameters: `analyze` (or `analyze --cpg`)
9. Save the configuration.
10. Run the build and verify analysis success in the command output.
