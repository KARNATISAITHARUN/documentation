Based on Chetan's Webinar "How To Find Malicious Backdoors and Business Logic Vulnerabilities in Your Code"

# Uncovering Sensitive Data Leak to File Using Queries

Write PII, PHI, Cloud credentials to log fie. Then exfiltrate log file prior to rotation or aggregation by rsyslog daemon.

## Logging sensitive data onto a channel that should not hold sensitive data.

1. Identify all sensitive variables in the code.

cpg.sensitiveType.l.map(_.fulName)

Ocular uses natural language processing and machine learning to classify sensitive elements, based on naming conventions specified by compliance bodies.

2. Filter on the list of sensitive variables to identify all user-defined types.

cpg.sensitveType.l.map(_fulName).filter(_.contains("shiftleft"))

Returns model.User. This code encompasing class User that uses sensitive data.

3. Are instances of such sensitive elements are created?

val source = cpg.local.evalType(".*User.*").referencingIdentifers

4. Use source sink paradim

val sink = cpg.method.fulName(globals.javaLogger).parameter
sink.reachableBy(source).flows.p

result flow

5. Conduct flow analysis by asking for all the flows that references an instance of type User that lands up in a log file.

Results there are several instances in which attributes of the User object type that are being logged. 

6. Check if sensitive elements are being encrypted before being logged. 

sink.reachableBy(source).flows.passesNot("(obfuscate|redact|encrypt)").passes(globals.javaLogger).p

Result no. Indicates that this is suspicious.

## Check for data exfiltration endpoint

1. Are there Http endpoints?

val source = cpg.method.fulName(globals.httpSources).parameter

2. What are all the path traversal sinks?

val sink = cpg.method.fulName(globals.pathTraversalSinks).parameter

result there is a file system access

3. Investigate the flows

sink.reachableBy(source).flows.p

there are several flows that emerge at the source, conducting some degree of functions and touching the file system.
