# Scanning for Types and Packages

This example shows you how to use ShiftLeft Ocular to examine types (classes) and packages, including the methods which use or call them. It uses [HelloShiftLeft](../../introduction/helloshiftleft.md) to demonstrate the steps and results.

## Listing all the Types Used in the Code

The query returns a list (`l`) of the full name of all the type declarations (`typeDecl`) in the Code Property Graph (CPG).

```
ocular> cpg.typeDecl.fullName.l
res3: List[String] = List( "org.springframework.web.bind.annotation.ExceptionHandler", "io.shiftleft.model.Constants$Type",
"java.lang.Enum",
"io.shiftleft.model.Constants$Type[]", "io.shiftleft.model.Constants",
"char",
"short",
"float",
"android.os.AsyncTask",
"null_type",
"io.shiftleft.model.Customer[]",
"java.lang.Long[]",
"io.shiftleft.model.Patient[]",
"bottom_type",
..
..
)
```

## Listing all the External Packages Used in the Code

The query returns a sorted distinct list (`l`) of the name of all the external namespace type declarations (`typeDecl`) in the CPG.

```
ocular> cpg.typeDecl.external.namespace.name.l.distinct.sorted
res4: List[String] = List( "com.ulisesbocchio.jasyptspringboot.annotation", "java.io",
"java.lang",
"java.lang.invoke",
"java.lang.ref",
"java.nio.charset",
"java.nio.file",
"java.util",
"java.util.concurrent",
"javax.annotation",
"javax.persistence",
"javax.servlet",
"javax.servlet.http", "org.apache.commons.codec.digest", "org.apache.http",
"org.apache.http.auth", "org.apache.http.client",
..
..
)
```

## Filtering the List of Type Declarations Used in the Code

The query returns a list (`l`) of the name of all the type declarations (`typeDecl`) in the CPG containing Http in their 
names.

```
ocular> cpg.typeDecl.name(".*Http.*").name.l
res5: List[String] = List(
  "HttpServletResponse",
  "HttpServletRequest",
  "HttpSession",
  "HttpStatus",
  "CloseableHttpClient",
  "HttpClients",
  "HttpPost",
  "HttpEntity",
  "HttpRequest",
  "HttpContext",
  "HttpUriRequest",
  "CloseableHttpResponse"
)
```
