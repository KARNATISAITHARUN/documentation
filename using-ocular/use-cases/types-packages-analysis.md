# Analyzing Types and Packages

This tutorial shows you how to use ShiftLeft Ocular to analyze types and packages. It uses the [HelloShiftLeft](../../introduction/helloshiftleft.md) sample application to illustrate the steps and results.

The various types (classes) and packages used by the HelloShiftLeft application, including the methods which use or call them,  can be analyzed using ShiftLeft Ocular.

## Listing all the Types Used in the Code

The query returns a list (`l`) of the full name of all the type ceclarations (`typeDecl`) in the CPG.

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

## Listing all the External Packages Using in the Code

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

## Filting the List of Packages Used in the Code

The query returns a list (`l`) of the name of all the type declarations (`typeDecl`) in the CPG Containing Http in their 
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
