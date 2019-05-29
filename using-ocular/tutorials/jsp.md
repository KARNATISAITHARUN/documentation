# Using ShiftLeft Ocular with JavaServer Pages (JSP)

You can use ShiftLeft Ocular to examine the software elements and flows in your JSP to identify complex business logic 
vulnerabilities that can't be scanned for automatically. Note that this support does not include JSP Expression Language. 

This tutorial illustrates the use of ShiftLeft Ocular with JSP, based on the sample project Java Vulnerable Lab. The process is:

1. [Download and build the Java Vulnerable Lab](https://github.com/CSPF-Founder/JavaVulnerableLab).

2. Start ShiftLeft Ocular.

3. In the ShiftLeft Ocular shell, create the CPG, the Security Profile overlay and the query _findings_ as follows:

```
ocular> createCpgAndSp("JavaVulnerableLab.war")
ocular> cpg.finding.p 
```

The query returns a comprehensive list of findings, each with its own title, score, description and associated vulnerable flow. 

```
-------------------------------------
Title: SQL Injection: HTTP data to SQL database via `request` 
Score: 9
Categories: [a1-injection]
Flow ids: [9212286253158463995]
Description: HTTP data is used in a SQL query without undergoing escaping or validation. This could allow an attacker to read sensitive data from the database, modify its content or gain control over the server.
```

## Countermeasures

This vulnerability can be prevented by using prepared statements on the HTTP data.

```
-------------------------------------
_________________________________________________________________________________________________________________________
 | tracked                     | lineNumber| method               | file                                                  |
 |========================================================================================================================|
 | request                     | 103       | _jspService          | io/shiftleft/precompiledjsp/admin/manageusers_jsp.java|
 | request                     | 266       | _jspService          | io/shiftleft/precompiledjsp/admin/manageusers_jsp.java|
 | this                        | N/A       | getParameter         | javax/servlet/http/HttpServletRequest.java            |
 | ret                         | N/A       | getParameter         | javax/servlet/http/HttpServletRequest.java            |
 | request.getParameter("user")| 266       | _jspService          | io/shiftleft/precompiledjsp/admin/manageusers_jsp.java|
 | param1                      | N/A       | <operator>.assignment| N/A                                                   |
 | param0                      | N/A       | <operator>.assignment| N/A                                                   |
 | user                        | 266       | _jspService          | io/shiftleft/precompiledjsp/admin/manageusers_jsp.java|
 | user                        | 267       | _jspService          | io/shiftleft/precompiledjsp/admin/manageusers_jsp.java|
 | param0                      | N/A       | append               | java/lang/StringBuilder.java                          |
 | ret                         | N/A       | append               | java/lang/StringBuilder.java                          |
 | $r33.append(user)           | 267       | _jspService          | io/shiftleft/precompiledjsp/admin/manageusers_jsp.java|
 | param1                      | N/A       | <operator>.assignment| N/A                                                   |
 | param0                      | N/A       | <operator>.assignment| N/A                                                   |
 | $r34                        | 267       | _jspService          | io/shiftleft/precompiledjsp/admin/manageusers_jsp.java|
 | $r34                        | 267       | _jspService          | io/shiftleft/precompiledjsp/admin/manageusers_jsp.java|
 | this                        | N/A       | append               | java/lang/StringBuilder.java                          |
 | ret                         | N/A       | append               | java/lang/StringBuilder.java                          |
 | $r34.append("\'")           | 267       | _jspService          | io/shiftleft/precompiledjsp/admin/manageusers_jsp.java|
 | param1                      | N/A       | <operator>.assignment| N/A                                                   |
 | param0                      | N/A       | <operator>.assignment| N/A                                                   |
 | $r35                        | 267       | _jspService          | io/shiftleft/precompiledjsp/admin/manageusers_jsp.java|
 | $r35                        | 267       | _jspService          | io/shiftleft/precompiledjsp/admin/manageusers_jsp.java|
 | this                        | N/A       | toString             | java/lang/StringBuilder.java                          |
 | ret                         | N/A       | toString             | java/lang/StringBuilder.java                          |
 | $r35.toString()             | 267       | _jspService          | io/shiftleft/precompiledjsp/admin/manageusers_jsp.java|
 | param1                      | N/A       | <operator>.assignment| N/A                                                   |
 | param0                      | N/A       | <operator>.assignment| N/A                                                   |
 | $r36                        | 267       | _jspService          | io/shiftleft/precompiledjsp/admin/manageusers_jsp.java|
 | $r36                        | 267       | _jspService          | io/shiftleft/precompiledjsp/admin/manageusers_jsp.java|
 | param0                      | N/A       | executeUpdate        | java/sql/Statement.java 
 
```

## Identifying the Vulnerable Flow Through JSP

In the flow associated with the vulnerability, the call starts from `io/shiftleft/precompiledjsp/admin/manageusers_jsp.java`, which is essentially a precompiled JSP created by ShiftLeft Ocular as part of the process of generating the CPG . The JSP is derived from the file https://github.com/CSPF-Founder/JavaVulnerableLab/blob/master/src/main/webapp/admin/manageusers.jsp. All precompiled files are part of the `io.shiftleft.precompiledjsp` namespace. This is one way to identify and audit JSP-only methods while investigating data flows using ShiftLeft Ocular. For example, the following query lists the methods that are part of the precompiled JSP from Java Vulnerable Lab:

```
ocular> cpg.namespace.name("io.shiftleft.precompiledjsp.vulnerability.sqli").method.fullName.l
```

Note that currently the line numbers don't correspond to the actual lines in the JSP files, since the information is derived from compiled Java files. This inconsistency will be fixed in a later release of ShiftLeft Ocular.
