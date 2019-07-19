# Analyzing Method Parameters

You can use ShiftLeft Ocular to analyze your application's method parameters. This tutorial illustrates how to do so
uses [HelloShiftLeft](../../introduction/helloshiftleft.md), a sample application.

Analysis can be driven by the parameters of methods, and their types.

## List all the Parameters of a Given Type in the Code

The query returns a list (`l`) of the parameters containing HttpServletRequest in the name of their respective types (`evalType`).

```
ocular> cpg.parameter.evalType(".*HttpServletRequest.*").name.l
res8: List[String] = List(
  "this",
  "request",
  "request",
  "param0",
  "request",
  "request",
  "request",
  "request",
  ..
.. )
```

## List all the Methods Containing a Parameter of a Given Type

The query lists (`l`) all the methods containing parameters having HttpServletRequest in the name of their respective types (`evalType`).

```
ocular> cpg.parameter.evalType(".*HttpServletRequest.*").method.name.l
res9: List[String] = List(
  "getParameter",
  "getAccountList",
  "doGetLogin",
  "<init>",
  "doGetSearch",
  "getTraceParameter",
  "errorHtml",
  "getStatus",
  "getErrorAttributes",
  "doPostLogin",
  "doGetPrintSecrets",
  "getAttribute",
  "error",
  "getSession",
  "doPostPrintSecrets"
)
```
