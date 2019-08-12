# Tomcat Integration

You can make a simple modification to your Tomcat startup environment to complete ShiftLeft integration. Make sure you've installed the ShiftLeft CLI on the Tomcat server, and that you have access to the `shiftleft.json` file that was created when you ran sl analyze. The most straightforward integration is simply to customize `catalina.sh` as follows:

1. Add a call to `sl update java-agent`  to ensure that the latest version of the ShiftLeft Java microagent library is present in `$HOME/.shiftleft`
2. export `SHIFTLEFT_CONFIG=(complete path to the shiftleft.json file)`
3. Add the `-javaagent` flag to `JAVA_OPTS`. Point the flag to the location of the current ShiftLeft microagent jarfile e.g. `"-javaagent:$HOME/.shiftleft/sl-microagent-latest.jar"`

Make sure you back up `catalina.sh` before you proceed.
