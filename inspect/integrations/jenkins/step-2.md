# How to Configure the Integration with Jenkins

Once you have provided your ShiftLeft account parameters to Jenkins, you can proceed with the Inspect integration in one of two ways:

1. **Configuring a final build step that runs Inspect**: edit the project's build configuration and add the `sl analyze` command as the final step of the build

2. **Configuring a post-build task for each Jenkins project that you want analyzed by Inspect**: install the [Post Build Task plugin](https://plugins.jenkins.io/postbuild-task) and add the `sl analyze` command as a Post Build Task

For the following examples, we will be working with the [HelloShiftLeft](https://github.com/ShiftLeftSecurity/helloshiftleft) sample app, but you are free to use your own app as well.

## Option 1: Configure a Final Build Step that Runs Inspect

The instructions for integration Inspect with Jenkins as a final build step differ based on whether you are configuring a Freestyle project or a Pipeline.

### Working with Freestyle Projects

The following steps show you how to configure a Jenkins Freestyle project to build and submit HelloShiftLeft for analysis. (If you would like to submit your own application for analysis in lieu of HelloShiftLeft, you can do so by providing the appropriate links to your repository instead.)

1. Log in to Jenkins with an administrative user account.

2. Choose your Jenkins project. You can create a new Freestyle project, or you can reconfigure an existing Freestyle project.

3. Under **General**, provide a link to your **GitHub project**. If you're using HelloShiftLeft, this will be `https://github.com/shiftLeftSecurity/helloshiftleft/`.

4. Under **Source Code Management**, select **Git**. This opens up the **Repositories** area. Provide the `https://github.com/shiftLeftSecurity/helloshiftleft/` as your **Repository URL**.

5. Under **Build Triggers**, make sure you select **Poll SCM**.

6. Under **Build**, click **Add build step**, and in the drop-down that appears, select **Invoke top-level Maven targets** since we are using Maven to build HelloShiftLeft. You will be asked to provide your **Goals**; enter `clean package`.

7. Click **Add build step** again, and in the drop-down that appears, select **Execute shell**. Provide the following **Command**:

```bash
#!/bin/bash
/usr/local/bin/sl analyze --app HelloShiftLeft --java target/hello-shiftleft-0.0.1.jar
```

Click **Save**. At this point, you are ready to build and test your project.

### Working with Pipelines

To have Jenkins run the `sl analyze` (or `sl analyze --cpg` for Java/Scala apps) shell command as a final build step in a Jenkins Pipeline project, you will need to add the following to your Jenkinsfile):

```bash
stage('SLAnalyze') {
    dir("<path_to_your_project_package>") {
        sh '/usr/local/bin/sl analyze --app HelloShiftLeft --java target/hello-shiftleft-0.0.1.jar'
    }
}
```

Click **Save**.

## Option 2: Configure a Post-Build Task for Jenkins Projects to be Analyzed by Inspect

You can configure a post-build task for each target project that submits your code to ShiftLeft for analysis.

To begin, [install](https://jenkins.io/doc/book/managing/plugins/#installing-a-plugin) the [Post Build Task plugin](https://plugins.jenkins.io/postbuild-task):

1. Log in to the Jenkins Dashboard and go to **Manage Jenkins** > **Manage Plugins**. Select the **Available** tab on the Plugin Manager screen.
2. In the **Filter**, enter "Post Build Task". Check the **Install** box next to the plugin in the results.
3. At the bottom of the page, choose **Install without restart**.

You will be redirected to a progress page that tells you the installation status. When the plugin is installed, click **Go back to the top page**. You can check for installed plugins at any time by going to **Manage Jenkins** > **Manage Plugins**, then clicking **Installed**.

### Adding a Post Build Task

Once you've installed the Post Build Task plugin, you can add a post-build action to run Inspect:

1. Choose your Jenkins project. You can create a new Freestyle project, or you can reconfigure an existing Freestyle project. (If you would like to submit your own application for analysis in lieu of HelloShiftLeft, you can do so by providing the appropriate links to your repository instead.)

2. Under **General**, provide a link to your **GitHub project**. If you're using HelloShiftLeft, this will be `https://github.com/shiftLeftSecurity/helloshiftleft/`.

3. Under **Source Code Management**, select **Git**. This opens up the **Repositories** area. Provide the `https://github.com/shiftLeftSecurity/helloshiftleft/` as your **Repository URL**.

4. Under **Build Triggers**, make sure you select **Poll SCM**.

5. Under **Build**, click **Add build step**, and in the drop-down that appears, select **Invoke top-level Maven targets** since we are using Maven to build HelloShiftLeft. You will be asked to provide your **Goals**; enter `clean package`.

6. Under **Post-build actions** click **Add post-build action** and in the drop-down menu that appears, select **Post build task**. In the configuration area that appears, provide the following as your Script:

```bash
#!/bin/bash
mvn clean package
/usr/local/bin/sl analyze --app HelloShiftLeft --java target/hello-shiftleft-0.0.1.jar
```

Click **Save**. At this point, you are ready to build and test your project.
