# Using ShiftLeft Ocular with MISRA C Applications

[MISRA C](https://www.misra.org.uk/) is a set of software development guidelines for the C programming language developed by the Motor Industry Software Reliability Association (MISRA). Its aims are to facilitate code safety, security, portability and reliability in the context of embedded systems. You can use ShiftLeft Ocular to examine the software elements and flows in your MISRA C applications to identify complex business logic vulnerabilities that can't be scanned for automatically.

This tutorial illustrates the capabilities of ShiftLeft Ocular to check your code base for MISRA violations, through the use of the rules 17.6 and 22.422.4
of the MISRA 2012 standard.


## Mandatory Rule 17.6 

Rule 17.6 states that the declaration of an array parameter should not contain the `static` keyword between the []. This rule covers the possibility of developers assuming a fixed number of parameters provided to a function. Developers do so to increase performance, but with the risk that a function is called without the correct amount of parameters. 

### Test Code

```C
int fun1(int x[static 10]) {}

int arr[ 20 ];
void main() {
   fun1(arr);
}
```

### ShiftLeft Ocular Query

Use the following query to check for violations in the currently loaded Code Property Graph (CPG).


```scala
def checkMisra2012_176() {
        cpg.method                                  // all methods
           .parameter                               // all parameter
           .filter(_.typ                            // filter the type of the parameter
                    .name(".* \\[.*static.*\\]")    // that contains `static`
                  )
           .l                                       // make a list
           .foreach { violation =>                  // print line number file and parametername
                         val lineNumber = violation.location.lineNumber.get
                         val fileName = violation.location.filename
                         val parameter = violation.code
                         val methodName = violation.start.method.name.l.headOption.getOrElse("")
                         println("[-] Test case 2012 17.6: Found violation on line %d in file %s inside the parameter %s of method %s".format(lineNumber, fileName, parameter, methodName))
                    }
}
```

### Result

```
ocular> checkMisra2012_176
[-] Test case 2012 17.6: Found violation on line 1 in file ../../testcases/misra/2012/176.c inside the parameter int x[static 10] of method fun1
```


## Mandatory Rule 22.4 

Rule 22.4 determines that in MISRA C, there should be no attempt to write to a stream which has been opened as read-only. Writing to a file that is only opened to read causes undefined behavior and thus should be avoided.

### Test Code

```C
#include <stdio.h>
void fn ( void )
{
    FILE *fp = fopen ( "testfile", "r" );
    fprintf(fp, "violation");
}

void main() {
    fn();
}
```

### ShiftLeft Ocular Query

Use the following query to check for violations in the currently loaded Code Property Graph (CPG).

```scala
def checkMisra2012_224() {
    // we start from the return 
    // of all methods named `fopen`
    // and whose second argument is `"r"`
    val source = cpg.method
                    .filter(_.name("fopen")
                             .parameter
                             .argument
                             .order(2)
                             .isLiteral
                             .code("\"r\"")
                           )
                    .methodReturn

    // sink is a file writing method
    // fprintf in our case but could
    // also be fwrite, fputs, .. 
    val sink = cpg.method
                  .name("fprintf")
                  .parameter
    
    // find all flows and print them
    println(sink.reachableBy(source).flows.p)
}
```

### Result
```
ocular> checkMisra2012_224
2019-07-04 13:24:34.550 [main] INFO mainTasksSize: 2, reachedSource: 1,
List( ________________________________________________________________________________________________
 | tracked                  | lineNumber| method               | file                            |
 |===============================================================================================|
 | ret                      | N/A       | fopen                | N/A                             |
 | fopen ( "testfile", "r" )| 3         | fn                   | ../../testcases/misra/2012/224.c|
 | p2                       | N/A       | <operator>.assignment| N/A                             |
 | p1                       | N/A       | <operator>.assignment| N/A                             |
 | fp                       | 3         | fn                   | ../../testcases/misra/2012/224.c|
 | fp                       | 4         | fn                   | ../../testcases/misra/2012/224.c|
 | p1                       | N/A       | fprintf              | N/A                             |
)
```
