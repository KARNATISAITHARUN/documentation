# Integrating with Bamboo

Bamboo is a continuous integration and deployment tool that ties automated builds, tests and releases together in a single workflow. You can integrate your Bamboo project with ShiftLeft Inspect for automated code analysis. To do so, configure each Bamboo project to execute the ShiftLeft Inspect shell command [`sl analyze`](../inspect/analyzing-applications.md).

## Bamboo Integration Prerequisites

The prerequisites for integrating Bamboo application builds are:

- [Bamboo installation](https://confluence.atlassian.com/bamboo/bamboo-installation-guide-289276785.html).
- [ShiftLeft requirements](../../introduction/requirements.md).
- Familiarity with [ShiftLeft Inspect](../../using-inspect-protect/inspect-protect-quick-start.md).
- ShiftLeft account credentials: **Organization ID** and **Access Token**. When you first log into ShiftLeft, these credentials are provided. Once you have established your account, you can obtain your Organization ID and Access Token from the [**Account Settings** page of the ShiftLeft Dashboard](https://www.shiftleft.io/user/profile).

![ShiftLeft Account Credentials](img/credentials.jpg)

## Configuring the Bamboo Build

You use the CLI to configure Bamboo:

1. [Install the ShiftLeft Command Line Interface (CLI)](../../using-inspect-protect/using-cli/install-cli.md) on the Bamboo host.
2. Log into the Bamboo server as an administrator. 
3. Create the following **Environment variables**

  * Name: `SHIFTLEFT_ORG_ID`| Value: <**Organization ID**>
  * Name: `SHIFTLEFT_ACCESS_TOKEN`| Value: <**Access Token**>

4. Create a shell script in Bamboo

   a. Select the project.
   
   b. Go to the **Tasks** tab.
   
   c. Add a new task.
   
   d. Select **Script**.
   
   e. Make sure the task you are adding is the last task in the list of build tasks.
   
   f. Enter a task description, such as **SL Analyze**.
   
   g. Enter the script body `/usr/local/bin/sl analyze` or `/usr/local/bin/sl analyze - -cpg`.
   
   h. Enter working subdirectory `/directory/where/built/project/packages/are/located`.
