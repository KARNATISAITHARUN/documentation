# Java Vulnerable Lab

This tutorial is based on the popular Java project, [Java Vulnerable Lab](https://github.com/CSPF-Founder/JavaVulnerableLab), a benchmarking application for
vulnerability discovery tools. The project includes different sample vulnerabilities, such as 
typical injection vulnerabilities like SQL injection. 

## Prerequisite

[Install](../getting-started/installation.md) and [start](../getting-started/starting.md) ShiftLeft Ocular.

## Running the Java Vulnerable Lab Sample Application

The Java Vulnerable Lab WAR file is included in the ShiftLeft Ocular distribution for your convenience.

Focusing on an SQL injection vulnerability in the `EmailCheck.java`, this
controller also consumes POST requests. The GET
request that ends up in a SQL query is of particular interest.

```
protected void processRequest(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    response.setContentType("application/json");
    PrintWriter out = response.getWriter();
    try {
           Connection con=new DBConnect().connect(getServletContext().getRealPath("/WEB-INF/config.properties"));
           String email=request.getParameter("email").trim();
           JSONObject json=new JSONObject();
            if(con!=null && !con.isClosed())
            {
                ResultSet rs=null;
                Statement stmt = con.createStatement();  
                rs=stmt.executeQuery("select * from users where email='"+email+"'");
            [...]
}

@Override
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    processRequest(request, response);
}
```

The variable `request` is of type
`HttpServletRequest` and is part of the `doGet` signature. The object is passed to `processRequest`, where the parameter
`email` is read as a string. The variable is concatenated to
the SQL query without prior checks. This leads to an SQL injection. This vulnerability is identified using
ShiftLeft Ocular.

## Generating CPGs and Security Profiles

You create the application's [Code Property Graph (CPG)](../getting-started/create-cpg.md) and [Security Profile](../getting-started/generate-sp.md) using the command

```
ocular> createCpgAndSp("subjects/JavaVulnerableLab.war") -o cpg.bin.zip
```

This command creates a file named `cpg.bin.zip` containing the CPG and Security Profile in a binary format. 

## Generating an Initial Analysis Report

ShiftLeft Ocular lets you query CPGs and Security Profiles, both [interactively and non-interactively](../about/modes.md). An example of non-interactive querying is the script in `scripts/report.sc`

```
@main def exec(spFilename: String, outFilename: String) = {
  loadSp(spFilename)
  sp.findings.sortedByScore.l |> outFilename
}
```

This script loads the Security Profile at `spFilename`, and evaluates the expression `sp.findings.sortedByScore.l` to obtain a list of findings sorted by score. The list is piped to the file `outFilename` via the `|>` operator.

You can also run this interactive script, using the [Ocular Query Language (OQL)](https://ocular.shiftleft.io/api/).
```
ocular> ./ocular.sh --script scripts/report.sc --params spFilename=javavulnerablelab.sp,outFilename=report.txt
```

As a result, the text file `report.txt` is generated containing all findings in a human-readable format. Take a look at one of the findings to get an idea of the type of information the report contains

```
Title: http-to-sql
Score: 9.0
Categories: [a1-injection]
Flow ids: [1715]
Description: Attacker controlled data is used in a SQL query without undergoing escaping or validation. This could allow an attacker to read sensitive data from the database or modify its content.See http://cwe.mitre.org/data/definitions/89.html
-------------------------------------
Flow 0:
IO Tags: Set(http) -> Set(sql)
Data Tags: Set(DATA_TYPE: pii, DATA_TYPE: contact, DATA_LANGUAGE: SQL)
routes: 
trigger methods:
org.cysecurity.cspf.jvl.controller.EmailCheck.doGet:void(javax.servlet.http.HttpServletRequest,javax.servlet.http.HttpServletResponse)
org.cysecurity.cspf.jvl.controller.EmailCheck.doPost:void(javax.servlet.http.HttpServletRequest,javax.servlet.http.HttpServletResponse)

Primary flow:
 ____________________________________________________________________________________________________________________________________________________________________________________________________________________
 | param     | type                                 | method        | signature                                                                                                                                      |
 |===================================================================================================================================================================================================================|
 | request(1)| javax.servlet.http.HttpServletRequest| doGet         | org.cysecurity.cspf.jvl.controller.EmailCheck.doGet:void(javax.servlet.http.HttpServletRequest,javax.servlet.http.HttpServletResponse)         |
 | request(1)| javax.servlet.http.HttpServletRequest| processRequest| org.cysecurity.cspf.jvl.controller.EmailCheck.processRequest:void(javax.servlet.http.HttpServletRequest,javax.servlet.http.HttpServletResponse)|
 | this(0)   | javax.servlet.http.HttpServletRequest| getParameter  | javax.servlet.http.HttpServletRequest.getParameter:java.lang.String(java.lang.String)                                                          |
 | return(-1)| java.lang.String                     | getParameter  | javax.servlet.http.HttpServletRequest.getParameter:java.lang.String(java.lang.String)                                                          |
 | this(0)   | java.lang.String                     | trim          | java.lang.String.trim:java.lang.String()                                                                                                       |
 | return(-1)| java.lang.String                     | trim          | java.lang.String.trim:java.lang.String()                                                                                                       |
 | email     | java.lang.String                     | processRequest| org.cysecurity.cspf.jvl.controller.EmailCheck.processRequest:void(javax.servlet.http.HttpServletRequest,javax.servlet.http.HttpServletResponse)|
 | param0(1) | java.lang.String                     | append        | java.lang.StringBuilder.append:java.lang.StringBuilder(java.lang.String)                                                                       |
 | return(-1)| java.lang.StringBuilder              | append        | java.lang.StringBuilder.append:java.lang.StringBuilder(java.lang.String)                                                                       |
 | this(0)   | java.lang.StringBuilder              | append        | java.lang.StringBuilder.append:java.lang.StringBuilder(java.lang.String)                                                                       |
 | return(-1)| java.lang.StringBuilder              | append        | java.lang.StringBuilder.append:java.lang.StringBuilder(java.lang.String)                                                                       |
 | this(0)   | java.lang.StringBuilder              | toString      | java.lang.StringBuilder.toString:java.lang.String()                                                                                            |
 | return(-1)| java.lang.String                     | toString      | java.lang.StringBuilder.toString:java.lang.String()                                                                                            |
 | param0(1) | java.lang.String                     | executeQuery  | java.sql.Statement.executeQuery:java.sql.ResultSet(java.lang.String)                                                       |
```

Along with other findings, notice an SQL injection vulnerability
triggerable via HTTP with a score of 9.0. Findings are scored in order
to allow for filtering. Findings also include a human-readable
description that further characterizes the potential vulnerability, as
well as the information flows associated with the vulnerability.

In this example, a single information flow is associated with the
vulnerability. ShiftLeft Ocular identifies that the parameter
`request` of the method `doGet` is attacker-controlled with high
probability, since it is an HTTP request parameter. Tracking the flow of
`request`, the variable is passed into the method `processRequest`
where it is Base64-decoded and used in the initialization of a
`ByteArrayInputStream`. This input stream is itself used to initialize
an `ObjectInputStream`. Lastly, the `readObject` method is invoked on the
tainted input stream, resulting in the deserialization of
attacker-controlled data. The flow description additionally provides
HTTP input routes when possible (`/admin/login` in this case), and
externally triggerable methods to invoke the vulnerable flow
(`doPostLogin` in this example).

## Interactively Exploring and Filtering the Security Profile

Using interactive mode, load the Security Profile "javavulnerablelab.sp" by 

```
ocular> loadSp("javavulnerablelab.sp")
```

This creates an object named `sp` that provides access to the Security
Profile. Now enter

```
 sp.findings.<TAB>
 !=             category       equals         getClass       isInstanceOf   raw            scoreAtMost    spTestsFlows   |>
 ==             dedup          filter         hashCode       l              score          size           title
 asInstanceOf   description    flatMap        ioFlows        map            scoreAtLeast   sortedByScore  toString
```

to obtain a list of possible operations that can be executed on
findings. In particular, findings support the `scoreAtLeast` method,
which allows findings to be filtered such that only findings scored
above or equal to a threshold are returned. For example

```
ocular> sp.findings.scoreAtLeast(8).l.size
```

returns only findings with a score of at least 8. Note that the
query language is lazily evaluated, that is,
`sp.findings.scoreAtLeast(8)` only yields in an expression, and it is
only evaluated as it is converted to a list via the `l` directive (a
shorthand for "toList").

All string properties support regular expressions. You
can obtain all findings related to serialization by

```
ocular> sp.findings.title(".*serialize.*").l
```

All lists support the functional combinators of the Scala language.
Moreover, the functional combinators `filter`, `map`, and
`flatMap` are provided directly for expression of the DSL. For example, instead of
using the built-in method `scoreAtLeast`, the same effect can be
achieved via a filter operation 

```
sp.findings.filter(_.score >= 8).size
```

This allows more complex filtering rules to be expressed via lambdas

```
ocular> sp.findings.filter(x =>  x.score >=8 && x.categories.contains("a1-injection")).l
```

returns only the findings with a score greater or equal to 8, where
the finding's categories includes "a1-injection".
