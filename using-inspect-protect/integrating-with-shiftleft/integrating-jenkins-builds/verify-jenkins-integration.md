# Verify Jenkins Integration

Once you have [installed the CLI](../../../using-cli/install-cli.md) and [authenticated with ShiftLeft](../../../using-cli/authenticating.md), and configured a [final build step](configure-final-build-step.md) or [post build task]((configure-post-build-task.md)) in Jenkins, verify build integration with ShiftLeft as follows:

1. Select the Jenkins project where you have configured the `sl analyze` (or `sl analyze --cpg`) shell command as a [final build step](configure-final-build-step.md) or [post build task](configure-post-build-task.md).
2. Click **Build Now**.
3. Select the build.
4. Select **Console Output**.
5. Verify that you see successful output as shown below.

   ```
   Shiftleft CLI Displaying directory tree for: /Users/user/.jenkins/workspace/HelloShiftLeft/.shiftleft
   Shiftleft CLI - - --------------------------------
   Shiftleft CLI Total bytes:   67501128
   Shiftleft CLI Total files:   83
   Shiftleft CLI Total folders: 2
   Shiftleft CLI - - --------------------------------
   Shiftleft CLI Successfully Completed task
   Shiftleft CLI Elapsed Time (in seconds): 418
   POST BUILD TASK : SUCCESS
   END OF POST BUILD TASK : 0
   Finished: SUCCESS
   ```

6. Log in to the ShiftLeft Dashboard for your organization. 
7. After analysis is complete, verify that you see an App Card for the analyzed app.
8. Select the App Card and view the App Summary for the analyzed app.
