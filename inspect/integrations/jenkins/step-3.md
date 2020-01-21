# How to Verify the Inspect-Jenkins Integration

1. Log in to Jenkins, and choose your Jenkins project.
2. Using the left-hand navigation bar, click **Build Now**. You'll see the build progress in the **Build History** box located beneath the navigation bar.
3. When the build is complete, select the build and using the left-hand navigation bar, select **Console Output**.
4. Check that you see the following output below that indicates that ShiftLeft was able to analyze your code successfully:

```bash
Saved config to shiftleft.json
... Done. Submitted for analysis
Wait for 5-10 minutes and load the following URL in your browser:
https://www.shiftleft.io/violationlist/HelloShiftLeft?apps=HelloShiftLeft&isApp=1
POST BUILD TASK : SUCCESS
END OF POST BUILD TASK : 0
Finished: SUCCESS
```

You can click the provided URL in the output to go to the ShiftLeft Dashboard and view your results (if you aren't already logged in, you will be asked to do so).
