# Analyzing Methods

This tutorial describes using ShiftLeft Ocular to analyze methods. It uses the [HelloShiftLeft](../../introduction/helloshiftleft.md) sample application to illustrate the steps and results.

ShiftLeft Ocular analyzes your application using methods directly in the code, by helping you navigate to exposed methods. The analysis can be mixed with parameters, method annotations and types to perform more powerful queries on the CPG. As an example, it is possible to navigate to the specific function in the code that accepts data from a publicly exposed Web API endpoint. In modern applications, Java annotations are used to link a given endpoint URI to a function that handles user provided data through the endpoint URI (for example, in Spring Framework, @GetMapping, @PostMapping and @PathVariable are used). 

## List HTTP Endpoint Handler Methods

The query returns a list (`l`) of the methods having a parameter annotated as PathVariable; those methods have no parent callers (which makes them user-controlled API endpoint handlers).

```
ocular> cpg.annotation.name(".*PathVariable.*").parameter.method.filterNot(_.callIn).name.l
res20: List[String] = List(
  "updateCustomer",
  "getLedger",
  "withdrawFromAccount",
  "addInterestToAccount",
  "removeCustomer",
  "getRssForCategorySafe",
  "getRssForCategoryUnsafe",
  "getCustomer",
  "getAccount",
  "depositIntoAccount"
)
```

## List All Methods Filtered by a String in their Name

The query lists (`l`) all the methods containing the `Controller` string in their full name.

```
ocular> cpg.method.fullName(".*Controller.*").name.l
res24: List[String] = List(
  "updateCustomer",
  "init",
  "handleGeneralError",
  "getLedger",
  "<init>",
  "<clinit>",
  "<init>",
  "withdrawFromAccount",
  "addInterestToAccount",
  "<clinit>",
  "getAccountList",
  "index",
  "saveSettings",
  "removeCustomer",
  "doGetLogin",
.. )
```
