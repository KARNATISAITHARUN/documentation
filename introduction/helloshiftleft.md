# About the HelloShiftLeft Sample Application

[HelloShiftLeft](https://github.com/ShiftLeftSecurity/HelloShiftLeft) is a Spring-based sample Java Web application. It 
contains various classes of vulnerabilitie, such as path traversal, sensitive data leaks and code execution. 

HelloShiftLeft is used throughout the ShiftLeft documentation to illustrate examples and to help you learn how to use ShiftLeft.

## Downloading and Compiling HelloShiftLeft

You can download and compile HelloShiftLeft with these commands

```
git clone https://github.com/conikeec/HelloShiftLeft.git 
cd HelloShiftLeft && mvn clean package
```

This creates the HelloShiftLeft JAR `target/hello-shiftleft-0.0.1.jar`. The JAR file is used as the target for analysis.

## Analyzing HelloShiftLeft with ShiftLeft Inspect

If your usual ShiftLeft Inspect command is `java -jar target/hello-shiftleft-0.0.1.jar` and the packaged application is at `target/hello-shiftleft-0.0.1.jar`, then you can wrap the command as

```
sl run 
--app HelloShiftLeft 
--analyze target/hello-shiftleft-0.0.1.jar 
-- java -jar target/hello-shiftleft-0.0.1.jar
```

## Triggering HelloShiftLeft Activity

Use the following script (in a separate terminal window) to trigger HelloShiftLeft activity, and view that activity in the ShiftLeft Dashboard

```
curl -s localhost:8081/customers/1 >/dev/null ;
sleep 1 ;
curl -s localhost:8081/customers/2 >/dev/null ;
curl -s localhost:8081/customers/1 >/dev/null ;
sleep 1 ;
curl -s localhost:8081/customers/1 >/dev/null ;
curl -s localhost:8081/customers/1 >/dev/null ;
sleep 1 ;
curl -s localhost:8081/customers >/dev/null ;
curl -s localhost:8081/saveSettings >/dev/null ;
sleep 1 ;
curl -s localhost:8081/customers >/dev/null ;
sleep 1 ;
curl -s localhost:8081/ >/dev/null ;
curl -s localhost:8081/account/1 >/dev/null ;
curl -s localhost:8081/account >/dev/null ;
curl -s localhost:8081/account/2 >/dev/null ;
curl -s localhost:8081/account >/dev/null ;
curl -s localhost:8081/account/3 >/dev/null ;
curl -s localhost:8081/account/3 >/dev/null ;
curl -s localhost:8081/account/4 >/dev/null ;
curl -s localhost:8081/account/5 >/dev/null ;
curl -s localhost:8081/account/5 >/dev/null ;
curl -s localhost:8081/account/5 >/dev/null ;
curl -s localhost:8081/off >/dev/null ;
sleep 1 ;
done 
```
