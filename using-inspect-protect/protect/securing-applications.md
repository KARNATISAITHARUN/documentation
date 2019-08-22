# Securing Your Applications Using ShiftLeft Protect

ShiftLeft Protect secures applications against exploitation of vulnerabilities by leveraging "code informed" insights. ShiftLeft Protect creates runtime security specifications based on identified vulnerabilities discovered by [ShiftLeft Inspect](../../introduction/products.md) during analysis of your code. ShiftLeft Protect deploys a ShiftLeft Microagent in-memory alongside your application in production (or other environments such as staging, test, UAT). The Microagent is customized to your application's specific shape and weaknesses and connects to a ShiftLeft Proxy server to display events and metrics in the Dashboard.

## Monitoring

You can use ShiftLeft Protect in monitoring mode with your IAST tool.

## Exploit Discovery and Payload Detection

The following exploits and payload vulnerabilities are detected by ShiftLeft Protect:

* Cookie Injection
* Deserialization
* Jackson Deserialization
* Open redirect
* Path Traversal (aka Directory Traversal)
* Sensitive to HTTP
* Sensitive to Log
* SQLi
* Numeric SQLi
* Weak Hash
* XPath Injection
* XSS
* XXE
* RCE

## Malicious Payload Blocking/Sanitization

ShiftLeft Protect not only detects, but also blocks and sanitizes, the following malicious payloads:

* Deserialization
* Jackson Deserialization
* Path Traversal (aka Directory Traversal)
* SQLi
* Numeric SQLi
* XPath Injection
* XSS
* XXE
