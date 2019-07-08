# Debugging ShiftLeft Ocular Scripts with JDB

Use the following command to integrate your ShiftLeft Ocular output with the Java Debugger (jdb).

```
export JAVA_OPTS='-Xdebug -Xrunjdwp:transport=dt_socket,address=8002,server=y,suspend=n'
./ocular.sh
```
The first line of output is `Listening for transport dt_socket at address: 8002`. You can now connect to remote debug with the  jdb command line.
