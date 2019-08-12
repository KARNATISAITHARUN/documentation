# Wildfly Configuration

Make sure you've installed the ShiftLeft CLI on the WildFly server, and that you have access to the shiftleft.json file for the specific application asset (jar, war, or ear file) you've deployed - this file was created when you ran sl analyze against the asset.  Also please make sure that the $HOME and $JBOSS_HOME environment variables are properly set.

The simplest way to proceed is to modify standalone.sh (or standalone.conf if present):

1. Add a call to 'sl update java-agent' to ensure that the latest version of the ShiftLeft Java microagent library is present in $HOME/.shiftleft

2. export SHIFTLEFT_CONFIG=(complete path to the shiftleft.json file)

3. Add the -javaagent flag to JAVA_OPTS. Point the flag to the location of the current ShiftLeft microagent jarfile e.g. "-javaagent:$HOME/.shiftleft/sl-microagent-latest.jar". **Note: there is an interaction between the ShiftLeft agent and WildFly's logging configuration, so please note the following code sample carefully:
**

```bash
SL_OPTS=-javaagent:$HOME/.shiftleft/sl-microagent-latest.jar

export JAVA_OPTS="$JAVA_OPTS -Xbootclasspath/p:$JBOSS_HOME/modules/system/layers/base/org/jboss/logmanager/main/jboss-logmanager-2.0.4.Final.jar -Djava.util.logging.manager=org.jboss.logmanager.LogManager $SL_OPTS"
```


Make sure you back up standalone.conf or standalone.sh before you proceed.
