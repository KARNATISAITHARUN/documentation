# Integrating Jenkins Builds

To integrate Jenkins builds with ShiftLeft and automate code analysis, configure each Jenkins project to execute the `sl analyze` (or `sl analyze --cpg)` shell command.

## Jenkins Integration Options

There are **two ways** to integrate with Jenkins: by configuring a final build step or post build task for *each* Jenkins project you want to submit for analysis to ShiftLeft.

Configuration Option | Description
--- | ---
[Final Build Step](configure-final-build-step.md) | Edit the project build configuration and add the `sl analyze` (or `sl analyze --cpg)` shell command as a final build step.
[Post Build Task](configure-post-build-task.md) | Install the *Hudson Post Build Task Plugin* plugin and add the `sl analyze` (or `sl analyze --cpg)` shell command as a post build task.

## Jenkins Integration Prerequisites

The requirements for integrating Jenkins application builds are as follows:

- [Jenkins installation](https://jenkins.io/changelog/) version 2.78 or later
- Supported application and build tool (see [code analysis requirements](../../../introduction/requirements.md))
- Familiarity with [ShiftLeft Inspect and ShiftLeft Protect](../../../using-inspect-protect/inspect-protect-quick-start.md) 
- ShiftLeft account credentials: **Organization ID** and **Access Token**
Initially these credentials will be provided to you by ShiftLeft. Once you have established your account you can copy them from the **My Profile** page at the ShiftLeft Dashboard.

![Get ShiftLeft Account Credentials](../img/copy-org.png)

## Jenkins Integration Instructions

1. [Install the ShiftLeft CLI](../../using-cli/using-cli.md) on the host where Jenkins is installed.
2. Log in to Jenkins as an administrator.
3. Select **Manage Jenkins > Configure System > Global properties**. 
4. Select (check) **Environment variables** and create the following:
  * Name: `SHIFTLEFT_ORG_ID`| Value: Paste your **Organization ID**
  * Name: `SHIFTLEFT_ACCESS_TOKEN`| Value: Paste your **Access Token**

   ![Adding Jenkins environment variables for ShiftLeft authentication](../img/jenkins-envvars.png)

5. Configure each Jenkins project you want to [analyze](../../../using-inspect-protect/inspect/analyzing-applications-in-ci.md) using one of the following approaches: 
 * [Final Build Step](configure-final-build-step.md), or
 * [Post Build Task](configure-post-build-task.md)
6. Test and [verify Jenkins integration](verify-jenkins-integration.md).

For basic Jenkins administration you can set and inject build env vars as described above. For advanced Jenkins administration, you can use Jenkins secret store to inject the ShiftLeft credentials into builds. Refer to the [Jenkins documentation](https://jenkins.io/doc/) for details.

## Jenkins Integration Demo

Here is a quick video (~ 2 minutes) that demonstrates how to integrate ShiftLeft with an existing Jenkins freestyle project using the [final build step](configure-final-build-step.md) approach: https://youtu.be/bI1GATsD5mc.
