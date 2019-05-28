# Analyzing Libraries

By default, `java2cpg` recursively unpacks JARs to create one Code
Property Graph (CPG) that captures all components of the application. In
order to exclude libraries and frameworks, `java2cpg` applies a
blacklist of common libraries (e.g., Apache-commons libraries). This
means that, by default, `java2cpg` returns an empty CPG when run
on a blacklisted dependency.

To analyze a library, disable blacklisting with the `nb` flag.
For example, create a CPG for commons-io
([mvnrepository](http://central.maven.org/maven2/commons-io/commons-io/2.5/commons-io-2.5.jar))
as 

```
./java2cpg.sh -f protobufzip -o commons-io-2.5.bin.zip commons-io-2.5.jar -nb

```

## Attack Surface for Libraries

For applications, I/O sources and sinks can be automatically marked using 
the Policy, e.g., mark HTTP parameters, input read from files, or
data going into sockets. For libraries, additionally allow
marking of all public method parameters as potential sources of
attacker-controlled data. This can be achieved with the cpg2sp `-m` flag

```
 time ./cpg2sp.sh --cpg commons-io-2.5.bin.zip -o commons-io-2.5.sp -m 
```

This flag marks every parameter of `public` functions as source, as `attacker-controlled`. This allows you to define
conclusions to detect attacker-controlled data
that reaches deserializers.

```
CONCLUSION attacker-to-deserializer = FLOW DATA (attacker-controlled) -> IO (deserializer)
WHEN CONCLUSION attacker-to-deserializer => EMIT {
    title: "Attacker controlled data to deserialization",
    description: "Attacker controlled data is deserialized in this flow. ...",
    category: "a1-injection",
    score: "8.0"
}
```

This rule is included in ShiftLeft Ocular's default Policy.
