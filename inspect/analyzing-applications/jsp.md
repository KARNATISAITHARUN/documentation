# How to Analyze JavaServer Pages (JSP) Applications Using Inspect

This article shows you how to analyze your applications that are written in JavaServer Pages (JSP). It assumes that you have already [installed and authenticated](/inspect/getting-started/README.md) with ShiftLeft.

## Analyzing Your JavaServer Pages (JSP) Application

To analyze your JSP application, run:

```java
sl analyze --app <name> --java [<path>]
```

| Parameter | Description |
| - | - |
| `--app <name>` | The name of the application to be analyzed |
| `--java` | The flag identifying the application's language |
| `<path>` | The location of the application's `.jsp` file to be analyzed |
